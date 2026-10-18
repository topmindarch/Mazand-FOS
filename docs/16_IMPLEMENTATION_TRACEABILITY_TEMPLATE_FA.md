# الگوی ردیابی نیاز تا کد، آزمون و انتشار

از این جدول برای هر نیاز مهم و هر تغییر استفاده شود. شناسه‌ها ثابت بمانند و برای نیاز تازه شناسه جدید صادر شود.

| فیلد | مقصود |
|---|---|
| Requirement ID | شناسه پایدار؛ مانند PAY-01، ACC-04، WGT-02، UX-03، SYNC-05 |
| Requirement text | شرح عینی قابل‌آزمون، نه برچسب کوتاه |
| Source/evidence | مسیر فایل/بخش یا تصمیم گفتگو؛ تفاوت requirement و engineering assumption مشخص شود |
| Priority / risk | Critical/High/Normal و پیامد عدم پوشش |
| Data entities | جدول‌ها/موجودیت‌ها و مالک داده |
| State transitions | Draft/Validated/Approved/Posted/Reversed/Sync states |
| UI entry points | صفحه/فرم/پنل سریع/دکمه/tooltip |
| API/service | endpoint/service/use case و رفتار retry/idempotency |
| Accounting impact | template، debit/credit و منع اثر تکراری؛ یا «بدون اثر GL» با دلیل |
| Inventory/treasury impact | movement/transaction/allocation/cheque lifecycle |
| Reports/print/Excel | گزارش‌ها و قالب‌های متأثر |
| Sync/security impact | permissions, conflict, offline, audit, sensitive fields |
| Migration | schema/data/backfill/rollback/precondition |
| Test IDs | آزمون مستقیم، regression، integration، visual/UAT |
| Evidence | مسیر log/screenshot/artifact/report؛ commit مرتبط |
| Status | NOT_INSPECTED/SPECIFIED/PARTIAL/IMPLEMENTED_UNVERIFIED/VERIFIED/BLOCKED |
| Owner/date | مسئول، تاریخ و reviewer |

## Definition of Done برای نیاز Critical
- تعریف کسب‌وکار و داده روشن است؛ null/zero، واحد، ارز و rounding تعیین شده.
- UI، API، persistence و schema واقعی هستند؛ mock فقط در test/demo محدود.
- مجوز و audit و وضعیت‌های سند تعریف و آزموده‌اند.
- اثر روی GL، موجودی، خزانه، گزارش، چاپ، Excel و sync بررسی شده؛ موارد غیرمرتبط نیز دلیل دارند.
- آزمون مستقیم و regression مرتبط، با نتیجه و شواهد واقعی، pass شده‌اند.
- migration و rollback در صورت تغییر schema/data آماده‌اند؛ Backup/restore مربوط تست شده.
- راهنمای کاربر و runbook پشتیبانی، خطا و limitation به‌روز شده.
- Reviewer/مالک بخش مربوط تأیید کرده، نه اینکه عامل خودش ادعای موافقت صاحب کسب‌وکار بسازد.
