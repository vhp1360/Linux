<div dir="rtl">بنام خدا</div>
#### Top

- [Who Login](#who-login)
- [Some Logs](#some-logs)
- [Some Commands](#some-commands)
- [Audit Issues](#audit-issues)
- [Certified Issue](#certified-issues)
- [SeLinux](#selinux)
- [Fail2Ban](#fail2ban)
- [Are you Under Attack](#are-you-under-attack)
- [SSH](#ssh)
- [Access List](#access-list)
- [Encrypt and Decrypt Files](#encrypt-and-decrypt-files)
  - [Gpg](#gpg)


[Top](#top)  
#### Who Login
- who: who connected.
- w: who are connected and what is last command
- last: last commands for each users
- lastlog: 
- cat /var/log/messages

[Top](#top)
#### Some Commands
- psacct
 ```
   ac -d -p username
   sa -u -m -c
   lastcome username ls
 ```vim
- `find` command:
 ```vim
   find /home/you -iname "*.txt" -mtime -8 -exec cat {} \; <-find and show contain, which text file modified in last 8 days
   find /home/you -iname "*.txt" -mtime -8 | wc -l <- count of above
   find /home/you -iname "*.pdf" -atime -8 -type -f <- files last accessed in 8 days ago
 ```
- netstate :
 - `netstat -tulpn`
 
- LSOF:
 - list of Established Connection : `lsof -i TCP:80 | grep ESTABLISHED` or `watch "lsof -i TCP:80"`
 
 
[Top](#top)
#### Audit Issues
- Configuring Audit:
 - num_logs=No.
 - max_log_file = No. --> in MB
 - max_log_file_action = ROTATE
 - log contain: for `auditctl -w /etc/ssh/sshd_config -p rwxa -k sshconfigchange` we'd find below record in log file

    type=SYSCALL,msg=audit(1434371271.277:135496):,arch=c000003e(cpu info),
    syscall=2(use `ausyscall 2` to unserstand),success=yes,ppid=6265,pid=6266,auid=1000,
    uid=0,comm="cat",exe="/usr/bin/cat",key="sshconfigchange"
 -
   ```vim
      ausearch -m LOGIN --start today -i
      ausearch -a 27020
      ausearch -f /etc/ssh/sshd_config -i
      aureport -x --summary
      aureport --failed
   ```
- remove Unnecessary packages: `yum erase inetd xinetd ypserv tftp-server telnet-server rsh-serve`
- Password Policy
 - User Accounts and Strong Password Policy:
  - Password Aging with `chage` command. Like `chage -M 60 -m 7 -W 7 userName`
  - use >/etc/login.defs
  - in ;/etc/shadow; file: {userName}:{password}:{lastpasswdchanged}:{Minimum_days}:{Maximum_days}:{Warn}:{Inactive}:{Expire}:
 - Restricting Use of Previous Passwords:
 - Locking User Accounts After Login Failures:
 - How Do I Verify No Accounts Have Empty Passwords?: `awk -F: '($2 == "") {print}' /etc/shadow` and Lock them `passwd -l UserName`
 - Make Sure No Non-Root Accounts Have UID Set To 0 : `awk -F: '($3 == "0") {print}' /etc/passwd`
 

[Top](#top)
#### Certified Issues
- generate Cert
  ```vim
    openssl genrsa -des3 -out server.key 1024
    openssl req -new -key server.key -out server.csr
    cp server.key server.key.org && openssl rsa -in server.key.org -out server.key <- you may need remove pass phrase
    openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
  ```
> _also we could generate them in one line_

```vim
  openssl req -x509 -nodes -sha256 -days 365 -newkey rsa:4096 -keyout Key.key -out Cert.crt
```
> [to check the security of certificate](https://shaaaaaaaaaaaaa.com/)

[Top](#top)
#### SeLinux
  - Note:
    + SELinux Access Control : 1-Type Enforcement (TE) 2-Role-Based Access Control (RBAC) 3-Multi-Level Security (MLS)
    + ls -Z --> user:role:type:mls
    + fundamental reason may seLinux prevent access to file or service or app:
      1. a mislabled file
      2. a process running under wrong selinux security context
      3. a bug in policy.an app needs acesses to a file that wasn't anticipaited
      4. an intrusion attempt
  - sestatus
  - sealert -a /var/log/audit/audit.log > /path/to/mylogfile.txt
  - chcon:
    ```vim
      chcon -v --type=httpd_sys_content_t /html
      chcon -Rv --type=httpd_sys_content_t /html <- recursively
    ```
  - restorecon :
    ```vim
      restorecon -Rv /var/www/html
      restorecon -Rv -n /var/www/html --> only show the default
      touch /.autorelabel <- to automaticaly relabled FileSystem after reboot
    ```
  - semanage:
    ```vim
      semanage port -a -t http_port_t -p tcp 81 <- open port
      semanage port -l <- Check Port is reserved
      semanage fcontext -a -t httpd_sys_content_t "/html(/.\*)?" <- add new labled for futurs files will be coming
    ```
  - Create SeModule:
    - Way 1:
      ```vim
        grep smtpd_t /var/log/audit/audit.log | audit2allow -m postgreylocal > postgreylocal.te && cat postgreylocal.te
        grep smtpd_t /var/log/audit/audit.log | audit2allow -M postgreylocal 
        semodule -i postgreylocal.pp
      ```
    - Way 2:
      ```vim
        grep postdrop /var/log/audit/audit.log | audit2allow -M postfixlocal
        cat postfixlocal.te
        dontaudit postfix_postdrop_t httpd_log_t:file getattr; <- in .te file
        checkmodule -M -m -o postfixlocal.mod postfixlocal.te
        semodule_package -o postfixlocal.pp -m postfixlocal.mod
        semodule -i postfixlocal.pp
      ```
  - getsebool -a
  - seinfo --portcon=80
  - setsebool -P BooleanParameter 1
  
  
[Top](#top)
#### Fail2Ban:
  - Default:bantime,maxretry,enabled,banaction,action
  - [ssh_d_]:filter,port,maxretry 
  - [...]: filter=[...]
- Check if you are under DDoS attack:`netstat -anp |grep 'tcp\|udp' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort –n`
- Conntrack : track connections
 - is module loaded : `modeprobe nf_conntrack_ipv4`
 - installation `yum install conntrack-tools libnetfilter_conntrack`
 - report `conntrack -L -C  -E -e NEW  -p tcp --state ESTABLISHED --dport 22`
 - there is contrack app as daemon with _conntrackd_ name that you could find it's info.
 - go out from jail : 
  1. `iptables -L -n`
  2. `fail2ban-client set YOURJAILNAMEHERE unbanip IPADDRESSHERE`
  
[Top](#top)
#### Are you Under Attack
- DDoS Attack : `netstat -anp |grep 'tcp\|udp' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort`
- DDoS on Port : netstat -n | grep :80 |wc -l
- on Packet Type : netstat -n | grep :80 | grep SYN |wc -l
- Under ICMP : `while :; do netstat -s| grep -i icmp | egrep 'received|sent' ; sleep 1; done`
- Under a SYN flood : netstat -nap | grep SYN

[Top](#top)
#### SSH
- sshd_config:
 - Port:
 - PermitRootLogin yes|no
 - AuthenticationMethods "publickey,password" "publickey,keyboard-interactive"
 - RSAAuthentication yes
 - PubkeyAuthentication yes
 - Specified Special Hosts: to make type of connect , Which Users Allowed , ... .
 ```vim
   Match Address IPAddress
     AlloewUsers UserName1 UserName2
     AuthenticationMethodes "publickey"|"authentication"
   Match User Name1,Name2 Address IP
     AuthenticationMethodes 
 ```
- Key:
 1. ssh-keygen -t RSA
 2. ssh-copy-id -i PathToSSHKey User@Server
- ssh_config
 - Special Host:
 ```vim
  Host Name
    HostName IP
    Port
  Host IP
    Port
 ```
- config: to specify which Host should use wich Key to connect
```vim
  Host HostName
    IdentityFile ~/.ssh/Host_Private_Key_Name
```
- Save Key Passphrase:
```vim
  ssh-agent
  ssh User@IP
  ssh-add /Path to Key
```
  > may you need ssh to destination before above commands.


[Top](#top)
#### Access List

the package is acl : `yum -y install acl`
1. get the Access List:
```vim
  getfacl /Path
```
the Output like
> getfacl: Removing leading '/' from absolute path names \

> \# file: Path

> \# owner: root
  
> \# group: root

> user::rwx

> user:cent:r--

> group::---

> mask::r--

> other::---

> default:user::rwx

> default:user:cent:r-x

> default:group::---

> default\:\mask\::r-x

> default:other::---

2. Set or Remove Acl:
```vim
  setfacl -m [u|g|o]:RelativeName:(r,w,x) /Path
  setfacl -x [u|g|o]:RelativeName:(r,w,x) /Path
  setfacl -b /Path
```
3. Set resursively:
```vim
  setfacl -R -m ...
```
4. Set or Remove Default:
```vim
  setfacl -d -R -m ...
  setfacl -k ...
```
5. Set from File: the content of file would be like the output of `getfacl`
```vim
  setfacl --restore=acl.txt
```

[Top](#top)
### Encrypt and Decrypt Files
- ###### Gpg: 
```vim
  gpg -c FileName -> Create Encrypted file with Passphrase
  gpg -d FileName.gpg -> Output of File in StdOut
  gpg -d FileName.gpg -o FileName -> SaveFile in Hard
```

[Top](#top)
#### 
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
