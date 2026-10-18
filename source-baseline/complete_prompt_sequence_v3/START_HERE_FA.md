# شروع سریع دقیق Mazand FOS — نسخه ۳

## تصمیم‌های صفحه Create repository
- Owner: حساب GitHub خودت؛ پیش از ساخت مطمئن شو این مالک به Arena Connector دسترسی دارد.
- Repository name: `mazand-fos` (نام تجاری: Mazand FOS).
- Description: `Offline-first citrus operations, inventory, accounting, settlements, reporting and Excel integration for Mazandasht.`
- Visibility: **Private**.
- Add README: **Off برای repo جدید** چون بسته README دارد. اگر قبلاً On انتخاب شده، README را overwrite نکن؛ merge کن.
- Add .gitignore: **No .gitignore** چون بسته `.gitignore` اختصاصی دارد. اگر قبلاً ایجاد شده، تفاوت را بررسی و ادغام کن.
- License: **No license** برای پروژه اختصاصی.

## Clone و قرار دادن بسته
1. دانلود `Mazand_FOS_Complete_Prompt_Sequence_v3_FA.zip` و Extract.
2. مخزن GitHub را Clone کن.
3. محتویات پوشه Extract شده را به root clone کپی کن، نه فولدر والد را.
4. README قبلی را backup/compare کن؛ فایل‌های same-name را کورکورانه overwrite نکن.
5. مطمئن شو GitHub واقعی، root مخزن، و محل بازکردن ZIP یکی هستند.
6. PowerShell از root:

```powershell
python scripts/validate_docs.py
if ($LASTEXITCODE -ne 0) { throw "Documentation validation failed" }
git status --short
git diff --check
```

برای فایل‌های تازه که هنوز stage نشده‌اند، پس از review:

```powershell
git add .
git diff --cached --check
git status --short
```

تنها پس از اطمینان از نبود داده حساس commit کن.

## Arena.ai
1. Connect GitHub و اجازه دسترسی فقط به `mazand-fos` (اگر UI امکان محدودسازی repository را دارد).
2. Agent Mode را باز کن و branch/main/ref را تأیید کن.
3. کادر Prompt 00 را از `prompts/SEQUENTIAL_COPY_PASTE_RUNBOOK_FA.md` کپی و ارسال کن.
4. پس از پاسخ، بررسی کن Agent واقعاً فایل را خوانده باشد. بعد Prompt 01 را در پیام مجزا بفرست.
5. بعد از هر مرحله توقف و گزارش را مرور کن؛ approval پیام جداگانه است.
6. هرگونه `NOT FOUND`, `BLOCKED`, mismatch یا secret exposure یعنی توقف؛ از Agent نخواه برای پوشاندن مشکل چیزی بسازد.
7. پس از مرحله ۰۶ باید design/architecture sign-off ثبت شود؛ پیش از مرحله ۰۷ تأیید شروع کدنویسی بده.

## خط قرمز
این کیت مستندات/پرامپت است، نه نرم‌افزار قابل نصب. نصب‌شدن مخزن یا موفقیت `validate_docs.py` به معنی build/financial correctness نیست. هیچ قابلیت به‌عنوان implemented معرفی نمی‌شود مگر کد و آزمون واقعی آن اثبات شود.
