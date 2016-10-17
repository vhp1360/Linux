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
 - files last accessed in 8 days ago -> `ind /home/you -iname "*.pdf" -atime -8 -type -f`
 

<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
