<div dir="rtl">بنام خدا</div><br/>
<div dir="rtl">نحوه ساخت یک اتصال خصوصی مجازی یا همان وی-پی-ان</div><br/>
<div dir="rtl">کافیست مراحل زیر را بترتیب دنبال نمایید</div><br/>
Setting up a VPN,please follow below steps<br/>
1-#yum install pptp -y<br/>
2-#modprobe nf_conntrack_pptp<br/>
echo 'VPN.UserName PPTP VPN.Password *' >> /etc/ppp/chap-secrets
Create VPN.conf File and add six below lines: #vim /etc/ppp/peers/VPN.conf<br/>
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
