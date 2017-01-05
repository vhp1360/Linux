<div dir="rtl">بنام خدا</div>
##### top

- [Jobs](#jobs)
- [Users](#users)
- [IPV6](#ipv6)
- [VPN Routes](#vpn-routes)
- [Partitioning](#partitioning)
- [NetWork](#network)

##### Jobs
- \& :
- fg :
- bg :
- disown :
  `run command`,<kbd>Ctrl</kbd><kbd>Z</kbd>,`disown -h Job-ID`

[Top](#top)
##### Users
* Change username:
 - becare first find OldName user and group ID.
```vim
  usermod -l NewName OldName
  groupmod -n NewName oldName
  usermod -d /home/NewName -m NewName
  find / -group OldGroupID -exec chgrp -h NewName {} \;
  find / -user OldUserID -exec chown -h Newname {} \;
```
* useradd
```vim
  -e Account Expir Date
  -f Password Expire
  -M without Home Directory
  -d Specified Home Directory
  -N without Group
  -s specified shell
  -L without loggin permission
```

[Top](#top)
##### IPv6
* disabling IPv6
```vim
  vim /etc/sysctl.conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
  sysctl -p
```


[Top](#top)
##### VPN Routes
* [coonect client to server which will connected to vpn:](http://unix.stackexchange.com/questions/237460/ssh-into-a-server-which-is-connected-to-a-vpn-service)
 + Public IP is 50.1.2.3
 +  Public IP Subnet is 50.1.2.0/24
 +  Default Gateway is x.x.x.1
 +  eth0 is device to gateway
```vim
  ip rule add table 128 from 50.1.2.3
  ip route add table 128 to 50.1.2.0/24 dev eth0
  ip route add table 128 default via x.x.x.1
```
* Scans
```vim
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --set
  IPTABLES -A INPUT -p tcp -i eth0 -m state --state NEW -m recent --update --seconds 30 --hitcount 10 -j DROP
```

[Top](#top)
### Partitioning
- [Parted](#parted)


###### Parted
- add partition to existing free space(may raid) :
  1- `# parted` --> so you intered in (parted) mode.
  2- `select /dev/md100` --> inside of disk.
  3- `unit s print free` --> give fisrt and last sector number for free unpatitioned space on disk.
  4- `mkpart`  --> and go ahead and answer questions.
  5- `quit` --> goback to __bash__ mode.
  6- `mkfs. ... NewPartitionName`
  

[Top](#top)
#### NetWork
- ncat: to send data on machine port and listening.
  1. listening: `nc -l PortNo.`
  2. sending: `cat File | nc IP PortNo.`
- wget: to download
  1. simple code: `wget http:/.... or ftp://... `
  2. Username&Pass: 
  ```vim
    wget http://userName:Password@Address
    wget --http-user=User --http-password=Password http://...
    wget --ftp-user=User --ftp-password=Password ftp://...
  ```
  3. Recursively of Web: `wget -r http://... or ftp://... `
  4. Resumable : `wget -c http://... or ftp://... `
  5. From Files: `wget -i /Path to Text File `

[Top](#top)
<div dir="rtl"></div>
