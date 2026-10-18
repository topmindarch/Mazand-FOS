# راهنمای ساخت و آماده‌سازی مخزن خصوصی GitHub برای Mazand FOS

## ساخت مخزن
- نام مخزن: `mazand-fos`
- Owner: حساب/سازمانی که مالک قانونی پروژه است؛ نام حساب فرض نشود.
- Visibility: **Private**
- Description: `Offline-first citrus operations, inventory, accounting, settlements, reporting and Excel integration for Mazandasht.`
- README: On برای توضیح پروژه؛ اگر مخزن فعلی قبلاً README مهم دارد، آن را overwrite نکن.
- `.gitignore`: Custom با الگوی این بسته، پس از بررسی stack واقعی.
- License: برای محصول اختصاصی/private، مجوز متن‌باز را بدون تصمیم مالک اضافه نکن.

این دستورالعمل نمی‌گوید مخزن فعلی حتما ایجاد شده یا URL دسترسی دارد. پیش از اقدام، URL و owner را از محیط واقعی تایید کن. اگر پروژه‌ای از قبل وجود دارد، repository جدید نساز مگر دستور صریح مالک.

## قبل از اولین push
1. `git status --short` و `git diff --stat` را اجرا کن.
2. فهرست فایل‌های stage شده را بازبینی کن؛ فایل customer, bank statement, database, `.env`, token/key, backup, export تولیدی و attachment قرارداد محرمانه نباید در commit باشد.
3. اسکن secret انجام بده؛ false positiveها را با evidence بررسی کن.
4. `.gitignore` را با stack واقعی تنظیم کن؛ build output را به‌طور پیش‌فرض commit نکن مگر policy artifact مشخص باشد.
5. commit اول باید README, PROJECT_SPEC, docs, prompts, templates و assets غیرحساس باشد؛ داده واقعی یا dump دیتابیس اضافه نشود.

## سیاست branch/PR
- `main` محافظت‌شده؛ تغییر مستقیم ممنوع.
- feature branch با نام کوتاه مثل `docs/v4-baseline`, `feat/payment-allocation`, `fix/weight-tare-rounding`.
- هر PR: خلاصه هدف، Requirement IDs، فایل‌های تغییرکرده، DB/API/report/print/Excel/sync impact، tests و evidence، migration/rollback، known limits.
- تغییرات حسابداری/مهاجرت نیازمند بازبینی تخصصی مالی است؛ عامل کدنویسی نمی‌تواند sign-off مالک/حسابدار را جعل کند.
- version tags پس از acceptance و ثبت changelog؛ تاریخچه rewrite/force push ممنوع.

## انتشار
Release GitHub و انتشار installer دو کار جدا هستند. ساخت release draft یا آپلود file ≠ go-live. قبل از publish از release gate استفاده و اجازه صریح مالک را ثبت کن. فایل نصب/backup نباید اطلاعات واقعی مشتری یا secrets داشته باشد. checksum مستقل منتشر و روی فایل دانلودشده دوباره محاسبه شود.
