
<div dir='rtl' align='right'><b>بنام خدا</b></div>

<div dir='rtl' align='right'>نصب جاوا</div>

Java Installation
```linux
yum install java java-devel:-)
```
<div dir='rtl' align='right'>پس از نصب لازم است که به سیستم عامل خود بگویید از کدام جاواباید استفاده کند.چون معمولا بیش از یک جاوا برروی سیسنم عامل نصب است.</div>

Now,we need reconfigure OS setting to use which Java.
```bash
alternative --config java

alternative --config javac
```
if you did not see last version for javac, try below
```bash
yum search java | grep openjdk
```
<div dir='rtl' align='right'>حالا خطهای زیر را لازم است که در فایل پروفایل اضافه کنید</div>

add belows line in /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.91-0.b14.el7_2.x86_64(each any version that you have)

export JRE_HOME=$JAVA_HOME/jre

export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin

