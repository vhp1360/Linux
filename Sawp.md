<div dir='rtl' align='right'>بنام خدا</div>
<div dir='rtl' align='right'></div>

<div dir='rtl' align='right'>افزودن حافظه جایگزین</div>
Adding Swap 

<div dir='rtl' align='right'>ابتدا یک فضا با اندازه مورد نظر ایجاد می کنیم،آنرا به نوع حافظه جایگزین تبدیل می کنیم، دسترسی های لازم را می دهیم</div>

frist create file ,so make it as swap type, grand acess to all users
```vim
  dd if=/dev/zero of=/swapfile bs=10M count=1024 --> consider 10G for swap :-)
  mkswap /swapfile
  chmod 600 /swapfile
```
<div dir='rtl' align='right'>حال باید آنرا تخصیص بدهیم.توجه شود که می توان درهمان حال که حافظه جایگزین موجود در حال استفاده است، می توان حافظه جدید را اضافه کرد</div>

Now, we could add new swap file to existing and running "swap"
```vim
  swapon /swapfile
```
<div dir='rtl' align='right'>می توان حافظه موجود را قطع کرد و بعد حافظه جدید یا هردو را اضافه کرد</div>

or we can stop "swap" and add ne or both.
```vim
  swapoff -a
  or
  swapon /swapfile /path/old/swap or swapon /swapfile
```
<div dir='rtl' align='right'>ترجیحا تنظیمات ذیل را در فایل مورد نظر اعمال نمایید تا این اقدامات بصورت خودکار انجام شود</div>

it is better to add below line to fstab file.
```vim
  vim /etc/fstab
  /sawpfile swap _fill like above line in file_
```
