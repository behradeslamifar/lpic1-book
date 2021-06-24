---
layout: article
chapter: نصب لینوکس و مدیریت بسته ها
order: 6.4
title: مدیریت بسته‌های نرم افزاری در دبیان
---

###نصب لینوکس و مدیریت بسته

####انواع بسته های نرم افزاری

بسته های نرم افزاری دو دسته هستند:

بسته های باینری و بسته های کد منبع که تفاوت آن ها این است که بسته های کد منبع هنوز به زبان ماشین(صفر و یک) کامپایل (ترجمه) نشده اند ولی بسته های باینری تبدیل به زبان ماشین شده اند.برای نصب کد های منبع ابتدا باید آنها را به زبان ماشین ترجمه کنید و در نهایت آنها را نصب کنید اما بسته های باینری با دوبار کلیک رویشان نصب خواهند شد. موضوع پر اهمیت دیگر مفهوم وابستگی های نرم افزاری است. هر بسته نرم افزاری مجموعه ای از کد ها و کتابخانه هایی است که در کنار هم نرم افزار به درستی کار می کند. پس چیزی که در مورد هر دو بسته اهمیت دارد این است که از قبل باید تمام پیش نیاز ها روی سیستم موجود باشند. 

بسته های باینری شامل بسته هایی با پسوند rpm,deb,run و بسته های کد منبع بسته هایی با پسوند tar.gz,tar.bz2,tgz است.
 
مثال:

*<div align="left" dir="ltr">‌‌Binary</div>*

```
  $ ls Downloads
  google-chrome-stable_current_amd64.deb
  google-chrome-stable_current_amd64.rpm
  NVIDIA-Linux-x86_64-340.32.run
  firefox-31.0-32bit.tar.bz2
  thunderbird-17.0.5.tar.bz2
```


*<div align="left" dir="ltr">‌‌Source Code</div>*

```
  #ls /usr/src/
  linux-patch-3.2-rt.patch.bz2
  linux-source-3.2.tar.bz2
  postfix-2.9.3.tar.gz
  pidgin-2.10.9.tgz
```



#### مفاهیم اولیه

Dependencies:به نرم افزارهای و کتابخانه هایی که یک نرم افزار برای اجرای مناسب به آن ها نیاز دارد و در صورت نصب نبودن آن ها امکان نصب نرم افزار مورد نظر هم نیستوابستگی های سیستم می گویند.در مواقعی که امکان نصب هم زمان دو بسته در یک سیستم وجود نداشته باشد اصطلاحا این دو بسته تداخل دارند.


####همخوانی بسته ها
 
در حالت ایده آل هیچ مشکلی برای استفاده یک بسته rpm یا deb در نسخه های مختلف لینوکس که بر مبنای rpm و deb هستند وجود ندارد ولی به دلایلی این موضوع در دنیای واقعی امکان پذیر نیست از جمله:

- نام گذاری های مختلف بسته ها در توزیع های مختلف

- موجود نبودن وابستگی های مورد نیاز یک بسته و یا قدیمی یا جدید بودن نسخه های موجود

- وجود اسکریپت های نصب مختص به یک توزیع مشخص در یک بسته

- عدم همخوانی نسخه rpm یا dpkg مورد استفاده در تولید بسته و نسخه مورد استفاده در نصب

###نحوه استفاده از مدیریت بسته ها در دبیان

####ساختار نام گذاری بسته deb

نام گذاری یک بسته به صورت زیر انجام می شود: 

```

  foo_VVV_RRR_AAA.deb 
```

- بخش اول:قسمت VVV‌ شماره نسخه ای است که نویسنده برنامه به آن نسبت داده شده یا همان شماره نسخه برنامه اصلی است.

- بخش دوم:قسمت RRR شماره ساخت یا build number یا همان ورژن است.این شماره تعداد دفعات کامپایل مجدد برنامه با پیکربندی های متفاوت است و تغییرات و patch های انجام شده توسط نگهدارنده یا maintainer بسته است.

- بخش سوم:‌قسمت AAA همان بستر سخت افزاری است که بسته برای آن کامپایل شده است.

دستور dpkg

 این دستور مدیریت بسته ها را به عهده دارد و نحوه استفاده از آن به صورت زیر است:

