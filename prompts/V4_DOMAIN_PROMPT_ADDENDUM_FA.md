# الحاقیه پرامپت‌های تخصصی v4 — اجزایی که باید در توالی اجرا صریح شوند

این الحاقیه را همراه پرامپت‌های v4 اجرا کن. در صورت تعارض، `PROJECT_SPEC.md` و دفتر تصمیمات v4 مرجع هستند؛ اختلاف‌ها باید در ADR ثبت شوند، نه اینکه خاموش حذف شوند.

## دستور مالی مشترک
شرایط پرداخت، ابزار، رویداد خزانه، تخصیص و ثبت GL پنج مدل مجزا هستند. نقدی/نسیه/پیش‌پرداخت/علی‌الحساب/اقساط/چندمرحله‌ای/ترکیبی/جزئی/تجمیعی/تخصیص‌نیافته/اضافه‌پرداخت/تهاتر/برگشت وجه/انتقال داخلی/چک/ارز را پوشش بده. یک پرداخت می‌تواند به چند فاکتور و چند پرداخت به یک فاکتور تخصیص یابد؛ allocation از source یا target available بیشتر نشود. bank statement import اثر قطعی پرداخت/هزینه ندارد. retry/import دوباره و double-click نباید سند یا اثر تکراری بسازند.

## دستور کدینگ حساب
درخت حساب باید parent/child, code unique, account type, normal balance, posting_allowed, required dimensions, subsidiary control, currency policy, explicit balance-sheet/P&L/cash-flow mapping, current/non-current rule, closing rule, effective dates and audit داشته باشد. طبقه را از title حدس نزن. account referenced حذف فیزیکی نشود. PostingTemplate نسخه‌دار باشد و سند منبع/خطوط GL قابل drill-down باشند.

## دستور صافی بارفروش
هر بار/ارسال، گزارش فروش بارفروش، هر هزینه/کمیسیون/کسری/اعتراض، صافی و پرداخت مستقل ولی لینک‌شده است. خروجی وزن مبدأ و وزن اعلامی دو مقدار جدا باقی می‌مانند؛ commission method/rate/date version snapshot است؛ هزینه دارای مدرک و تایید؛ چند بار در یک صافی و صافی جزئی برای یک بار مجاز؛ اصلاح بعد از posting فقط amendment/reversal.

## دستور وزن و سطر فاکتور
Gross/net basis صریح. gross شامل سبد: tare=count×tare snapshot; net=gross−tare; loss kg از درصد یا percent از kg با آخرین ورودی به‌عنوان منبع; billable=net−contractual loss−other approved deductions; amount از basis مشخص قرارداد. آزمون مرجع: 1500−(50×10)=1000؛ افت 1%=10؛ billable=990. اگر tare=5، net=1250؛ افت=12.5؛ billable=1237.5. فروش خروجی موجودی است، نه ضایعات منفی.

## دستور تجربه کاربری
تمام‌صفحه/RTL، فضای کاری ثابت، header/action bar ثابت، جدول با اسکرول داخلی، جمع‌های ثابت، tooltip فارسی روی هر کنترل مهم، quick-create drawer که draft parent را حفظ و پس از save برمی‌گرداند، KPI واقعی با drill-down، حالت‌های offline/unsynced/conflict. ساخت داشبورد زیبا با mock data به‌عنوان پیاده‌سازی واقعی پذیرفته نیست.

## دستور Excel و همگام‌سازی
Excel template versioned؛ column mapping، preview، validation، duplicate strategy، atomic commit default، source-line lineage و export audit. Office 2013 روی محیط واقعی تست شود یا unverified ثبت شود. Offline sync با UUID, idempotency, outbox/inbox, revision, conflict queue و receipts; last-write-wins برای پرداخت/سند قطعی ممنوع.

## دستور انتشار
Go-live منوط به reconciliation داده مهاجرت، موازنه GL، backup/restore drill، نصب پاک و ارتقا، rollback، امنیت/secret scan، تست‌های Critical و تایید انسانی است. هیچ push/Release بدون اجازه صریح مالک انجام نده.
