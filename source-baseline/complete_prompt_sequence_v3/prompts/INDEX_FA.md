# فهرست ترتیبی پرامپت‌ها — Mazand FOS v3

برای دیدن تمام متن‌ها پشت سر هم، `prompts/SEQUENTIAL_COPY_PASTE_RUNBOOK_FA.md` را باز کن. در آن هر prompt کامل و قابل کپی آمده است. فایل‌های مستقل زیر همان promptها را جداگانه نگه می‌دارند. یک مرحله در هر نوبت؛ هیچ prompt خودکار chaining ندارد.

| شماره | فایل | شرح | نوشتن؟ |
|---:|---|---|---|
| 00 | `00_GLOBAL_CONTRACT_FA.md` | قرارداد سراسری و نحوه استفاده از ایجنت | `NO-WRITE` |
| 01 | `01_READ_ONLY_AUDIT_FA.md` | ممیزی فقط‌خواندنی مخزن واقعی از ابتدا تا انتها | `READ-ONLY` |
| 02 | `02_DOCS_INVENTORY_AND_MIGRATION_FA.md` | موجودی کامل اسناد و نقشه مهاجرت/ادغام | `ANALYSIS-ONLY` |
| 03 | `03_GAP_TRACEABILITY_AND_DECISIONS_FA.md` | شکاف نیازمندی‌ها، ردیابی و رجیستر تصمیم | `ANALYSIS-ONLY` |
| 04 | `04_ARCHITECTURE_DECISION_FA.md` | تصمیم معماری، مرزهای سیستم و استقرار | `DESIGN-ONLY` |
| 05 | `05_DOMAIN_MODEL_AND_DB_FA.md` | مدل دامنه، رابطه موجودیت‌ها، schema و migration | `DESIGN-ONLY` |
| 06 | `06_DESIGN_SYSTEM_VISUAL_AUDIT_FA.md` | سیستم طراحی بصری، موکاپ‌ها و ممیزی تمام صفحات | `DESIGN-ONLY` |
| 07 | `07_IMPLEMENTATION_FOUNDATION_FA.md` | زیرساخت پیاده‌سازی و اولین vertical skeleton | `CODE-AFTER-APPROVAL` |
| 08 | `08_APP_SHELL_DASHBOARD_UX_FA.md` | پوسته تمام‌ارتفاع RTL و داشبورد اصلی | `APPROVED-SLICE` |
| 09 | `09_MASTER_DATA_PARTIES_FA.md` | داده پایه، حساب‌ها و طرف‌حساب پویا | `APPROVED-SLICE` |
| 10 | `10_CITRUS_INVOICE_WEIGHT_FA.md` | فاکتور مرکبات و موتور دقیق وزن/افت | `APPROVED-SLICE` |
| 11 | `11_INVENTORY_SORTING_FA.md` | گردش انبار، ورود/خروج، سورتینگ و تلفات | `APPROVED-SLICE` |
| 12 | `12_ACCOUNTING_LEDGER_STATEMENTS_FA.md` | حسابداری دوبل، دفتر کل، بستن دوره و صورت‌های مالی | `APPROVED-SLICE` |
| 13 | `13_BANK_TREASURY_BARFOROOSH_FA.md` | بانک، دریافت‌وپرداخت، چک، تخصیص و صافی بارفروش | `APPROVED-SLICE` |
| 14 | `14_IMPORT_STUDIO_EXCEL_FA.md` | Import Studio، Excel/CSV، INFO و Export | `APPROVED-SLICE` |
| 15 | `15_OFFLINE_SYNC_MODES_FA.md` | آفلاین، LAN، Sync چنددستگاهی و صف تعارض | `APPROVED-SLICE` |
| 16 | `16_SECURITY_BACKUP_RESTORE_FA.md` | امنیت، نقش‌ها، ممیزی و پشتیبان/بازیابی | `APPROVED-SLICE` |
| 17 | `17_DASHBOARD_REPORTS_LINEAGE_FA.md` | داشبورد، گزارش‌ها، Drill-down و Data Lineage | `APPROVED-SLICE` |
| 18 | `18_PRINT_BUILDER_TEMPLATES_FA.md` | Print Builder و الگوهای فاکتور/صورت‌حساب | `APPROVED-SLICE` |
| 19 | `19_SCALE_HARDWARE_ADAPTER_FA.md` | ترازو و سخت‌افزار با Adapter و Simulator | `APPROVED-SLICE` |
| 20 | `20_VERTICAL_SLICE_DELIVERY_FA.md` | تحویل یک مسیر عمودی واقعی End-to-End | `APPROVED-SLICE` |
| 21 | `21_VISUAL_REGRESSION_ACCESSIBILITY_FA.md` | بازبینی بصری، RTL، Accessibility و Responsive Regression | `QA-ONLY-THEN-FIX-APPROVED` |
| 22 | `22_CHANGE_IMPACT_REGRESSION_FA.md` | پروتکل دائمی تحلیل اثر و رگرسیون برای هر تغییر | `REUSABLE-EVERY-PR` |
| 23 | `23_MIGRATION_INSTALLER_ROLLBACK_FA.md` | مهاجرت داده، Installer، Update و Rollback | `RELEASE-PREPARATION` |
| 24 | `24_UAT_RELEASE_GATE_FA.md` | آزمون پذیرش کاربر، کنترل انتشار و Go/No-Go | `REPORT-FIRST-GATE` |
| 25 | `25_FINAL_AUDIT_HANDOFF_FA.md` | ممیزی نهایی، تحویل، مستندسازی و اثبات وضعیت | `FINAL-AUDIT` |

## Gateها
- 00–06: قرارداد، ممیزی و طراحی؛ هیچ کدنویسی محصول.
- 07: اولین مرحله نوشتن؛ نیازمند تأیید صریح scope و branch جدا.
- 08–20: توسعه slice به slice؛ هر prompt یک scope و gate مستقل.
- 21–24: QA، اثرسنجی، migration/installer، UAT.
- 25: ممیزی و handoff نهایی؛ انتشار/merge خودکار ممنوع.
- 22 یک پروتکل reusable است که پیش از هر PR/رفع خطا مجدداً اجرا می‌شود.
