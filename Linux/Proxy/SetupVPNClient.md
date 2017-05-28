<div dir="rtl">بنام خدا</div>
<div dir="rtl">نحوه ساخت یک اتصال خصوصی مجازی یا همان وی-پی-ان</div>
<div dir="rtl">کافیست مراحل زیر را بترتیب دنبال نمایید</div>

. Setting up a VPN,please follow below steps
  1. first:
  ```
    #yum install pptp -y
    #modprobe nf_conntrack_pptp
    #echo 'VPN.UserName PPTP VPN.Password *' >> /etc/ppp/chap-secrets
  ```
  2. Create VPN.conf File and add six below lines: `#vim /etc/ppp/peers/VPN.conf` and add below:
  ```
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
