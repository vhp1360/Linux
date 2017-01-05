<div dir="rtl">بنام خدا</div>



- Privent Ports Scan
```bash
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```
- Logs: Followed by a level number or name. Valid names are (case-insensitive) __debug__, __info__, __notice__, __warning__, __err__,
        __crit__, __alert__ and __emerg__, corresponding to numbers 7 through 0
  - to change log file save name 
    1. `echo "kern.warning /var/log/iptables.log">>/etc/syslog.conf`
    2. `service rsyslog restart`
  - simple : `iptables -A INPUT -j LOG`
  - next example : `iptables -A INPUT -s a.a.a.a -j LOG --log-prefix 'Source a.a.a.a **' --log-level 6`
  - to prevent flooding log file, limit log line per 5 minutes for 7 bursts
  `iptables -A INPUT -s a.a.a.a -m limit --limit 5/m --limit-burst 7 -j LOG --log-prefix 'Source a.a.a.a **' --log-level 6`
  - after logging you should tell iptables to what does do: `iptables -A INPUT -s a.a.a.a -j DROP`
- connlimit:
  - limit number of ssh connection: `iptables -A INPUT -p tcp --syn --dport 22 -m connlimit --connlimit-above 3 -j REJECT`



<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
