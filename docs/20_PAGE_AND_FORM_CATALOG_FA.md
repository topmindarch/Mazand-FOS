# کاتالوگ صفحات، فرم‌ها و قرارداد UX برای پیاده‌سازی

برای هر صفحه، skeleton، حالت بارگذاری/خالی/خطا/آفلاین/محدودیت مجوز، help/tooltip و permission باید مشخص شود. این فهرست معنای صفحه را مشخص می‌کند؛ هر چیزی که در کد موجود نیست نباید به عنوان قابلیت حاضر نمایش داده شود.

| صفحه/کار | هدر و ورودی اصلی | منطقه کاری | Summary/عمل ثابت | جزئیات/آزمون بحرانی |
|---|---|---|---|---|
| Dashboard مدیریت | بازه زمانی، انبار/مسئول، وضعیت اتصال | KPI قابل کلیک، روند وزن/مالی، استثناها، رویداد اخیر | آخرین زمان داده و status دستگاه | KPI از query واقعی؛ Missing ≠ zero؛ drill-down حفظ فیلتر |
| Party directory | Search, role, status | جدول نام/کد/نقش/مانده/آخرین گردش | Create/import/export based permission | پرونده یکپارچه چندنقشی، duplicate candidate، merge dry-run |
| Party dossier | شناسه، نقش‌ها، اعتبار و terms | timeline اسناد/بانک/چک/صافی/GL | مانده باز/سررسید/تخصیص آزاد | جدیدترین‌به‌قدیمی‌ترین؛ هر رخداد source-linked |
| Account tree/editor | code, name, parent, class/type | tree + property editor + mapping/dimensions | validate/save/version preview | child posting, inactive referenced account, audit/approvals |
| Product/Basket | code, title, grade, unit, default values | tabs basic/price/weight/loss/handling | Save/deactivate/history | tare/loss validity versioned; override permissions |
| Scale/receipt | type gross/net basis, ticket, actor, date, source | load/baskets/weight components | net/loss/billable summary + discrepancy | don't deduct tare twice; explicit unit and source |
| Purchase invoice | party, date, warehouse, terms, scale link | line grid and weight calculator | weight totals, net amount, warning, draft/validate/post | manual create movement once; payment separate |
| Sales invoice | buyer, type, terms, warehouse/dispatch | product row grid with price/discount and stock notice | gross/net/billable/amount/tender/open balance | credit vs cash; no hard lock on negative balance by default |
| Payment/receipt | event type, party/account/instrument, date, amount | allocation list and evidence pane | amount, allocated, unallocated, remaining | N:M allocation, mixed tender, idempotency, cheque state |
| Cheque center | direction/status/due range/bank | lifecycle timeline + linked docs | due/overdue/bounced amounts | events and their GL effects are separate |
| Bank reconciliation | account/date/import batch | statement lines + candidate matches | matched/unmatched totals and variance | no silent classification/posting; duplicate import |
| Barforoosh load | contract, party, dispatch/scale ticket | product/basket weight details | dispatched amount/weight and current status | load number unique, source docs linked |
| Sales report from barforoosh | counterparty, period, imports | reported quantity/price/returns and evidence | variance versus dispatch | keep origin and reported weight separately |
| Safi settlement | barforoosh, included loads, contract version | gross plus each deduction line and evidence | net settlement, allocated advances, paid/open balance | partial/multi-load; line-level cost; draft→approved→posted |
| Sorting run | input warehouse/products, line/time | input, grade outputs, waste, WIP, cost | mass balance and exception | tolerances; no silent balancing |
| Inventory movements | item/grade/warehouse/date/type | movement ledger, source links | on-hand projection, negative stock exception | balance derived from movement; sales not negative waste |
| General journal | date/period, source/template, description | debits/credits/dimensions and attachments | totals/difference and post/reverse controls | imbalance Draft allowed; post blocked |
| GL/Trial balance | period, account tree, posting status | drillable rows | debit/credit controls | 4/6 columns exact definitions; GL/subledger reconciliation |
| Financial statements | period/office/mapping | balance sheet, P&L, cash flow, equity | reported status, method, data freshness | explicit mappings; estimated profit marked |
| INFO/Excel wizard | workbook/sheet/template | column mapping/preview/validation/conflicts | valid/warning/rejected/duplicate counts | no commit before preview; atomic default; Office 2013 compatibility |
| Sync center | device/profile/schema, connectivity | outbox/inbox/conflicts/event receipts | last success, pending, rejected, conflict | status truthful; no last-write-wins financial conflicts |
| Backup center | retention/target/schedule | backup history/checksum/restore jobs | latest verified backup and latest restore test | restore drill; failures visible; permission and audit |
| Print builder | doc/report/paper selection | rows/columns, widths, orientation, signatures, headers | preview/save version/export | template version and stable print layout; physical book number where needed |

## Print/statement builder minimum
User can choose show/hide allowed columns, row groupings, table width, paper size/orientation, header/footer, signature titles, date/number formats, totals, page break behavior and print labels. Each template is versioned and permission controlled. Invoice/account book use can display a per-row or per-document number according to template needs. Export and print must preserve source document numbers and units; a screenshot-like PDF without selectable totals doesn't replace structured report data.

## Accessibility and low-literacy requirements
- visible label, tooltip and optional inline example for weight/account/payment fields.
- warnings with action and severity; separate non-blocking warning from blocking validation.
- tab order follows workflow; Enter applies contextual cell behavior but never accidentally posts a financial document without explicit intent.
- all important action buttons labeled; destructive/reversal action asks for reason and confirmation.
- empty state teaches next action; error states retain draft; unsupported feature clearly marked.
