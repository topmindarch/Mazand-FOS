# Mazand FOS — سامانه یکپارچه عملیات مرکبات و مالی

**نام مخزن پیشنهادی:** `mazand-fos`  
**مالک کسب‌وکار مرجع:** مازندشت  
**وضعیت این بسته:** خط مبنای محصول، اسناد طراحی، پرامپت‌های اجرایی و دارایی‌های بصری؛ نه نسخه نرم‌افزارِ پیاده‌سازی‌شده.

Mazand FOS برای پیوند دادن عملیات مرکبات، خریدوفروش، ورود و خروج بار، سورتینگ، سبد، بانک، تسویه بارفروش، حسابداری دوبل، گزارش‌گیری، Excel/CSV، چاپ، پشتیبان‌گیری و اجرای آفلاین طراحی می‌شود.

## اصل اول: از واقعیت شروع کنیم، نه از حدس

این مخزن باید قبل از هر توسعه‌ای، وضعیت موجود کد، داده‌ها، شاخه‌ها، مستندات و تست‌ها را روشن کند. اگر مخزن از قبل محتوا دارد، هیچ‌یک از فایل‌های موجود بدون نقشه ادغام و تأیید مالک محصول بازنویسی یا حذف نمی‌شود. قابلیت صرفاً نمایشی یا mock نباید به‌عنوان قابلیت عملیاتی گزارش شود.

## از کجا شروع کنیم؟

1. `START_HERE_FA.md` را کامل بخوانید؛ این راهنمای دقیق ساخت مخزن و نخستین اجرای Arena.ai است.
2. `PROJECT_SPEC.md` را به‌عنوان ورودی کوتاه ایجنت مطالعه کنید.
3. `docs/INDEX_FA.md` را برای نقشه اسناد بخوانید.
4. **فقط پرامپت `prompts/00_GLOBAL_CONTRACT_FA.md` و سپس `prompts/01_READ_ONLY_AUDIT_FA.md` را اجرا کنید.**
5. تا زمانی که گزارش ممیزی، نقشه شکاف، وضعیت تست‌ها و پیشنهاد ادغام مستندات تأیید نشده، اجازه تغییر کد صادر نکنید.
6. پس از تأیید گزارش و معماری مرحله بعد، فقط یک پرامپت در هر نوبت اجرا و خروجی آن را بررسی کنید.

## قاعده سطح اختیار

- مرحله ممیزی: فقط‌خواندنی.
- مرحله طراحی: اسناد پیشنهادی در شاخه جداگانه، با ردیابی تغییرها.
- مرحله کدنویسی: پس از تأیید صریح و با Vertical Slice کوچک.
- هر تغییر: تحلیل اثر، تست هدفمند، رگرسیون مرتبط، بازبینی امنیت و گزارش شواهد.
- هیچ پرامپتی حق ندارد نتیجه تست یا وضعیت GitHub را جعل کند.

## نقشه پوشه‌ها

```text
/
├── README.md
├── START_HERE_FA.md
├── PROJECT_SPEC.md
├── SECURITY.md
├── CONTRIBUTING.md
├── LICENSE_POLICY.md
├── .gitignore
├── .editorconfig
├── .gitattributes
├── docs/                 # اسناد مرجع و قراردادهای محصول
├── prompts/              # دستورهای ترتیبی برای Arena.ai
├── assets/               # گرافیک، نمودارها و Design Tokens
├── tests/                # ماتریس پذیرش و آزمون‌های مبتنی بر نیاز
├── scripts/              # ابزار اعتبارسنجی اسناد و دارایی‌ها
└── .github/              # الگوهای Issue/PR و کنترل کیفیت اسناد
```

## مرز اعتماد و محرمانگی

