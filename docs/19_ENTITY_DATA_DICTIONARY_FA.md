# فرهنگ داده مرجع و قواعد موجودیت‌ها

این فرهنگ، قرارداد سطح مفهومی است. نام جدول و نوع فیلد نهایی با معماری repo تعیین می‌شود، اما معنای داده، کلیدها و invariants باید حفظ شوند.

## قواعد مشترک تمام رکوردها
- شناسه داخلی پایدار UUID؛ کد/شماره خوانا مستقل از UUID؛ tenant/company scope اگر چند شرکت/دفتر پشتیبانی شد.
- `created_at`/`updated_at` با UTC/offset معتبر در storage؛ نمایش محلی/شمسی لایه UI. زمان واقعی منبع با `source_timestamp` از زمان دریافت/ثبت سیستم جداست؛ ساعت ساختگی ممنوع.
- `created_by`, `updated_by`, `device_id`, `revision/version`, `status`, `source_type`, `source_id`, `idempotency_key` در جایی که semantics دارد.
- فیلدهای مالی/وزنی به نوع decimal دقیق با scale روشن؛ float برای مانده و وزن مالی استفاده نشود. واحد به همراه مقدار/نسخه snapshot ذخیره شود.
- `null` یعنی نامشخص/ثبت‌نشده؛ `0` به معنی صفر واقعی است. نمایش، import و report این دو را یکی نکنند.
- موجودیت referenced حذف فیزیکی نمی‌شود؛ archive/deactivate/status انتقالی؛ change history برای داده حساس/مرجع.

## موجودیت‌های عملیاتی/مالی
| موجودیت مفهومی | کلیدها و فیلدهای معنایی | روابط ضروری | invariantهای مهم |
|---|---|---|---|
| Party | party_uuid, code, legal/display name, aliases, contact, active/status | PartyRole, documents, subledger, files | یک شخص چند نقش دارد؛ merge کنترل‌شده؛ نام مشابه به‌تنهایی duplicate قطعی نیست |
| PartyRole | party_uuid, role_type, effective dates, role-specific terms | Party, contract/pricing groups | نقش تغییر کند، سند تاریخی دوباره تفسیر نشود |
| Account | code, name, parent, type, normal balance, posting flag, report mapping, dims | JournalLine, PostingTemplate, parent/children | account type از name حدس زده نشود؛ referenced account physically deleted نشود |
| PostingTemplate | source type, version, rules, date validity, accounts/resolvers | source docs, Journal | version immutable؛ یک source event دوبار Post نشود |
| Product | code, name, category/grade, default unit, sale/purchase defaults, loss policy | invoice lines, stock movement, basket rule | نام/قیمت پیش‌فرض تاریخی را عوض نکند؛ نوع مرکبات در row مستقل |
| BasketType | code, type/color, tare, unit, effective dates | invoice/scale rows, custody ledger | tare snapshot in historic row; tare doesn't subtract twice |
| Warehouse | code, name, active/status, type/location | InventoryMovement, scale/load | transfer has linked from/to movements; no silent negative adjustment |
| ScaleTicket | UUID/number, gross/entry basis, date, operator, device/source | purchase/sale/load/movement | gross/net basis explicit; ticket is not automatically payable or revenue |
| CommercialDocument | UUID/number, type, date, counterparty, status, terms, totals | document lines, payment allocations, movements, journals | draft vs posted; source links; immutable after post |
| DocumentLine | line UUID, product, quantity, unit, basket, gross/tare/net/loss/billable, price, amount | parent doc, stock line, tax/discount if used | stable row key; snapshots; calculation mode; last-edited loss source |
| InventoryMovement | UUID, product/grade, warehouse, signed quantity, unit, movement type, status, source | source document, transfer pair, cost layer/value | posting/edit/idempotency; movement is authoritative input to stock balances |
| SortingRun | input rows, output rows, waste, WIP, time/line, quality/cost | InventoryMovement, expense | mass balance within tolerance or exception |
| ConsignmentLoad | load ID, dispatch date, barforoosh, scale references, products, weight/baskets, contract version | dispatch movement, sale report, safi | dispatch and barforoosh-reported sales both retained |
| BarforooshSalesReport | report ID/date, load links, product/weight/price/returns/source evidence | load, Safi | amendment/history; no overwriting origin report silently |
| SafiSettlement | safi UUID/number, period, counterparty, load lines, gross and deductions, net, status/version | contract, reports, payments, journal | partial/multi-load, line-level deductions, net sum reconciliation |
| SafiDispute | type, quantity/amount, reason, evidence, owner, state, resolution | load/safi/report | unresolved dispute remains visible; state transition audit |
| PaymentTerms | type, installment schedule, advance, due rules, effective dates | commercial docs/contract | agreement not a money movement |
| PaymentInstrument | cash/POS/transfer/cheque/offset/custom, instrument attributes | TreasuryTransaction, Cheque | method not equal to allocation or GL posting |
| TreasuryTransaction | receipt/payment/transfer UUID, account, actual date, amount/currency, reference, state | allocations, bank statement, journal, cheque | actual time/source; transfer not income; retry idempotent |
| SettlementAllocation | source transaction UUID, target document/safi UUID, allocated amount, state/version | treasury, target balance | aggregate not > available amount; partial allocations supported |
| BankStatementLine | import batch/hash, bank date, amount in/out, reference, raw descriptor, match state | bank account, treasury/payment/safi/journal | imported row by itself is not accounting classification |
| Cheque | serial, bank/account, issue/due dates, amount, direction, lifecycle state, linked doc | treasury, counterparty, journal, replacement cheque | lifecycle state and financial effect are separate; returned/cleared traceable |
| Journal | voucher UUID/number, date/period, source, template version, status, reversal link | JournalLine, source documents | posted only if balanced; immutable; reversal link required |
| JournalLine | account, debit/credit, amount/currency, dimensions, source line | journal, account, party, cost center etc. | one-sided line according to design; total debits=credits; dims validated |
| ImportBatch | file hash, template version, source, row results, counts, creator, time | mapping, rows, entities created/updated | validate-before-commit; idempotent rerun; rejection log immutable |
| SyncEvent | event UUID, device, entity UUID, revision, payload schema, idempotency key, state | outbox/inbox, conflict item, receipt | duplicate event doesn't apply twice; conflict never silently discarded |
| AuditEvent | actor, action, subject UUID, before/after, reason, time/device, correlation ID | any sensitive entity/action | append-only model/retention; secrets masked |
| BackupArtifact | ID/path, hash, app/schema version, timestamp, result, restore test | backup policy, release/migration | existence alone not success; restore evidence required |

## نکته درباره relationshipها
هر سند تجاری باید رابطه منبع/اثر را داشته باشد: مثلا فاکتور خرید به ScaleTicket اختیاری، movementهای حاصل، payment allocations و Journal link؛ صافی به loads/reported sales/expense lines/transactions/journal. رابطه صرفا بر اساس نام/تاریخ جست‌وجو شده را «ارتباط قطعی» تلقی نکن؛ باید stored link یا سند تاییدشده باشد.
