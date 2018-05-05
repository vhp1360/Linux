<div dir='rtl'>بنام خدا</div>

- [OpenVPN](#openvpn)
   - [Client Setup](#client-setup)
- [PPTPd](#pptpd)
- [OpenConnect](#openconnect)
- [Squid](squid)
- [Cautions](#cautions)

[Top](#top)
### OpenVPN
[according this](https://www.unixmen.com/install-openvpn-centos-7/):
1. `install openvpn easy-rsa`
2. server side file config:
   - `cp /usr/share/doc/openvpn-*/sample/sample-config-files/server.conf  /etc/openvpn`
   - in _/etc/openvpn/server.conf_ file uncomment below:
     - port 1194
     - proto tcp
     - dev tun
     - ca ca.crt
     - cert server.crt
     - key server.key
     - dh dh2048.pem
     - server 192.110.133.0 255.255.255.0 --> VPN network IP
     - ifconfig-pool-persist ipp.txt
     - push "route 192.110.133.0 255.255.255.0"
     - push "redirect-gateway def1"
     - push "dhcp-option DNS 8.8.8.8"
     - push "dhcp-option DNS 8.8.8.4"
     - keepalive 10 120
     - cipher AES-256-CBC
     - comp-lzo
     - user nobody
     - group nobody
     - persist-key
     - persist-tun
     - status openvpn-status.log
     - verb 3
3. generating Server Keys:
   ```go
     mkdir -p /etc/openvpn/easy-rsa/keys
     cp -rf /usr/share/easy-rsa/2.0/* /etc/openvpn/easy-rsa
   ```
   - check _/etc/openvpn/easy-rsa/vars_ file and change as needed:
     - uncomment export KEY_CN
   - Now
     ```vim
       cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf
       cd /etc/openvpn/easy-rsa
       source ./vars
       ./clean-all
       ./build-ca
       ./build-key-server server
       ./build-dh
     ```
4. generating Client Keys:
   ```vim
     cd /etc/openvpn/easy-rsa/keys
     cp dh2048.pem ca.crt server.crt server.key /etc/openvpn
     cd /etc/openvpn/easy-rsa
     ./build-key ClientName
   ```
5. Iptables Configs and ip forwarding:
   - in nat table `-A POSTROUTING -s 192.110.96.0/24 -o enp4s0 -j MASQUERADE`
   - in filter table:
     ```vim
       -A INPUT -p tcp -m tcp -m state --state NEW --dport 1194 -j ACCEPT
       -A FORWARD -i tun0 -o enp4s0 -j ACCEPT
       -A FORWARD -i enp4s0 -o tun0 -j ACCEPT
     ```
   - Now:
   ```go
     echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
     sysctl -p
     service network restart
   ```
     - becareful if you use NetworkManager
6. [add pam authentication](https://www.linuxsysadmintutorials.com/setup-pam-authentication-with-openvpns-auth-pam-module)
   - add `plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so openvpn` to _/etc/openvpn/server.conf_ file
   - create _/etc/pam.d/openvpn_ file and add below:
   ```go
     auth    required        pam_unix.so    shadow    nodelay
     account required        pam_unix.so
   ```  
7. add service to level and start
   - `chkconfig --level35 openvpn@server on`
   - `service openvpn@server start`
8. finally we should issue ca.crt  ClientName.crt  ClientName.csr  ClientName.key to each clients.
9. client config:
   - client
   - dev tun
   - proto tcp
   - remote 213.233.161.61 1194
   - resolv-retry infinite
   - nobind
   - persist-key
   - persist-tun
   - comp-lzo
   - verb 3
   - ca ./ca.crt
   - cert ./ClientName.crt
   - key ./ClientName.key
   - cipher AES-256-CBC
   - auth-user-pass
10. run `sudo openvpn --config ./openvpn.ovpn`

enjoy!

##### Client Setup
<div dir="rtl">نحوه ساخت یک اتصال خصوصی مجازی یا همان وی-پی-ان</div>
<div dir="rtl">کافیست مراحل زیر را بترتیب دنبال نمایید</div>

. Setting up a VPN,please follow below steps
  1. first:
  ```vala
    #yum install pptp -y
    #modprobe nf_conntrack_pptp
    #echo 'VPN.UserName PPTP VPN.Password *' >> /etc/ppp/chap-secrets
  ```
  2. Create VPN.conf File and add six below lines: `#vim /etc/ppp/peers/VPN.conf` and add below:
  ```vala
    pty "pptp 123.123.1.1 --nolaunchpppd"
    name VPN.UserName
    remotename PPTP
    require-mppe-128
    file /etc/ppp/options.pptp
    ipparam VPN.conf
  ```
  3. Connect to VPN: `pppd call VPN.conf`
  4. Check Connection: #ip a s or #ifconfig
  5. Disconnect: `#pkill pppd`

<div dir="rtl">درضمن ممکن است که نیاز به openconnectپیدا کنیم که برای این امر می بایست منبع مربوطه نصب شود.</div><br/>
Also we may need openconnect on CentOS so, we need and EPEL repo<br/>
#rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-7.noarch.rpm


[top](#top)
### Cautions
1. if you tried to run next vpn into existing VpnServer, you should provide NAT and FORWARDING isseus in iptables.

[top](#top)
### 

[top](#top)
### 

[Top](#top)
### 

