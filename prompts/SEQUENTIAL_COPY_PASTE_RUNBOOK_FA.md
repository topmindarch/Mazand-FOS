# Mazand FOS — runbook پیوسته v4

این فایل ترتیب کلی قدیمی را حفظ و پرامپت‌های تخصصی v4 را به‌عنوان توالی فعلی تعریف می‌کند. هر مرحله مستقل است؛ پس از گزارش و تایید، مرحله بعد را اجرا کنید.## ترتيب پیشنهادی v4


---

# PROMPT 00 — قرارداد همیشگی

در همه مراحل: هیچ ادعای بدون شواهد، عدم تغییر main، ثبت impact map، حفاظت از داده، و توقف پس از همان مرحله.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 01 — ممیزی فقط‌خواندنی

ابتدا repo/branch/commit/working tree را تایید کن؛ 7 سند اصلی نام‌برده در START را در صورت وجود کامل بخوان؛ architecture, schema, persistence, auth, reports, Excel, sync, backup, installer و tests را با مسیر دقیق بررسی کن. هیچ فایل را تغییر نده و در انتها توقف کن.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 02 — ماتریس نیاز تا پیاده‌سازی

تمام نیازها را با ID به کد/داده/API/UI/GL/موجودی/خزانه/گزارش/چاپ/Excel/sync/security/migration/test map کن؛ موردهای صرفا نمایشی و فرض‌ها را جدا ثبت کن. فقط تحلیل؛ بدون تغییر کد.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 03 — معماری و ADR

تصمیم در مورد مرز ماژول، persistence، API، شناسه/شماره، transaction/idempotency، decimal، offline/LAN/sync/conflicts، installer و migrations بگیر؛ چند گزینه/پیامد/rollback را مستند کن؛ پس از ADR توقف.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 04 — کدینگ حساب و اشخاص

درخت حساب با account type/normal balance/postable/dimensions/control accounts/statement mapping/close rule; Party چندنقشی؛ template ثبت نسخه‌دار؛ quick-create drawer با حفظ draft و جلوگیری از duplicate. نام حساب مبنای mapping نیست.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 05 — شرایط/ابزار/رویداد/تخصیص/GL

پنج شیء PaymentTerms, PaymentInstrument, TreasuryTransaction, SettlementAllocation, JournalPosting را جدا پیاده کن. پشتیبانی نقدی/نسیه/پیش‌پرداخت/اقساط/ترکیبی/جزئی/تجمیعی/اعتبار آزاد/تهاتر/refund/انتقال داخلی/چک/ارز و N:M allocation. ممنوعیت over-allocation و double-post را تست کن.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 06 — بارفروش و صافی

Contract/version, load, sales report, dispatch vs reported weight, commission version, documented expenses, dispute, partial/multi-load safi, advance/payment allocations and journal links. هیچ صافی قطعی بی‌صدا ویرایش نشود؛ amendment/reversal.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 07 — وزن و فاکتور مرکبات

فرمول gross/tare/net/loss/billable/amount را در domain service مشترک اجرا کن؛ basis=GROSS/NET مشخص، tare snapshot، آخرین ورودی درصد/کیلو افت، کنترل manual override، خرید دستی فقط یک inbound movement. مثال 1500−50×10=1000; 1%=10; billable=990.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 08 — رابط مدرن

RTL، full viewport, fixed header/sidebar/action bar, internal table scroll, stable totals, no long form scrolling, Persian contextual tooltips, keyboard/focus, visible state, real drilldown KPIs, quick-create return context, loading/empty/error/offline/conflict. Mock فقط با Demo label.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 09 — INFO/Excel

Template registry/version، mapping preview، normalization، type validation، duplicate candidates، atomic import default، explicit partial import، stable IDs, raw row lineage, no overwrite, Excel 2013 compatibility test and structured reports.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 10 — موجودی و سورت

Movement ledger as stock source; purchase/harvest/sale/transfer/sort/waste/return/adjustment; sale != negative waste; sorting mass balance; baskets/tare and custody separate; negative stock warning not global block.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 11 — دفترکل و گزارش

PostingTemplate versions; true double-entry; balanced posted journal; immutable postings and reversal; 4/6-column trial balance; balance sheet distinct; P&L/cash flow/equity; subledger control and explicit mappings; missing != zero, unverified cost = estimated.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 12 — بانک/چک/مغایرت

Statement import creates raw lines only; suggestions require confirmation; match existing payment without second GL; lifecycle of received/issued/cleared/returned/replaced cheque; bank fee separated; transfers between own accounts not income.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 13 — آفلاین/Sync/backup/security

Standalone local and LAN central profiles; no writable SQLite over network; transactional outbox/inbox, UUID, idempotency, conflict queue, no financial last-write-wins; RBAC at service; masked bank data; scheduled/checksum backup and actual restore drill.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 14 — برش عمودی end-to-end

Deliver one complete UI→service→DB→movement/treasury→GL→report/print→audit path, real persistence, error/offline handling and tests. Scope small, no UI-only feature called done.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 15 — تست و UAT

Run unit/integration/API/database/migration/UI/visual/offline/Excel/restore/installer tests, record command, expected/actual, environment, artifact and commit; owner signs UAT. Do not present unrun as PASS.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

# PROMPT 16 — مهاجرت، نصب، انتشار

Dry-run legacy mapping and reconcile counts/weights/money/subledgers/GL; package versioned installer, migration/rollback, docs, checksums and known limits; release only after all gates + explicit owner authorization.

## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.


---

## منبع تاریخی v3
نسخه runbook پیشین به صورت `LEGACY_SEQUENTIAL_RUNBOOK_v3_FA.md` حفظ شده؛ برای ردیابی از آن استفاده کن، نه اینکه بدون تطبیق تمام قواعد قبلی را بر نسخه v4 غالب بدانی.
