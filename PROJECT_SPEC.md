# PROJECT_SPEC — Mazand FOS (خط مبنای محصول و اجرای سیستم)

**نسخه سند:** 4.0-Documentation Baseline · **زبان محصول:** فارسی RTL · **نوع محصول هدف:** ERP تخصصی عملیات مرکبات با حسابداری دوبل، خزانه، موجودی، گزارش و تبادل داده.

> این سند تعریف نیازمندی است و بیان نمی‌کند همه قابلیت‌ها در مخزن نرم‌افزار فعلی پیاده شده‌اند. وضعیت واقعی باید در audit ثبت شود. قبل از هر تغییر، `prompts/01_READ_ONLY_AUDIT_FA.md` اجرا و نتیجه تایید شود.

## 1. هدف و مرز محصول
Mazand FOS برای عملیات واقعی مازندشت طراحی می‌شود: برداشت باغ، ورود و باسکول، خرید، فروش، انبار، سورتینگ، بسته‌بندی، گردش سبد، ارسال امانی، گزارش فروش بارفروش، صافی، دریافت و پرداخت، بانک و چک، حسابداری دوبل و صورت‌های مالی؛ همچنین داده پایه، Excel/INFO، import/export، پشتیبان، sync چنددستگاهی، نصب و گزارش‌گیری.

این سامانه فقط دفتر ثبت نیست؛ هر رویداد باید ارتباط روشن بین سند تجاری، وزن/موجودی، طرف‌حساب، خزانه، تخصیص، سند حسابداری، گزارش و audit داشته باشد. ثبت در یک ماژول نباید با واردکردن دوباره همان داده در ماژول دیگر باعث اثر دوبرابر شود.

## 2. اصول غیرقابل‌نقض
1. ممیزی بدون تغییر قبل از پیاده‌سازی؛ حفظ تاریخچه Git و داده؛ branch جدا؛ هیچ تغییر مستقیم main.
2. حسابداری واقعی دوبل؛ Post قطعی فقط با توازن debit/credit. Draft نامتوازن به‌روشنی Draft می‌ماند.
3. سند قطعی از مسیر عادی edit/delete نمی‌شود؛ اصلاحیه/reversal با علت، کاربر، زمان و لینک به اصل.
4. شرط پرداخت، ابزار پرداخت، رویداد خزانه، تخصیص و JournalPosting مدل‌های جدا هستند.
5. حساب‌ها درختی و قابل‌تعریف با طبقه‌بندی، ماهیت مانده، ابعاد، نگاشت گزارش و تاریخ اعتبار صریح هستند؛ نام حساب کافی نیست.
6. مدل تسویه باید پشتیبانی کند: نقدی، نسیه، پیش‌پرداخت/علی‌الحساب، اقساط، چندمرحله‌ای، ترکیبی/چندابزاری، جزئی، تجمیعی، تخصیص‌نیافته/اضافه‌پرداخت، تهاتر، برگشت وجه، انتقال داخلی، چک و ارز.
7. بارفروش: ارسال، گزارش فروش طرف، وزن مبدأ/اعلامی، کمیسیون، هزینه‌های مستند، کسری/اعتراض، صافی جزئی و تجمیعی و پرداخت به‌هم مرتبط باشند؛ نرخ/قرارداد نسخه‌دار.
8. ریال واحد ذخیره پایه؛ تومان تبدیل نمایشی ثابت 10:1. ارز خارجی مبلغ اصل، نرخ تاریخ‌دار/منبع و مبلغ پایه جدا دارد.
9. وزن/افت/سبد فرمول مرجع دارد؛ gross vs net basis explicit است؛ آخرین تغییر بین loss-percent و loss-kg تعیین می‌کند کدام ورودی منبع محاسبه باشد.
10. موجودی حرکت‌محور؛ فاکتور خرید دستی در صورت نبود receipt باید فقط یک حرکت ورودی ایجاد کند؛ فروش خروجی است، نه ضایعات منفی. منفی بودن موجودی هشدار/استثناست و قفل عمومی نیست.
11. import بانک یا Match پیشنهادی اثر قطعی مالی نیست. بدون تایید، classification/post خودکار انجام نشود؛ تطبیق با پرداخت موجود اثر دوباره ایجاد نکند.
12. Excel قالب نسخه‌دار، mapping، preview، validation، duplicate/conflict handling و receipt import دارد؛ Office 2013 تنها پس از آزمون واقعی verified می‌شود.
13. Standalone Offline و LAN Shared دو پروفایل تعریف‌شده‌اند؛ انتقال/Sync UUID, idempotency, revision, conflict review و receipt دارد. last-write-wins برای اسناد مالی ممنوع؛ SQLite writable روی network share ممنوع.
14. UI باید premium, modern, graphical, RTL، full-height/workspace-based، سریع، با fixed action bar، جدول اسکرول داخلی، help/tooltip فارسی و quick-create drawer حفظ‌کننده Draft باشد.
15. Backup زمان‌بندی‌شده + هنگام خروج + پیش از عملیات پرخطر، checksum و restore drill واقعی دارد.
16. انتشار نیازمند migration rehearsal، tests, restore, installer, hashes, known limits و تایید انسانی است. عامل کدنویسی مجاز به push/publish بدون دستور صریح مالک نیست.

