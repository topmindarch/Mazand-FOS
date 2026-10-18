# Runbook بازیابی/بازگشت نسخه

1. اعلام incident و توقف عملیات پرریسک؛ زمان/نسخه/کاربران/دستگاه‌ها ثبت شود.
2. تا روشن‌شدن دامنه، از تغییرات دسته‌ای و import مجدد خودداری شود. DB فعال را کپی امن/backup کن؛ snapshot روی snapshot overwrite نشود.
3. تشخیص بده مشکل فقط binary است یا schema/data/migration/sync event نیز متاثر شده.
4. اگر کد جدید مشکل دارد اما schema سازگار است، rollback کد فقط طبق ماتریس سازگاری تاییدشده.
5. اگر schema/data تغییر کرده، downgrade مستقیم را اجرا نکن؛ migration برگشتیِ آزموده‌شده، forward-fix یا restore کنترل‌شده انتخاب شود.
6. restore ابتدا در محیط ایزوله؛ integrity checks، تعداد رکوردها، debit=credit، source→journal links، weight/movement totals، payment allocations، bank matches و outbox receipt بررسی شود.
7. هر event بعد از snapshot یا دستگاه آفلاین در صف sync را شناسایی کن؛ از replay کورکورانه و ایجاد دوباره پرداخت/سند جلوگیری کن.
8. تصمیم بازگشت/بازیابی نیاز به مجوز مسئول مجاز دارد؛ قبل از هر restore روی production وضعیت داده و اثر از دست رفتن داده گزارش شود.
9. بعد از بازیابی smoke test و reconciliation؛ root cause، داده متاثر، زمان اختلال، روش اصلاح و تغییرات پیشگیرانه ثبت شود.
