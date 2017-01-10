<div dir="rtl">بنام خدا</div>
######## Top
-[Packet Traveling](#packet-routing)

#### Packet Traveling
1. Tables:
 - *PREROUTING*:left_right_arrow:**NF_IP_PRE_ROUTING** hook<font size="2"> will be triggered by any incoming traffic very soon after entering the network stack. This hook is processed before any routing decisions have been made</font>
 - INPUT: Triggered by the NF_IP_LOCAL_IN hook.
 - FORWARD: Triggered by the NF_IP_FORWARD hook.
 - OUTPUT: Triggered by the NF_IP_LOCAL_OUT hook.
 - POSTROUTING: Triggered by the NF_IP_POST_ROUTING hook.

Tables:arrow_down:/Chains:arrow_right:|**PREROUTING**|**INPUT**|**FORWARD**|**OUTPUT**|**POSTROUTING**
:---:|:---:|:---:|:---:|:---:|:---:|
(routing decision)||||:white_check_mark:||
raw|:white_check_mark:|||:white_check_mark:||
(Conntrack enabled)|:white_check_mark:|||:white_check_mark:||
mangle|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|:white_check_mark:|
nat(DNAT)|:white_check_mark:|||:white_check_mark:||
(routing decision)|:white_check_mark:|||:white_check_mark:||
filter||:white_check_mark:|:white_check_mark:|:white_check_mark:||
security||:white_check_mark:|:white_check_mark:|:white_check_mark:||
nat(SNAT)||:white_check_mark:|||:white_check_mark:|

- Privent Ports Scan
```vim
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  iptables -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```
- Logs: Followed by a level number or name. Valid names are (case-insensitive) __debug__, __info__, __notice__, __warning__, __err__,
        __crit__, __alert__ and __emerg__, corresponding to numbers 7 through 0
  - to change log file save name 
    1. 
    ```vim
      echo "kern.warning /var/log/iptables.log">>/etc/syslog.conf
    ```
    2. 
    ```vim
      service rsyslog restart
    ```
  - simple : `iptables -A INPUT -j LOG`
  - next example : `iptables -A INPUT -s a.a.a.a -j LOG --log-prefix 'Source a.a.a.a **' --log-level 6`
  - to prevent flooding log file, limit log line per 5 minutes for 7 bursts
  ```vim
    iptables -A INPUT -s a.a.a.a -m limit --limit 5/m --limit-burst 7 -j LOG --log-prefix \
    'Source a.a.a.a **' --log-level 6
  ```
  - after logging you should tell iptables to what does do: `iptables -A INPUT -s a.a.a.a -j DROP`
 - connlimit:
  - limit number of ssh connection:
  ```vim
    iptables -A INPUT -p tcp --syn --dport 22 -m connlimit --connlimit-above 3 -j REJECT
  ```



<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