فایل واقعی مشتریان، دفتر حسابداری، گردش بانک، اطلاعات کارمندان، رمزها، فایل `.env`، نسخه پشتیبان واقعی و کلیدهای دسترسی را به GitHub اضافه نکنید؛ حتی در مخزن Private. برای آزمون فقط از داده ساختگی و بی‌نام استفاده شود. اطلاعات واقعی باید از مسیر امن و کنترل‌شده وارد محیط عملیاتی شود.

## اسناد حاکم

- نیازمندی‌ها و خط مبنای محصول: `PROJECT_SPEC.md` و `docs/01_PRODUCT_AND_ACCEPTANCE_FA.md`
- قواعد تصمیمات: `docs/27_DECISION_REGISTER_FA.md`
- گرافیک و تعامل: `docs/06_DESIGN_SYSTEM_FA.md` و `docs/07_UX_INTERACTION_STATES_FA.md`
- پروتکل اصلاح: `docs/26_GITHUB_CHANGE_CONTROL_FA.md` و `prompts/22_CHANGE_IMPACT_REGRESSION_FA.md`
- پرامپت شروع: `prompts/00_GLOBAL_CONTRACT_FA.md` سپس `prompts/01_READ_ONLY_AUDIT_FA.md`

## وضعیت فعلی

این مخزنِ آماده‌سازی شامل مستندات و دارایی بصری است. وجود سند، نمودار، موکاپ یا پرامپت به معنی پیاده‌سازی یا تأیید عملکرد نرم‌افزار نیست. وضعیت واقعی فقط با مشاهده کد، اجرای تست و ثبت شواهد قابل اعلام است.

## جهت بصری محصول (Concept only)

تصویر زیر یک مرجع طراحی است و اعداد آن داده عملیاتی نیستند. wireframeهای قابل ویرایش و توکن‌ها در `assets/visuals/` و `assets/` قرار دارند.

![موکاپ مفهومی داشبورد Mazand FOS](assets/visuals/dashboard_concept_reference.png)

- [Wireframe داشبورد](assets/visuals/dashboard_wireframe.svg)
- [Workbench فاکتور مرکبات](assets/visuals/invoice_workbench_wireframe.svg)
- [صفحه تطبیق بانک](assets/visuals/bank_reconciliation_wireframe.svg)
- [Import Studio](assets/visuals/import_studio_wireframe.svg)
- [نمودار معماری](assets/diagrams/architecture_modes.svg)

- [فهرست کامل ۱۰+ Wireframe صفحه‌ای قابل ویرایش](assets/visuals/VISUAL_CATALOG_FA.md)
- [مشخصات اجرایی بیش از ۳۰ صفحه](docs/46_PAGE_BY_PAGE_INTERACTION_SPEC_FA.md)
- [تنظیمات دقیق Repository در GitHub](docs/00_GITHUB_SETUP_AND_FIRST_COMMIT_FA.md)

- [قواعد امنیت و حفاظت شاخه main در GitHub](docs/48_GITHUB_SETTINGS_SECURITY_BRANCH_RULES_FA.md)
- [Manifest و چک‌سام فایل‌ها](MANIFEST_FA.md)


## شروع دقیق و اجرای تمام پرامپت‌ها (نسخه v3)
1. `START_HERE_FA.md` — راهنمای ساخت مخزن و اتصال.
2. `prompts/SEQUENTIAL_COPY_PASTE_RUNBOOK_FA.md` — تمام متن کامل ۲۶ پرامپت برای copy/paste پشت سر هم، به ترتیب ۰۰ تا ۲۵.
3. `prompts/INDEX_FA.md` — فهرست و Gateهای هر مرحله.
4. `docs/49_REPOSITORY_ALL_FILE_REVIEW_PROTOCOL_FA.md` — الزام ممیزی همه فایل‌های tracked.
5. `tests/PROMPT_EXECUTION_LEDGER_FA.md` — ثبت شواهد و تأییدهای واقعی.

**نکته:** این مخزن مشخصات و پرامپت است؛ اجرای tests آن قابلیت‌های نرم‌افزار را تأیید نمی‌کند.