## 3. ماژول‌های دامنه
- **Master Data:** Party/PartyRole؛ Chart of Accounts؛ Product/Grade؛ BasketType؛ Warehouse؛ BankAccount; CostCenter; payment terms/instruments; document/print/import templates.
- **Operations:** Harvest; ScaleTicket; Purchase; Sales; Transfer; InventoryMovement; SortingRun; Packaging; PhysicalWaste; BasketCustody; Return and Adjustments.
- **Barforoosh/Consignment:** Contract/version; Load/Dispatch; BarforooshSalesReport; Commission; ExpenseLine; Dispute; SafiSettlement and lines; PaymentAllocation.
- **Treasury:** Cashboxes; BankAccounts; Receipt/Payment/Transfer; PaymentInstruments; Cheques/lifecycle; BankStatementLine; Matching/Reconciliation; overdue maturity.
- **Accounting:** PostingTemplate versions; Journal/JournalLine; GL/subledgers; trial balances; balance sheet; P&L; cash flow; equity changes; closing controls; audit and reconciliation.
- **Data Exchange:** INFO/template registry; Excel import/export; mapping preview; duplicate/merge/conflict; controlled legacy migration.
- **Platform:** Auth/RBAC/action permissions; audit; local DB/outbox/inbox; LAN API; sync; backup/restore; installer; logs/health; documentation and help.

## 4. قراردادهای حیاتی کسب‌وکار
### 4.1 وزن و افت
For gross weight including baskets:
`basket_tare_total = basket_count * tare_snapshot`
`fruit_net = gross_scale_weight - basket_tare_total`
`loss_weight = fruit_net * loss_percent / 100` unless loss weight was last manually edited, in which case percent is derived.
`billable_weight = fruit_net - contractual_loss_weight - other_approved_weight_deductions`
`line_amount = billable_weight * unit_price` unless the approved document type explicitly selects a different price basis, which must be visible.

Reference example: 1500kg gross, 50 baskets × 10kg = 500kg tare, 1000kg net, loss 1%=10kg, 990kg billable. With 5kg tare, net=1250kg and loss=12.5kg, billable=1237.5kg. If scale input already represents net fruit, tare is not deducted again. Missing/negative/over-limit data must be validated and traceable.

### 4.2 Party dossier and quick create
One Party can have multiple roles. Selecting a party shows current/open balance, overdue, unallocated amounts, open invoices/safi, bank/cheque activity and full chronological history. Quick-create opens a side panel, stores parent draft, creates the master record, returns to the exact invoice/payment/safi context and selects the new record without losing table rows/focus/scroll.

### 4.3 Payment/settlement
Five separate concepts: PaymentTerms, PaymentInstrument, TreasuryTransaction, SettlementAllocation and JournalPosting. Support N:M allocations; partial payment; multiple payments per invoice; one payment allocated to multiple documents; advance before invoice; unallocated credits; mixed cash/bank/cheque/credit; installments; offsets/contra; refunds; own-account transfers; currency; cheque lifecycle. Allocation cannot exceed unallocated source/target without an explicitly modeled approved credit/adjustment.

### 4.4 Barforoosh safi
Safi is a versioned, auditable settlement result, not a single freeform amount. Components: confirmed sales, commission formula/version, freight, approved expenses and receipts, contractual shortage/returns/claims, previous advances/payment allocations and documented adjustments. Keep outbound and reported sales weights distinct. Multi-load safi and partial safi per load. Any final change after posting is an amendment/reversal, never silent edit.

### 4.5 Accounts and reports
Account class/type, normal balance, posting eligibility, required dimensions, control/subledger type, financial statement mapping, current/noncurrent rule, closing rule and currency policy are explicit. Four-column trial balance = debit/credit movements and debit/credit balances. Six-column trial balance = opening debit/credit, period movements debit/credit, closing debit/credit. Balance sheet is not trial balance. Missing data is not zero; unverified cost policy means profit estimated/unfinalized.

## 5. UX baseline
Dashboard with clickable real KPIs and drilldowns; stable RTL shell; role-based workspaces; large well-labeled controls; fixed header and action bar; internal table scrolling; contextual Persian help; clear Draft/Approved/Posted/Reversed/Sync Pending/Conflict/Bank Unmatched/Estimate states. Critical actions not icon-only. All screens require loading, empty, error, permission-denied, offline, unsynced and conflict states. Mock values only in explicitly marked demo environment.

## 6. Platform/offline/data integrity
Standalone mode owns local state; LAN mode uses central API and database with no dependency on public internet; controlled sync joins them through event UUID/idempotency and conflict resolution. Reports label whether local or central/fresh. Do not claim shared instant totals across isolated devices. Windows 7 is legacy compatibility only where actual tests pass; no secure remote access promise. Remote access from supported hosts via VPN/secure gateway.

## 7. Acceptance and release definition
Definition of Done is in `docs/16_IMPLEMENTATION_TRACEABILITY_TEMPLATE_FA.md`. Acceptance catalog `docs/15_ACCEPTANCE_TEST_CATALOG_FA.md`; release gate `release/RELEASE_GATE_CHECKLIST_FA.md`. Every Critical requirement needs code path, data persistence, permission/audit, tests, actual artifacts and migration/rollback where relevant. Not-run or blocked is never pass.

## 8. Read order and prompt order
Read `00_START_HERE_FA.md`, decision register, workflow baseline and specialist docs. Start with prompt 00 then prompt 01 audit-only. Follow `prompts/INDEX_FA.md` and stop at each gate. Read the `source-baseline/` docs as historical/raw source; keep differences explicit. The package itself is specification and prompt material, not proof of implementation.
