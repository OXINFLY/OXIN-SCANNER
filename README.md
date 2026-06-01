۱. لایه ۴ (Transport - TCP Handshake)

برقراری 3-Way Handshake روی پورت ۴۴۳
تست باز بودن پورت و امکان اتصال فیزیکی

۲. لایه ۶ (Session - TLS/SSL Handshake)

انجام Handshake رمزنگاری با SNI سفارشی
تأیید اینکه آی‌پی واقعاً پشت گواهی کلودفلر است

۳. لایه ۷ (Application - HTTP Request)

ارسال درخواست HEAD کوچک HTTP/1.1
بررسی پاسخ واقعی سرور و تشخیص Spoofing و DPI


⚡ بهینه‌سازی عملکرد روی گوشی‌های ضعیف
یکی از نقاط قوت OXIN SCANNER، عملکرد روان آن حتی روی دستگاه‌های پایین‌رده (مانند Samsung Galaxy A12) است:

State Buffering & Throttling: بروزرسانی UI هر ۳۵۰ میلی‌ثانیه به صورت دسته‌ای
LazyColumn Keying: استفاده از key = { it.ip } برای جلوگیری از recomposition غیرضروری
Log Chunking: محدود کردن نمایش لاگ به ۳۰ پیام آخر با takeLast(30)
استفاده از ConcurrentHashMap برای مدیریت ترد-safe نتایج

نتیجه: رابط کاربری buttery smooth حتی هنگام اسکن همزمان ۴۰–۵۰ آی‌پی.

🎨 هویت بصری و طراحی

آیکون رسمی حرفه‌ای با تم سایبرپانک (سپر کربنی + X کرومی + رادار نارنجی)
Adaptive Icon کاملاً سازگار با اندروید ۸ به بالا
تم تاریک عمیق با accents نئونی آبی و نارنجی
انیمیشن‌های fluid و مدرن با Jetpack Compose


🏗️ ساختار پروژه
#Bashapp/src/main/java/com/oxin/scanner/
#├── MainActivity.kt              # Single Activity + Compose UI
#├── ui/
#│   └── DashboardViewModel.kt    # منطق اصلی + State Management
#├── tester/
#│   └── IpTester.kt              # پیاده‌سازی سه لایه L4/L6/L7
#├── generator/
#│   └── CloudflareIpGenerator.kt # تولید هوشمند آی‌پی از CIDRهای کلودفلر
#├── data/
#│   └── local/                   # Room Database
#└── utils/
    #└── extensions/              # Extension functions و ابزارهای کمکی

✅ ویژگی‌های کلیدی

✅ اعتبارسنجی سه‌لایه (L4 + L6 + L7)
✅ پشتیبانی از SNI سفارشی
✅ تشخیص و حذف آی‌پی‌های Spoofed
✅ رابط کاربری مدرن و سریع با Jetpack Compose
✅ عملکرد عالی روی دستگاه‌های ضعیف
✅ ذخیره‌سازی محلی آی‌پی‌های تمیز با Room
✅ لاگ زنده و حرفه‌ای
✅ طراحی آیکون تطبیقی و تم سایبرپانک




  OXIN SCANNER — فراتر از یک اسکنر ساده

```
