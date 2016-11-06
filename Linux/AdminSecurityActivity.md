<div dir="rtl">بنام خدا</div>

- [Who Login](#who-login)
- [Some Logs](#some-logs)
- [Some Commands](#some-commands)
- [Audit Issues](#audit-issues)
- [Certified Issue](#
- [SeLinux](#selinux)
- [Fail2Ban](#fail2ban)
- [Are you Under Attack](#Are-you-under-attack)

#### Who Login
- who: who connected.
- w: who are connected and what is last command
- last: last commands for each users
- lastlog: 
- cat /var/log/messages

#### Some Commands
- psacct
 - `ac -d -p username`
 - `sa -u -m -c`
 - `lastcome username ls`
- `find` command:
 - find and show contain, which text file modified in last 8 days -> `find /home/you -iname "*.txt" -mtime -8 -exec cat {} \; `
 - count of above -> `find /home/you -iname "*.txt" -mtime -8 | wc -l`
 - files last accessed in 8 days ago -> ` find /home/you -iname "*.pdf" -atime -8 -type -f `
- netstate :
 - `netstat -tulpn`
 
- LSOF:
 - list of Established Connection : lsof -i TCP:80 | grep ESTABLISHED` or `watch "lsof -i TCP:80"`
 
 
#### Audit Issues
- Configuring Audit:
 - num_logs=No.
 - max_log_file = No. --> in MB
 - max_log_file_action = ROTATE
 - log contain: for `auditctl -w /etc/ssh/sshd_config -p rwxa -k sshconfigchange` we'd find below record in log file

    type=SYSCALL,msg=audit(1434371271.277:135496):,arch=c000003e(cpu info),
    syscall=2(use `ausyscall 2` to unserstand),success=yes,ppid=6265,pid=6266,auid=1000,
    uid=0,comm="cat",exe="/usr/bin/cat",key="sshconfigchange"
 - `ausearch -m LOGIN --start today -i`
 - `ausearch -a 27020`
 - `ausearch -f /etc/ssh/sshd_config -i`
 - `aureport -x --summary`
 - `aureport --failed`
 
- remove Unnecessary packages: `yum erase inetd xinetd ypserv tftp-server telnet-server rsh-serve`
- Pawword Policy
 - User Accounts and Strong Password Policy:
  - Password Aging with `chage` command. Like `chage -M 60 -m 7 -W 7 userName`
  - use >/etc/login.defs
  - in ;/etc/shadow; file: {userName}:{password}:{lastpasswdchanged}:{Minimum_days}:{Maximum_days}:{Warn}:{Inactive}:{Expire}:
 - Restricting Use of Previous Passwords:
 - Locking User Accounts After Login Failures:
 - How Do I Verify No Accounts Have Empty Passwords?: `awk -F: '($2 == "") {print}' /etc/shadow` and Lock them `passwd -l UserName`
 - Make Sure No Non-Root Accounts Have UID Set To 0 : `awk -F: '($3 == "0") {print}' /etc/passwd`
 

#### Certified Issue
- generate Cert
 - 1: openssl genrsa -des3 -out server.key 1024
 - 2: openssl req -new -key server.key -out server.csr
 - 3: you may need remove pass phrase -> cp server.key server.key.org && openssl rsa -in server.key.org -out server.key
 - 4: openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

> _also we could generate them in one line_

`openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout server.key -out server.crt`


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
    - chcon -v --type=httpd_sys_content_t /html
    - chcon -Rv --type=httpd_sys_content_t /html --> recursively
  - restorecon :
    - restorecon -Rv /var/www/html
    - restorecon -Rv -n /var/www/html --> only show the default
    - touch /.autorelabel --> to automaticaly relabled FileSystem after reboot
  - semanage:
    - open port : `semanage port -a -t http_port_t -p tcp 81`
    - Check Port is reserved : `semanage port -l`
    - add new labled for futurs files will be coming: `semanage fcontext -a -t httpd_sys_content_t "/html(/.\*)?"`
  - Create SeModule:
    - Way 1:
      1. grep smtpd_t /var/log/audit/audit.log | audit2allow -m postgreylocal > postgreylocal.te && cat postgreylocal.te
      2. grep smtpd_t /var/log/audit/audit.log | audit2allow -M postgreylocal 
      3. semodule -i postgreylocal.pp 
    - Way 2:
      1. grep postdrop /var/log/audit/audit.log | audit2allow -M postfixlocal
      2. cat postfixlocal.te
      3. in .te file -> dontaudit postfix_postdrop_t httpd_log_t:file getattr; 
      4. checkmodule -M -m -o postfixlocal.mod postfixlocal.te
      5. semodule_package -o postfixlocal.pp -m postfixlocal.mod
      6. semodule -i postfixlocal.pp 
  - getsebool -a
  - seinfo --portcon=80
  - setsebool -P BooleanParameter 1
  
  
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

#### Are you Under Attack
- DDoS Attack : `netstat -anp |grep 'tcp\|udp' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort`
- DDoS on Port : netstat -n | grep :80 |wc -l
- on Packet Type : netstat -n | grep :80 | grep SYN |wc -l
- Under ICMP : `while :; do netstat -s| grep -i icmp | egrep 'received|sent' ; sleep 1; done`
- Under a SYN flood : netstat -nap | grep SYN

<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