```
  # dpkg [ -iEGRlLsSr] <packagename.deb>
``` 

این دستور را میتوان با گزینه های زیر استفاده کرد:



<table align="center" border="1" style="text-align:center">
<tr> <td> گزینه</td> <td> توضیحات</td> </tr>
<tr> <td>  <span dir="ltr"> -i</span> </td> <td> نصب یک بسته </td> </tr>
<tr> <td> <span dir="ltr"> -E</span></td> <td> در صورت برابر بودن با نسخه قبلی بسته نصب شده قبلی را باز نویسی نمی کند </td> </tr>
<tr> <td>  <span dir="ltr"> -G</span> </td> <td> اگر نسخه موجود جدید تر باشد نصب انجام نمی شود  </td> </tr>
<tr> <td>  <span dir="ltr"> -R</span> </td> <td> به همراه <span dir="ltr"> -iو --unpack</span>زیر شاخه ها به صورت بازگشتی می شوند </td></td>  </td> </tr>
<tr> <td> <span dir="ltr"> -l [pattern]</span> </td> </td> <td> اطلاعات وضعیت یک بسته را نمایش می دهد. </td> </tr>
<tr> <td><span dir="ltr"> -L package</span> </td></td> <td> لیست فایل هایی که یک بسته نصب شده می سازد را نمایش می دهد. </td> </tr>
<tr> <td><span dir="ltr"> -s یا --status</span></td> <td> اطلاعات مربوط به یک بسته را نمایش میدهد همچنین فایل های پیکربندی را نیز لیست می کند.</td> </tr>
<tr> <td><span dir="ltr"> -- purge</span></td> <td>  بسته را به همراه فایل های پیکربندی آن پاک می کند. </td> </tr>
<tr> <td><span dir="ltr"> -r یا‌ --remove</span></td> <td> همه چیز را به غیر از فایل های پیکربندی پاک می کند </td> </tr>
<tr> <td><span dir="ltr"> -S</span></td> <td>  بسته مالک یک فایل را پیدا می کند. </td> </tr>
</table>

مثال:
پیدا کردن بسته مربوط به دستور ls

```
  $ dpkg -S /bin/ls
  coreutils: /bin/ls
```
 
اطلاعات مربوط به وضعیت بسته در /var/lib/dpkg/status


```
  $ dpkg -l dosfstools
  Desired=UNknown/Install/Remove/Purge/Hold
  | Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
  |/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
  ||/ Name	Version		Description
  +++-==============-==============-
  ======================================
  ii dosfstools		3.0.9-1		utilities for making and checking MS-DOS FAT
```

```
  $ dpkg -i virtualbox-4.3_4.3.6-91406~Debian~wheezy_amd64.deb
```

####فایل های مربوط به apt

- پوشه ای شامل فایل های پیکر بندی مرتبط با apt

```
  /etc/apt/apt.conf
  /etc/apt/apt.conf.d/
```

- لیست مخازن

مخازن سرویس دهنده هایی هستند که بسته های مورد نظر در ساختار از پیش تعریف شده برای استفاده توسط نرم افزار مدیر بسته ها بر روی شبکه و اینترنت در اختیار قرار می دهند.

```
  /etc/apt/source.list
  /etc/apt/source.list.d/
```

- محل ذخیره سازی فایل های دریافت شده از مخازن

```
  /var/cache/apt/archives/
```

- لیست فایل های دریافت شده از مخازن

```
  /var/lib/apt/lists/
```

برای مشاهده لیست مخازن از دستور زیر استفاده می کنیم:

```
  deb uri distribution [component1] [component2] [...]
```

در زیر نمونه هایی از آدرس دهی مخازن در فایل sources.list آورده شده است:

```
  deb file:/home/lpi/debian stable main contrib non-free
  deb http://ftp.de.debian.org/debian squeeze main contrib non-free
  deb ftp://ftp.debian.org/debian unstable contrib
```

برای اضافه کردن cdrom به لیست مخازن از دستور زیر استفاده می شود:

```
  # apt-cdrom add
  Using CD-ROM mount point /media/cdrom/
  Identifying... [c60d29055a06d05adcdb2ab75f3dd723-2]
  Scannind disc for index files...
```















