<div dir="rtl">بنام خدا</div><br/>

<div dir="rtl">این متن در مورد بهینه سازی حافظه تصادفی سیستم می باشد</div><br/>

<div dir="rtl">ابتدا می خواهیم گزارشی کامل از مقدار حافظه تصادی داشته باشیم</div><br/>

find out memory size complete<br/>

free -whlt

<table border="0" cellpadding="0" cellspacing="0">
<tr><td></td><td></td><td>total</td><td>used</td><td>free</td><td>shared</td><td>buffers</td><td>cache</td><td>available</td></tr>
<tr><td>Mem:</td><td></td><td>7.6G</td><td>5.4G</td><td>663M</td><td>2.7M</td><td>3.0M</td><td>1.6G</td><td>2.0G</td></tr>        
<tr><td>Low:</td><td></td><td>7.6G</td><td>7.0G</td><td>663M</td><td></td><td></td><td></td><td></td></tr>
<tr><td>High:</td><td></td><td>0B</td><td>0B</td><td>0B</td><td></td><td></td><td></td><td></td></tr>
<tr><td>Swap:</td><td></td><td>3.9G</td><td>534M</td><td>3.4G</td><td></td><td></td><td></td><td></td></tr>
<tr><td>Total:</td><td></td><td>11G</td><td>5.9G</td><td>4.0G</td><td></td><td></td><td></td><td></td></tr>
</table>

<div dir="rtl">دراین قسمت مقدار Page Cache سیستم را تغییر می دهیم</div><br/>
Page Cache:<br/>
echo vm.min_free_kbytes=1024 >> /etc/sysctl.conf<br/>

<div dir="rtl">حال نوبت به حافظه جایگزین می رسد</div><br/>
_swappiness_ value:<br/>
echo vm.swappiness=0 >> /etc/sysctl.conf<br/>

<div dir="rtl">مقدار حافظه ای که برای فایل های کلان درنظر گرفته شده است</div><br/>
_Number_ of Huge Pages:<br/>
echo "vm.nr_hugepages=512" >> /etc/sysctl.conf --> to check it: grep Hugepagesize /proc/meminfo<br/>

<div dir="rtl">چه تعداد فایل بتواند بطور همزمان در سیستم عامل باز باشد</div><br/>
_Number_ of open File<br/>
ulimite -n 8096<br/>

Limit page cache dirty bytes<br/>
_dirty_ratio_ : Defines a percentage value. Writeout of dirty data begins (via pdflush) when dirty data comprises this percentage of total system memory. The default value is 20.
    Red Hat recommends a slightly lower value of 15 for database workloads. 
dirty_background_ratio
    Defines a percentage value. Writeout of dirty data begins in the background (via pdflush) when dirty data comprises this percentage of total memory. The default value is 10. For database workloads, Red Hat recommends a lower value of 3. 
dirty_expire_centisecs
    Specifies the number of centiseconds (hundredths of a second) dirty data remains in the page cache before it is eligible to be written back to disk. Red Hat does not recommend tuning this parameter. 
dirty_writeback_centisecs
    Specifies the length of the interval between kernel flusher threads waking and writing eligible data to disk, in centiseconds (hundredths of a second). Setting this to 0 disables periodic write behavior. Red Hat does not recommend tuning this parameter. 


<div dir="rtl">مراحل زیر مستقل از موارد فوق می باشد و با آنها می توان تمامی حافظه های سریع را ابتدا در حافظه پایدار نوشت و سپس خالی کرد</div><br/>
_Free_ out RAM:<br/>
sync;echo 1 > /proc/sys/vm/drop_caches --> free all page caches memory<br/>
sync;echo 2 > /proc/sys/vm/drop_caches --> free all unused slab caches memory<br/>
sync;echo 3 > /proc/sys/vm/drop_caches --> 1,2<br/>
swapoff -a && swapon -a<br/>                --> free all swap data<br/>







