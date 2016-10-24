<div dir="rtl">بنام خدا</div>



- Scans
```
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```
- Logs: to change log file save name 1-echo "kern.warning /var/log/iptables.log">>/etc/syslog.conf, 2-service rsyslog restart
```
  iptables -A INPUT -j LOG
  iptables -A INPUT -s a.a.a.a -j LOG --log-prefix 'Source a.a.a.a **'
  
```




<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
