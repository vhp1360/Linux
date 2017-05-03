<div dir="rtl">بنام خدا</div><br/>

<div dir="rtl">این متن در مورد بهینه سازی حافظه تصادفی سیستم می باشد</div><br/>

<div dir="rtl">ابتدا می خواهیم گزارشی کامل از مقدار حافظه تصادی داشته باشیم</div><br/>

_First_ *find out memory size complete*<br/>
`free -whlt`

name|total|used|free|shared|buffers|cache|available
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:
Mem:|7.6G|5.4G|663M|2.7M|3.0M|1.6G|2.0G        
Low:|7.6G|7.0G|663M
High:|0B|0B|0B
Swap:|3.9G|534M|3.4G
Total:|11G|5.9G|4.0G

<div dir="rtl">دراین قسمت مقدار Page Cache سیستم را تغییر می دهیم</div><br/>
_Changing_ *Page cache size*:

Page Cache:
```vim
  echo vm.min_free_kbytes=1024 >> /etc/sysctl.conf
```
<div dir="rtl">حال نوبت به حافظه جایگزین می رسد</div><br/>
_Now_ *swap*:

_swappiness_ value:
`echo vm.swappiness=0 >> /etc/sysctl.conf`

<div dir="rtl">مقدار حافظه ای که برای فایل های کلان درنظر گرفته شده است</div><br/>
_There_ *is a setting to keep Huge File Pasge Size Cache*:<br/>
_Number_ of Huge Pages:<br/>
```vim
  echo "vm.nr_hugepages=512" >> /etc/sysctl.conf --> to check it: grep Hugepagesize /proc/meminfo
```

<div dir="rtl">چه تعداد فایل بتواند بطور همزمان در سیستم عامل باز باشد</div><br/>
_Number_ *of open Files*<br/>
ulimite -n 8096<br/>

<div dir="rtl">تنظیم مقدار فضای تصادفی جهت ذخیره اطلاعات  معروف به صفحات کثیف</div><br/>
<div dir="rtl">ابتدا گزارش کلی از مقادیر تخصیص داده شده را بدست می آوریم</div><br/>
_First_ report of Dirty Page<br/>
_Limit_ *page cache dirty bytes*:<br/>
```vim
  sysctl vm.dirtyratio=percentage # Writeout of dirty data when riched to this value, \
                                        recommended a slightly lowerthan of 15
  sysctl vm.dirtybackgroundratio=percentage # Writeout of dirty data in the background when ... ., \
                                              For database recommended a lower value of 3
  sysctk vm.dirtyexpirecentisecs=Sec.No # hundredths of a second dirty data remains in the page cache, \
                                          does not recommend tuning this parameter
  sysctl vm.dirtywritebackcentisecs=length of the interval between kernel flusher threads waking and \
                                    writing eligible data to disk, _does not recommend tuning this parameter
```
- <div dir="rtl">مراحل زیر مستقل از موارد فوق می باشد و با آنها می توان تمامی حافظه های سریع را ابتدا در حافظه پایدار نوشت و سپس خالی کرد</div>
- _Free_ out RAM:
```vim
  sync;echo 1 > /proc/sys/vm/drop_caches # free all page caches memory
  sync;echo 2 > /proc/sys/vm/drop_caches # free all unused slab caches memory
  sync;echo 3 > /proc/sys/vm/drop_caches # 1,2
  swapoff -a && swapon -a                # free all swap data
```
. some other command:
  . `vmstate -s`
  . `iostate`
  





