# Runbook اجرایی ساخت Release Candidate و انتشار

## فاز 1 — آمادگی
- ثبت نام/version و base commit؛ branch مخصوص release candidate بساز.
- مطمئن شو working tree clean است و تمام تغییرات PR-مرور شده‌اند.
- test fixture مصنوعی است؛ هیچ فایل مشتری/بانک/backup یا .env در stage نباشد.
- نسخه schema/migrations و compatibility بین server/client را ثبت کن.
- dependency/license/security scan و گواهی build را انجام بده.

## فاز 2 — دروازه‌های محصول
- تمام acceptance IDs بحرانی و P0/P1 را از catalog اجرا کن.
- مالی: debit=credit، subledger=GL reconciliation، receivables/payables aging، bank matching، cheque state، safi totals، payment allocation limits.
- عملیات: weight formulas، inbound/outbound ledger، sorting yield، stock exception، basket tare snapshots.
- Excel: import preview/atomicity/dedup/re-import/export; Office 2013 real test or explicitly unverified.
- offline/sync: restart, retry, duplicate UUID, conflicts, no loss/no double post.
- backup: checksum and actual restore drill.
- installer: clean install, upgrade, migration failure path and rollback.
- UX: RTL, keyboard, quick-create return, no lost drafts, error/offline/conflict states.

## فاز 3 — مهاجرت آزمایشی
- نسخه کپی و ناشناس‌شده داده legacy بساز.
- mapping owner-approved، validation summary و unresolved duplicates/conflicts را ثبت کن.
- قبل/بعد: تعداد Party/Product/Document؛ جمع وزن؛ فروش/خرید؛ مانده باز اشخاص؛ بانک/چک؛ صافی؛ inventory; debit/credit.
- هر اختلاف دسته‌بندی و مستند شود؛ «تلورانس» فقط اگر سیاست تاییدشده دارد.
- sign-off انسانی عملیات/مالی را نگه‌دار. بدون آن، Release Candidate ممکن است اما Go-Live نیست.

## فاز 4 — بسته‌بندی
- installer یا deployment package نسخه‌دار، migrations، config example بی‌راز، راهنمای ادمین/کاربر، changelog، known limitations و rollback را داخل بده.
- SHA-256 تولید کن؛ verify artifact پس از انتقال/دانلود.
- clean machine/VM install و upgrade آزمایش شود.
- artifact final را در محل release candidate مستقل نگه‌دار؛ پوشه runtime یا DB فعال داخل آن قرار نده.

## فاز 5 — تایید انسانی و انتشار
- Release gate checklist را هر بند تیک بزن و شواهد بده.
- بازبین/مالک تایید صریح release را بدهد. عبارت کلی «ادامه بده» اگر در context به release واقعی اشاره ندارد، معادل اجازه push/publish در نظر گرفته نشود.
- پس از تایید، tag نسخه، ایجاد GitHub Release، آپلود installer/hash/release notes و verify لینک/file انجام شود.
- اگر مجوز انتشار هنوز صادر نشده، وضعیت `READY FOR OWNER REVIEW` بماند؛ push/publish نکن.

## فاز 6 — پس از انتشار
- smoke test نصب تازه و upgrade path؛ login, create draft, sample operation, report, backup.
- monitor خطاهای migration, sync queue/conflict, backup failures, bank unmatched volume, out-of-balance posting attempts.
- trigger rollback: data integrity issue, duplicate financial effect, failed restore, security leak, migration corrupt, critical operational outage.
- rollback فقط به نسخه کد سازگار با schema/data؛ downgrade دیتابیس بدون برنامه داده‌ای ممنوع. در صورت نیاز forward-fix یا restore کنترل‌شده طبق incident plan.
