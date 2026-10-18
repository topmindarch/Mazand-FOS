# PROMPT 12 — بانک/چک/مغایرت

این فایل مستقل و قابل کپی است؛ در صورت امکان، همراه `PROJECT_SPEC.md` و سندهای `docs/` مرتبط اجرا شود.

Statement import creates raw lines only; suggestions require confirmation; match existing payment without second GL; lifecycle of received/issued/cleared/returned/replaced cheque; bank fee separated; transfers between own accounts not income.


## الزامات ثابت
- ابتدا بخوان و impact map بساز؛ داده/نام حساب/واحد/نوع سند را حدس نزن.
- Draft از Posted جدا؛ Posted immutable و correction با reason/link to original.
- برای هر تغییر، schema/API/UI/GL/inventory/treasury/report/print/Excel/sync/audit/security/migration اثرها را ثبت کن.
- آزمون واقعی با evidence; mock, not-run, blocked به عنوان pass گزارش نشود.
- main مستقیم تغییر نکند؛ push/release فقط با اجازه صریح.
- از docs مرجع در این بسته استفاده کن؛ اگر عامل دسترسی ندارد، اجرای تغییر را متوقف و سند را مطالبه کن.
- خروجی پایان کار: فایل‌های خوانده/تغییرکرده، IDs پوشش داده‌شده، test command/results, evidence, migration/rollback, limitations, next step. سپس توقف.
