<div dir="rtl">بنام خدا</div>

###### top
- [Top Command](#top-command)
- [Free](#free)
- [Cache](#cache)
- [Limitaion](#limitation)
- [Free Cache](#free-cache)
- [Some Other Ram Monitoring Command](#some-other-ram-monitoring-command)

[top](#top)
### Top Command
```go
  top
  top -O ColumnName <- Order By
  top -u UserName <- Only this user
  top -c  <- Absolute Process Path
  top -d Secs   <- Set refresh interval
  top -n 10  <- Finish after 10 times refreshing
```
- Some Key
   - <kbd>Shift</kbd><kbd>p</kbd> : Sort on CPU
   - <kbd>Shift</kbd><kbd>o</kbd> : then choose column to sort
   - <kbd>z</kbd> : Coloring
   - <kbd>k</kbd> : kill process

[top](#top)
### Free

<div dir="rtl">این متن در مورد بهینه سازی حافظه تصادفی سیستم می باشد</div>

<div dir="rtl">ابتدا می خواهیم گزارشی کامل از مقدار حافظه تصادی داشته باشیم</div>

- First *find out memory size complete*
```go
  free -whlt <- w:Wide mode h:Human l:Low&Max t:Totaly s:Priodicly 
```
name|total|used|free|shared|buffers|cache|available
:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:
Mem:|7.6G|5.4G|663M|2.7M|3.0M|1.6G|2.0G        
Low:|7.6G|7.0G|663M
High:|0B|0B|0B
Swap:|3.9G|534M|3.4G
Total:|11G|5.9G|4.0G


[top](#top)
### Cache

<div dir="rtl">دراین قسمت مقدار Page Cache سیستم را تغییر می دهیم</div>

_Changing_ *Page cache size*:

Page Cache:
```go
  echo vm.min_free_kbytes=1024 >> /etc/sysctl.conf
```
<div dir="rtl">حال نوبت به حافظه جایگزین می رسد</div>

_Now_ *swap*:

_swappiness_ value: `echo vm.swappiness=0 >> /etc/sysctl.conf`

<div dir="rtl">مقدار حافظه ای که برای فایل های کلان درنظر گرفته شده است</div>

_There_ *is a setting to keep Huge File Pasge Size Cache*:

_Number_ of Huge Pages:

```go
  echo "vm.nr_hugepages=512" >> /etc/sysctl.conf --> to check it: grep Hugepagesize /proc/meminfo
```

[top](#top)
### Limitaion
<div dir="rtl">چه تعداد فایل بتواند بطور همزمان در سیستم عامل باز باشد</div>

_Number_ *of open Files*

ulimite -n 8096

<div dir="rtl">تنظیم مقدار فضای تصادفی جهت ذخیره اطلاعات  معروف به صفحات کثیف</div>

<div dir="rtl">ابتدا گزارش کلی از مقادیر تخصیص داده شده را بدست می آوریم</div>

_First_ report of Dirty Page

_Limit_ *page cache dirty bytes*:

```vala
  sysctl vm.dirtyratio=percentage # Writeout of dirty data when riched to this value, \
                                        recommended a slightly lowerthan of 15
  sysctl vm.dirtybackgroundratio=percentage # Writeout of dirty data in the background when ... ., \
                                              For database recommended a lower value of 3
  sysctk vm.dirtyexpirecentisecs=Sec.No # hundredths of a second dirty data remains in the page cache, \
                                          does not recommend tuning this parameter
  sysctl vm.dirtywritebackcentisecs=length of the interval between kernel flusher threads waking and \
                                    writing eligible data to disk, _does not recommend tuning this parameter
```
[top](#top)
### Free Cache

- <div dir="rtl">مراحل زیر مستقل از موارد فوق می باشد و با آنها می توان تمامی حافظه های سریع را ابتدا در حافظه پایدار نوشت و سپس خالی کرد</div>

- _Free_ out RAM:

```go
  sync;echo 1 > /proc/sys/vm/drop_caches # free all page caches memory
  sync;echo 2 > /proc/sys/vm/drop_caches # free all unused slab caches memory
  sync;echo 3 > /proc/sys/vm/drop_caches # 1,2
  swapoff -a && swapon -a                # free all swap data
```

[top](#top)
### Some Other Ram Monitoring Command
- vmstate :
```go
  vmstate -s
```
- iostate :
- meminfo:
```go
  cat /proc/meminfo
```
- Ram Information:
```go
  dmidecode -t 17
```



[top](#top)
### 


