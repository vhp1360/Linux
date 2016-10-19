<div dir="rtl">بنام خدا</div>
- who: who connected.
- w: who are connected and what is last command
- last: last commands for each users
- lastlog: 
- cat /var/log/messages
- psacct
 - `ac -d -p username`
 - `sa -u -m -c`
 - `lastcome username ls`
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
- `find` command:
 - find and show contain, which text file modified in last 8 days -> `find /home/you -iname "*.txt" -mtime -8 -exec cat {} \; `
 - count of above -> `find /home/you -iname "*.txt" -mtime -8 | wc -l`
 - files last accessed in 8 days ago -> ` find /home/you -iname "*.pdf" -atime -8 -type -f `
- generate Cert
 - 1: openssl genrsa -des3 -out server.key 1024
 - 2: openssl req -new -key server.key -out server.csr
 - 3: you may need remove pass phrase -> cp server.key server.key.org && openssl rsa -in server.key.org -out server.key
 - 4: openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

- SeLinux
 - Note:
   + SELinux Access Control : 1-Type Enforcement (TE) 2-Role-Based Access Control (RBAC) 3-Multi-Level Security (MLS)
   + ls -Z --> user:role:type:mls
   + fundamental reason may seLinux prevent access to file or service or app:
     * a mislabled file
     * a process running under wrong selinux security context
     * a bug in policy.an app needs acesses to a file that wasn't anticipaited
     * an intrusion attempt
 - sestatus
 - 
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
