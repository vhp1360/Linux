<div dir="rtl">بنام خدا</div><br/>
<div dir="rtl">نحوه ساخت یک اتصال خصوصی مجازی یا همان وی-پی-ان</div><br/>
<div dir="rtl">کافیست مراحل زیر را بترتیب دنبال نمایید</div><br/>
Setting up a VPN,please follow below steps<br/>
1-#yum install pptp -y<br/>
2-#modprobe nf_conntrack_pptp<br/>
3-#echo 'VPN.UserName PPTP VPN.Password *' >> /etc/ppp/chap-secrets<br/>
4-Create VPN.conf File and add six below lines: #vim /etc/ppp/peers/VPN.conf<br/>
>pty "pptp 123.123.1.1 --nolaunchpppd"<br/>
>name VPN.UserName<br/>
>remotename PPTP<br/>
>require-mppe-128<br/>
>file /etc/ppp/options.pptp<br/>
>ipparam VPN.conf<br/>
save file<br/>
Connect to VPN: pppd call VPN.conf<br/>
Check Connection: #ip a s or #ifconfig<br/>
Disconnect: #pkill pppd<br/>

<div dir="rtl">درضمن ممکن است که نیاز به openconnectپیدا کنیم که برای این امر می بایست منبع مربوطه نصب شود.</div><br/>
Also we may need openconnect on CentOS so, we need and EPEL repo<br/>
#rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-7.noarch.rpm
