<div dir="rtl">بنام خدا</div>
######## Top
- [Packet Traveling](#packet-traveling)
- [NetWork Commands](#network-commands)


#### Packet Traveling
[return to this article](https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecture)
1. Tables:
 - *PREROUTING*:left_right_arrow:**NF_IP_PRE_ROUTING** hook
   
   will be triggered by any incoming traffic very soon after entering the network stack. This hook is processed before any routing decisions have been made
 - *INPUT*:left_right_arrow:**NF_IP_LOCAL_IN** hook
   
   is triggered after an incoming packet has been routed if the packet is destined for the local system.
 - FORWARD:left_right_arrow:**NF_IP_FORWARD** hook
    
    is triggered after an incoming packet has been routed if the packet is to be forwarded to another host.
 - OUTPUT:left_right_arrow:**NF_IP_LOCAL_OUT** hook
    
    is triggered by any locally created outbound traffic as soon it hits the network stack.
 - POSTROUTING:left_right_arrow:**NF_IP_POST_ROUTING** hook
    
    is triggered by any outgoing or forwarded traffic after routing has taken place and just before being put out on the wire.

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
  - Incoming packets destined for the local system: **PREROUTING** :arrow_right: **INPUT**
  - Incoming packets destined to another host: **PREROUTING** :arrow_right: **FORWARD** :arrow_right: **POSTROUTING**
  - Locally generated packets: **OUTPUT** :arrow_right: **POSTROUTING**

   so, an _incoming_ packet destined for the _local_ system will first be evaluated against the **PREROUTING** chains of the **_raw, mangle_**, and **_nat_** tables. It will then traverse the **INPUT** chains of the **_mangle, filter, security_**, and **_nat_** tables before finally being delivered to the local socket.  
- IPTables Rules: Contain Two Parts
  * Matching: Clear
  * Targets: Tow disicion
      1- Terminating:
      2- Marking for Next:
- _**contrack**_ : provides stateful option to track a packet, but what are states?
  * Available States:
      - NEW: When a packet arrives that is not associated with an existing connection, but is not invalid as a first
      - ESTABLISHED: A connection is changed from NEW to ESTABLISHED when it receives a valid response  
      For **TCP**:arrow_right:means a _SYN/ACK_,for **UDP & ICMP**:arrow_right:means a response where source and destination of the original packet are switched.
      - RELATED: Packets that are not part of an existing connection, but are associated with a connection already in the system.This could mean a helper connection, as is the case with FTP data transmission connections, or it could be ICMP responses to connection attempts by other protocols.
      - INVALID: Packets can be marked INVALID if they are not associated with an existing connection and aren't appropriate for opening a new connection, if they cannot be identified, or if they aren't routable among other reasons.
      - UNTRACKED: Packets can be marked as UNTRACKED if they've been targeted in a raw table chain to bypass tracking.
      - SNAT: A virtual state set when the source address has been altered by NAT operations. This is used by the connection tracking system so that it knows to change the source addresses back in reply packets.
      - DNAT: A virtual state set when the destination address has been altered by NAT operations. This is used by the connection tracking system so that it knows to change the destination address back when routing reply packets.



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
[top](#top)
###NetWork Commands
- NetCat
  - scan ports: `nc -zv IP 1-56555`
  - listen to the port : `nc -lk IP port`
  - live Send to the port: nc -n IP port
  - Send file: `nc IP Port < File`
  - Save reciver in File: `nc -l Port > File`
  - make simple server: `while true; do nc -l 8888 < index.html; done` & simply browse with `http://localhost:8888`
  - zip and senf file : `dd if=/dev/sda | gzip -9 | nc -l 33`&untar in reciever: `nc localhost 3333 | tar -zf -`
  - security connect to port: 
  ```go
    ssh -f -L 233:127.0.0.1:33 me@IP sleep 10;nc localhost 233 | pv -b > backup.iso
  ```

[top](#top)
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
