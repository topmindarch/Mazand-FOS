# پروتکل ممیزی عمیق تمام فایل‌های مخزن Mazand FOS

## هدف
این سند مرجع اجباری برای مرحله ۰۱ و ممیزی نهایی است. «تمام فایل‌های مخزن» یعنی تمام فایل‌های tracked/قابل‌دسترسی در commit بررسی‌شده، نه فقط مجموعه‌ای از نام‌های آشنا. فایل binary را از نظر metadata، hash و مصرف طبقه‌بندی می‌کنیم؛ آن را به‌دروغ فایل متنیِ خوانده‌شده اعلام نمی‌کنیم.

## ۱. ثبت مبنای غیرقابل ابهام
- repository owner/name/URL, visibility, default branch, analysis ref/commit SHA, timestamp, connector/tool limitations.
- اگر local clone وجود دارد: `git status --short`, `git rev-parse HEAD`, `git branch --show-current`, `git ls-files`; فرمان‌ها و exit code را ثبت کن. در remote-only mode، خروجی واقعی GitHub Connector را ذخیره/نقل کن و محدودیت‌ها را مشخص کن.
- ممیزی فقط روی ref ثابت انجام شود. اگر branch هم‌زمان تغییر کرد، آن را drift علامت بزن.

## ۲. طبقه‌بندی فایل‌ها
هر tracked path باید یکی از categoryهای زیر را بگیرد: source, UI, API, domain, persistence/schema, migration, test/fixture, build/dependency, CI/workflow, deployment/installer, config, docs/spec, prompt, visual/design asset, generated/binary, secret-risk, unknown. برای هر فایل ثبت کن: path, extension, bytes, SHA-256 اگر امکان دارد، first-party/third-party/generated, source-of-truth role, owner/module, read status, linked requirements, inbound/outbound references, test coverage evidence, security classification, disposition/notes.

## ۳. ترتیب خواندن مستندات
ابتدا اگر موجودند و به همین ترتیب: `PROJECT_SPEC.md`; `docs/10-master-ai-coding-directive.md`; `docs/01-business-workflows.md`; `docs/02-data-model.md`; `docs/03-accounting-settlement-rules.md`; `docs/05-offline-windows7-architecture.md`; `docs/08-testing-acceptance.md`. برای هر مسیر status دقیق `READ_FULL`, `READ_PARTIAL` (با line/range), `NOT FOUND`, `ACCESS DENIED`, `TOO LARGE/DEFERRED` باشد. سپس تمام README/MD/MDX, ADR, docs index, issue/PR templates, workflows, test instructions, release docs و prompt files را بخوان. فایل‌های قدیمی از قلم نیفتند؛ با snapshotهای مالک تطبیق داده شوند.

## ۴. کد و کانفیگ
- Inventory entry points, routes, screens, components, stores, services, domain functions, API handlers, DTO/schema validators, repositories, DB migrations, report queries, import/export, sync, backup/restore, print, device adapters, error/logging and auth.
- برای هر مسیر کاربر یک trace بنویس: UI event → validation → command/API → domain rule → transaction/persistence → audit/outbox → query/report → print/export → tests.
- از search/index های repo برای symbols استفاده کن؛ نام symbol، مسیر، call site و test reference را evidence بده. فایل بزرگ را کامل یک‌باره در context نادیده نگیر؛ بخش‌بندی و read ranges را ثبت کن.
- package manifest و lockfileها، tool versions, runtime targets, env example, compiler config, lint/test/build configs, container/CI/deploy/installer scripts را بررسی کن. dependency list alone implementation evidence نیست.

## ۵. فایل‌های باینری، تولیدشده و خطرناک
- Binary (png/svg/pdf/xlsx/zip/db/exe) را از نظر file type, bytes, SHA-256, origin/use, references, sensitivity classify کن. SVG را هم از نظر markup/links بررسی کن اگر متن آن قابل‌خواندن است.
- generated folders (`node_modules`, `dist`, `coverage`, caches) را برای فهم file count گزارش کن اما source audit را بر اساس tracked files انجام بده؛ دلیل skip را ذکر کن.
- اسرار را بازگو نکن. فقط path، نوع خطر و اقدام پیشنهادی (rotate/remove from history/ignore) ثبت شود. اگر واقعیات شخصی/بانکی یا اطلاعات کسب‌وکار در tracked file موجودند، محتوا را redacted نگه دار و merge را block کن تا owner آن را امن اصلاح کند.

## ۶. تشخیص سند و قابلیت معتبر
هر گزاره با evidence level مشخص شود: `E1 source path read`, `E2 symbol/call-site traced`, `E3 test found`, `E4 test actually run with command/exit`, `E5 UI observed in live build`. «صفحه موجود است» بدون E2/E3 اثبات پایان کار نیست. screenshot صرفاً concept، evidence E5 نیست. Test file موجود با test اجراشده فرق دارد.

## ۷. خروجی‌های ممیزی
A. repo identity + snapshot; B. complete file inventory artifact; C. core legacy path matrix; D. doc conflict and supersession map; E. architecture/data-flow graph; F. module/API/DB dependency map; G. capability verification matrix; H. test matrix and commands; I. security/privacy risks; J. requirement traceability; K. orphan/unreferenced file report; L. recommended minimal changes ordered by risk. از فایل‌های واقعی گزارش یک نسخه قابل بازبینی بساز، ولی در مرحله read-only آن را در repo ذخیره نکن مگر owner برای مرحله مستندسازی branch جداگانه را تأیید کرده باشد.

## ۸. حالت‌های توقف
- Connector به repo دسترسی ندارد → `BLOCKED`, بدون حدس.
- branch/commit مشخص نیست → identity unresolved; هیچ مقایسه‌ای با commit دیگر انجام نده.
- فایل موردنیاز نیست → `NOT FOUND`, پیشنهاد mapping; فایلی با همان نام جعلی نساز.
- اسکریپت احتمال write دارد/اتصال واقعی به DB دارد → اجرا نکن و `NOT RUN`.
- secrets or business data detected → خلاصه redacted و issue/blocker, نه paste value.

## معیار قبول
هیچ tracked file بی‌طبقه‌بندی نماند؛ همه مسیرهای core statusدار باشند؛ گزارش از شواهد قابل تکرار باشد؛ خواندن partial صادقانه label شود؛ dependencies و test status مشخص باشند؛ هیچ تغییری در phase read-only انجام نشده باشد.
