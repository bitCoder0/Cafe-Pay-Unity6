README — رفع خطاهای Poolakey (نسخه 2.1.1) در Unity 6

درباره این پروژه
----------------
این پروژه یک نمونه تستی است که برای بررسی و رفع دو خطای مشخص
در زمان پیاده‌سازی SDK پرداخت درون‌برنامه‌ای Poolakey نسخه 2.1.1
در Unity 6.2.6f2 ایجاد شده است.

SDK رسمی مورد استفاده:  

https://github.com/cafebazaar/PoolakeyUnitySdk/releases/tag/2.1.1

در فرآیند پیاده‌سازی، ابتدا خطای  زمان بیلد duplicate classو پس از آن خطای Runtime مشاهده شد


سورس کد این پروژه ، 

منبع کد های صحنه اصلی بازی و رابط کاربری
-----------------

کد های موجود در سین این پروژه که برای ارتباط با اس دی کی بازار و تست پرداخت استفاده میشن و خود سین و رابط کاربریش برگرفته از پروژه زیر هست

https://github.com/Behnamjef/Bazaar-IAP-in-Unity

ویدئوی آموزشی مرتبط:
https://www.youtube.com/watch?v=7Yo_JS4_QEM

با تشکر از @Behnamjef
 

کد فعلی از روی مخزن بالا فورک شده و سپس نسخه پولکی به جدیدتر تغییر پیدا کرده و همچنین رفع خطای مربوط به پرداخت انجام شده

خطاهای مشاهده‌شده
-----------------

خطای اول (Build)
A failure occurred while executing
com.android.build.gradle.internal.tasks.CheckDuplicatesRunnable

خطای دوم (Runtime)
NoClassDefFoundError
برنامه کرش نمی‌کرد، اما پرداخت درون‌برنامه‌ای عمل نمی‌کرد.


توضیح رفع خطا
-------------
مشکل اصلی به ناسازگاری نسخه کتابخانه‌های Kotlin و وابستگی Poolakey بازمی‌گردد.
برای رفع، دو فایل زیر تغییر داده شدند.


فایل اول
Assets/Bazaar/Poolakey/Scripts/Editor/CafeBazaarPlugin_Dependencies.xml

--------

<dependencies>
  <androidPackages>

    <androidPackage spec="com.github.cafebazaar.Poolakey:poolakey:2.0.0">
      <repositories>
        <repository>https://jitpack.io</repository>
      </repositories>
    </androidPackage>

    <androidPackage spec="org.jetbrains.kotlin:kotlin-stdlib:1.9.10" />
    <androidPackage spec="org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.9.10" />
    <androidPackage spec="org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.9.10" />

  </androidPackages>
</dependencies>


فایل دوم
Assets/Plugins/Android/mainTemplate.gradle

```

apply plugin: 'com.android.library'
apply from: '../shared/keepUnitySymbols.gradle'
apply from: '../shared/common.gradle'

configurations.all {
    resolutionStrategy {
        force "org.jetbrains.kotlin:kotlin-stdlib:1.9.10"
        force "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.9.10"
        force "org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.9.10"
    }
}

dependencies {
    implementation 'com.github.cafebazaar.Poolakey:poolakey:2.0.0'
    implementation 'org.jetbrains.kotlin:kotlin-stdlib:1.9.10'
    implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.9.10'
    implementation 'org.jetbrains.kotlin:kotlin-stdlib-jdk8:1.9.10'
}
```

نکات مهم

- پروژه به‌صورت کامل تست نشده است.
- Minify را در Player Settings فعال نکنید.


سلب مسئولیت
-----------
این پروژه آزمایشی است و استفاده از آن به عهده توسعه‌دهنده می‌باشد.


برنامه‌های آتی
--------------
- تست مجدد پروژه
- بهبود توضیحات readme
- ساخت فایل unitypackage
