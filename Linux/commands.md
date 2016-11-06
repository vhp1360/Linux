<div dir="rtl">بنام خدا</div>
+ [Jobs](#jobs)
+ [Users](#users)
+ [IPV6](#ipv6)
+ [VPN Routes](#vpn-routes)


##### Jobs
- \& :
- fg :
- bg :
- disown :
  `run command`,<kbd>Ctrl</kbd><kbd>Z</kbd>,`disown -h Job-ID`


##### Users
* Change username
```
  usermod -l NewName OldName
  groupmod -n NewName oldName
  usermod -d /home/NewName -m NewName
```
* useradd
```
  -e Account Expir Date
  -f Password Expire
  -M without Home Directory
  -d Specified Home Directory
  -N without Group
  -s specified shell
  -L without loggin permission
```

##### IPv6
* disabling IPv6
```
  vim /etc/sysctl.conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
  sysctl -p
```


##### VPN Routes
* [coonect client to server which will connected to vpn:](http://unix.stackexchange.com/questions/237460/ssh-into-a-server-which-is-connected-to-a-vpn-service)
 + Public IP is 50.1.2.3
 +  Public IP Subnet is 50.1.2.0/24
 +  Default Gateway is x.x.x.1
 +  eth0 is device to gateway
```
  ip rule add table 128 from 50.1.2.3
  ip route add table 128 to 50.1.2.0/24 dev eth0
  ip route add table 128 default via x.x.x.1
```
* Scans
```
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```

<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
<div dir="rtl"></div>
