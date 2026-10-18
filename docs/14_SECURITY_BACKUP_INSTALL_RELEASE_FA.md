# امنیت، مجوز، پشتیبان، نصب، ارتقا و انتشار

## هویت و دسترسی
کاربر مستقل یا الگوی مجوز تأییدشده برای هر نقش؛ حساب اشتراکی کمینه شود تا audit فردی از بین نرود. نقش‌ها می‌تواند مدیر، حسابدار، فروش، خرید، انبار، باسکول، سرپرست، خزانه‌دار، مشاهده‌گر و نقش سفارشی باشد. مجوز صرفا page-level نیست؛ عمل حساس مانند مشاهده قیمت خرید، تخفیف، وزن، مانده، Post، reversal، تغییر کدینگ، import/export، backup/restore، تغییر نقش و reopen دوره کنترل می‌شود. least privilege؛ پیش‌فرض deny؛ ثبت تلاش دسترسی ناموفق؛ rate limiting و password hashing امن؛ مدیریت session و logout؛ secrets خارج از repo و log. برای remote/admin، MFA یا محافظت تکمیلی در صورت پشتیبانی.

## اطلاعات حساس و audit
شماره حساب/شبا، اطلاعات تماس، فایل قرارداد و backup با کمترین دسترسی و در صورت نیاز رمزگذاری شوند. log عملیاتی نباید رمز، token یا اطلاعات بانکی کامل را بازتاب دهد. Audit append-only از ایجاد/تغییر/تایید/ابطال/Export/Import/Login/Permission/Backup/Restore نگهداری کند؛ شامل actor، زمان منبع، device، نوع رویداد، شناسه هدف، مقدار قبل/بعد با masking حساس و دلیل. دستکاری audit باید شناسایی‌پذیر باشد.

## Backup
حداقل: هنگام خروج تمیز، زمان‌بندی قابل تنظیم، قبل از migration/upgrade/restore یا عملیات پرخطر. سیاست retention قابل تنظیم، چند نسل نسخه، مسیر ذخیره مجزا از دیتابیس اصلی و کپی آفلاین/رسانه جدا؛ backup بیرونی رمزگذاری شود. هر snapshot manifest، timestamp، نسخه برنامه/DB، checksum، اندازه، وضعیت و نتیجه دارد. خطای backup باید در داشبورد و log با هشدار قابل مشاهده باشد؛ وجود فایل تنها مساوی سالم بودن نیست.

## Restore drill
در محیط جداگانه restore شود؛ checksum، بازشدن DB، تعداد جدول/رکورد، foreign key/integrity check، حساب توازن، تعداد اسناد مالی، چند query کلیدی، شمار اسناد منبع/GL و کارایی حداقلی بررسی شود. گزارش شامل زمان شروع/پایان، نسخه backup، نتیجه و اختلاف‌ها باشد. Restore روی داده زنده بدون تأیید و snapshot قبلی ممنوع.

## نصب و ارتقا
بسته قابل انتشار باید شامل installer/portable package متناسب با تصمیم معماری، وابستگی‌ها، migrationها، فایل نمونه config بی‌راز، راهنمای نصب، راهنمای admin، نسخه/commit، hash/checksum، release notes، محدودیت‌ها، دستور backup/restore و دستور rollback باشد. نصب اولیه، ارتقای نسخه موجود، downgrade ممنوع یا مدیریت‌شده، قطعی برق وسط migration و شکست migration آزمایش شود. Migration قبل از تغییر schema backup معتبر می‌گیرد، تراکنش یا روند قابل ادامه دارد و نسخه schema را ثبت می‌کند.

## GitHub و مدیریت سورس
مخزن Private `mazand-fos`؛ شاخه محافظت‌شده main؛ pull request با review/checks؛ ممنوعیت force push و secret؛ branch کوتاه‌عمر؛ tag نسخه؛ GitHub Actions فقط با permission کمینه. `.gitignore` باید DB، فایل‌های مشتری، exportها، backup، secrets، env، نصب‌کننده/artefactهای حجیم و داده واقعی را پوشش دهد. داده مصنوعی فقط برای نمونه. قبل از upload، git status/diff و scan راز و داده حساس بررسی شود. license متن‌باز بدون تایید مالک اضافه نشود.

## لحظه انتشار
1. freeze scope و ثبت Release Candidate commit.
2. اجرای full test suite و regression روی سیستم پاک و نسخه ارتقایی.
3. تست migration واقعی روی کپی ناشناس/آزمایشی، تطبیق count/weight/money totals.
4. restore rehearsal از backup تازه.
5. اثبات Office 2013 / Windows 7 فقط اگر محیط‌های مربوط واقعاً تست شده باشند.
6. چک مجوزها، security scan، رازها و فایل‌های ناخواسته.
7. تولید installer، ثبت checksum امضا/هش، release notes، known limits و گزارش آزمون.
8. تایید مالک برای انتشار و سپس publish/tag/upload. عامل نباید بدون اجازه صریح push یا release بسازد.
9. بعد انتشار: smoke test، log، health/sync/backup check و مسیر rollback.

## محدودیت ایمنی
بسته مستندات نمی‌تواند امنیت واقعی یا نصب موفق را ثابت کند. این ادعاها فقط بعد از آزمون محیط هدف و شواهد قابل بازتولید مجازند.
