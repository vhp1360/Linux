<div dir='rtl'>بنام خدا</div>

########Top

- [OpenVPN](#openvpn)
- [PPTPd](#pptpd)
- [OpenConnect](#openconnect)
- [Squid](squid)

#####OpenVPN
[according this](https://www.unixmen.com/install-openvpn-centos-7/):
1- `install openvpn easy-rsa`
2- server side file config:
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
3- generating Server Keys:
   - `mkdir -p /etc/openvpn/easy-rsa/keys`
   - `cp -rf /usr/share/easy-rsa/2.0/* /etc/openvpn/easy-rsa`
   - check _/etc/openvpn/easy-rsa/vars_ file and change as needed:
     - uncomment export KEY_CN
   - `cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf`
   - `cd /etc/openvpn/easy-rsa`
   - `source ./vars`
   - `./clean-all`
   - `./build-ca`
   - `./build-key-server server`
   - `./build-dh`
4- generating Client Keys:
   - `cd /etc/openvpn/easy-rsa/keys`
   - `cp dh2048.pem ca.crt server.crt server.key /etc/openvpn`
5- Iptables Configs:
   - in nat table `-A POSTROUTING -s 192.110.96.0/24 -o enp4s0 -j MASQUERADE`
   - in filter table:
     - `-A INPUT -p tcp -m tcp -m state --state NEW --dport 1194 -j ACCEPT`
     - `-A INPUT -i tun0 -j ACCEPT`
     - `-A FORWARD -i tun0 -o enp4s0 -j ACCEPT`
     - `-A FORWARD -i enp4s0 -o tun0 -j ACCEPT`
6- Enabling IP Forwarding:
   - `echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf`
   - `sysctl -p`
   - `service network restart` 
     - becareful if you use NetworkManager
7- add pam authentication
   - add `plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so openvpn` to _/etc/openvpn/server.conf_ file
   - create _/etc/pam.d/openvpn_ file and add below:
   ```go
     auth    required        pam_unix.so    shadow    nodelay
     account required        pam_unix.so
   ```  
7- add service to level and start
   - `chkconfig --level35 openvpn@server on`
   - `service openvpn@server start`
8- client config:
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
   - cert ./client.crt
   - key ./client.key
   - cipher AES-256-CBC
   - auth-user-pass
enjoy!
[Top](#top)