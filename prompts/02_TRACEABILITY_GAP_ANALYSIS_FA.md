# PROMPT 02 — ماتریس نیاز تا پیاده‌سازی

این فایل مستقل و قابل کپی است؛ در صورت امکان، همراه `PROJECT_SPEC.md` و سندهای `docs/` مرتبط اجرا شود.

تمام نیازها را با ID به کد/داده/API/UI/GL/موجودی/خزانه/گزارش/چاپ/Excel/sync/security/migration/test map کن؛ موردهای صرفا نمایشی و فرض‌ها را جدا ثبت کن. فقط تحلیل؛ بدون تغییر کد.


## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.
