# اجرای کامل Mazand FOS در Arena.ai — دفترچه ترتیبی پرامپت‌ها از ۰۰ تا ۲۵

> این سند یک Runbook واحد است: مراحل شروع GitHub، اتصال Arena و **متن کامل همه ۲۶ پرامپت** را دقیقاً به ترتیب دارد. برای اجرا، هر بار فقط بلوک همان مرحله را کپی کن. این پرامپت‌ها خودکار اجرا نمی‌شوند؛ قراردادن فایل در GitHub به معنای اجرای آن نیست.

## بخش A — تنظیم GitHub از صفر

1. در Create repository، Owner حساب خودت را انتخاب کن. نام فنی پیشنهادی `mazand-fos` و نام محصول در UI `Mazand FOS` است.
2. Visibility را **Private** بگذار.
3. برای مخزن کاملاً جدید، پیشنهاد ساده‌تر این کیت: Add README = **Off** چون ZIP خودش `README.md` دارد؛ Add .gitignore = **No .gitignore** چون این بسته `.gitignore` مخصوص دارد؛ License = **No license** چون فعلاً پروژه اختصاصی است.
4. اگر مخزن کاملاً جدید است، پیشنهاد این کیت: Add README = **Off** تا README داخل ZIP دوباره‌کاری نشود؛ Add .gitignore = **No .gitignore** چون نسخه سفارشی در بسته وجود دارد؛ License = **No license** چون پروژه فعلاً اختصاصی است.
5. اگر قبلاً مخزن را با README روشن ساخته‌ای، مخزن را حذف نکن و README را کورکورانه overwrite نکن؛ README فعلی را backup/compare و سپس merge کن.
6. Create repository و URL واقعی را یادداشت کن. در ویندوز PowerShell می‌توانی از الگوی زیر استفاده کنی؛ اگر Owner یا نام مخزن متفاوت است فقط URL را با مقدار واقعی جایگزین کن:

   ```powershell
   $Repo = "$HOME\source\mazand-fos"
   $Zip = "$HOME\Downloads\Mazand_FOS_Complete_Prompt_Sequence_v3_FA.zip"
   $Extract = "$HOME\Downloads\Mazand_FOS_Kit_v3"
   New-Item -ItemType Directory -Force -Path "$HOME\source" | Out-Null
   Expand-Archive -LiteralPath $Zip -DestinationPath $Extract -Force
   git clone "https://github.com/eisapourmajid-ai/mazand-fos.git" $Repo
   Copy-Item -Path (Join-Path $Extract "*") -Destination $Repo -Recurse -Force
   Set-Location $Repo
   python scripts/validate_docs.py
   if ($LASTEXITCODE -ne 0) { throw "Documentation validation failed" }
   git status --short
   git add .
   git diff --cached --check
   git status --short
   ```

   قبل از اجرا باید ZIP واقعاً در مسیر `$Zip` باشد. اگر GitHub owner یا slug متفاوت است، URL را اصلاح کن. فرمان `Copy-Item` فایل‌هایی با نام برابر را overwrite می‌کند؛ این مسیر را فقط برای repository جدید/خالی پیشنهاد می‌کنیم. اگر مخزن قبلاً README یا فایل‌های دیگری دارد، ابتدا تفاوت‌ها را بررسی و ادغام کن.
7. پیش از commit تأیید کن که هیچ فایل واقعی Excel/INFO، دیتابیس، backup، `.env`, token، کلید خصوصی یا گردش بانک در staging نیست. `git status --short` را بررسی و فقط پس از تأیید خودت commit کن.
8. برای repository خالی، پس از review می‌توانی branch اولیه را تعیین و baseline اسناد را ثبت کنی:

   ```powershell
   git branch -M main
   git commit -m "docs: add Mazand FOS baseline and sequential AI workflow"
   git push -u origin main
   ```

   اگر commit نام نویسنده می‌خواهد، `git config user.name` و `git config user.email` را فقط با مقادیر واقعی خودت تنظیم کن. هرگز `push --force` نکن. در repo موجود از branch/status واقعی پیروی کن و این دستور را کورکورانه اجرا نکن.
9. سپس در GitHub شاخه `main` را محافظت کن اگر امکانات plan اجازه می‌دهد. required status checks را قبل از تأیید نام دقیق workflow/check اجباری نکن.
10. GitHub Connector را در Arena.ai متصل و دسترسی به همان repo خصوصی را فعال کن. در هر prompt از Agent بخواه مسیر فایل را واقعاً باز کند؛ اگر نتوانست، باید `BLOCKED` بگوید.

## بخش B — قاعده اجرای مرحله‌ها

- تمام پرامپت‌ها در زیر آمده‌اند؛ copy/paste مستقیم امکان‌پذیر است. لازم نیست Arena از حافظه مکالمه این متن را حدس بزند.
- مرحله ۰۰ قرارداد را تعیین می‌کند؛ مرحله ۰۱ ممیزی read-only است.
- بعد از هر پرامپت، گزارش را بررسی کن. ادامه تنها با پیام جداگانه مالک مثل «گزارش مرحله ۰۱ تأیید است؛ مرحله ۰۲ را اجرا کن» مجاز است.
- تا پایان ۰۶ **کدنویسی محصول ممنوع**. مرحله ۰۷ به بعد نیز فقط با تأیید روشن شروع کدنویسی است.
- هیچ مرحله‌ای را خودکار به مرحله بعد chained نکن. اگر context نشست پر شد، نشست تازه بساز، قرارداد سراسری را دوباره بده، report آخرین مرحله را منتقل کن و شماره مرحله بعد را صریح مشخص کن.
- اگر Agent نتوانست GitHub file را بخواند، محتوای همان فایل را محلی باز کن و در prompt دستی paste کن. به پاسخ ادعایی Agent بدون مسیر/شواهد اعتماد نکن.

## بخش C — وضعیت ثبت اجرا

برای هر مرحله این موارد را در `tests/PROMPT_EXECUTION_LEDGER_FA.md` یا tracker خودت ثبت کن: شماره prompt، زمان، repo URL، branch/commit، ورودی/شواهد، status، owner approval، مسیر گزارش، فایل‌های تغییرکرده، commands/test outputs، blockers و next step. هیچ `READY` بدون evidence به `DONE` تبدیل نشود.

---

# بخش D — تمام پرامپت‌ها به ترتیب



## PROMPT 00 — قرارداد سراسری و نحوه استفاده از ایجنت

**فایل مستقل متناظر:** `prompts/00_GLOBAL_CONTRACT_FA.md`  
**Mode:** `NO-WRITE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 00 — قرارداد سراسری و نحوه استفاده از ایجنت

**Mode:** `NO-WRITE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

هدف این مرحله فقط تنظیم قرارداد رفتاری است، نه بررسی یا تغییر مخزن. ابتدا خود این فایل و سپس در صورت دسترسی صرفاً برای زمینه `README.md`, `START_HERE_FA.md`, `PROJECT_SPEC.md`, `docs/INDEX_FA.md`, `docs/27_DECISION_REGISTER_FA.md` را از مخزن واقعی بخوان؛ ولی از اجرای هر پرامپت دیگر خودداری کن. قرارداد زیر را در کل نشست رعایت کن.

**نقش‌ها:** تحلیل‌گر کسب‌وکار، معمار نرم‌افزار، طراح UX/UI، مهندس داده، متخصص حسابداری دوبل، متخصص عملیات مرکبات، مهندس مهاجرت Excel، متخصص امنیت/آفلاین، QA و نویسنده مستندات.

**قانون شواهد:** نام repo/branch/commit، مسیر، schema، فناوری، تعداد تست، قابلیت پیاده‌شده یا نتیجه تست را حدس نزن. فقط چیزی را «خوانده/اجراشده/تأییدشده» بنام که ابزار واقعاً آن را باز/اجرا کرده باشد. هر ادعا باید یک مسیر، خروجی فرمان، شناسه شواهد یا توضیح محدودیت داشته باشد. تست اجرا نشده = `NOT RUN`. مسیر پیدا نشده = `NOT FOUND`. ابزار/مجوز ناکافی = `BLOCKED`.

**توقف و تأیید:** در هر نوبت فقط همین شماره را اجرا کن؛ به مرحله بعد نپر. پایان هر مرحله یکی از `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED` باشد. تأیید یک مرحله اجازه تغییرات نامحدود در مراحل دیگر نیست. تا مرحله ۰۶ کد محصول ننویس. مرحله ۰۱ کاملاً فقط‌خواندنی است. هر نوع تغییر بعدی فقط در branch جدا و پس از تأیید صریح دامنه تغییر مجاز است. `main` را مستقیم تغییر نده، force-push نکن، history را بازنویسی نکن، فایل را حذف/rename نکن مگر با نقشه مهاجرت و تأیید صریح.

**منابع مرجع:** خط مبنای مصوب مالک محصول و snapshots تصمیم‌ها را بخوان. مسیرهای قدیمی که باید اگر وجود دارند بررسی شوند: `PROJECT_SPEC.md`, `docs/10-master-ai-coding-directive.md`, `docs/01-business-workflows.md`, `docs/02-data-model.md`, `docs/03-accounting-settlement-rules.md`, `docs/05-offline-windows7-architecture.md`, `docs/08-testing-acceptance.md`. نبودن هرکدام باید دقیقاً ثبت و به سند جایگزین نگاشت شود؛ هرگز ادعا نکن فایل مفقود خوانده شده است. سپس همه فایل‌های مرتبط و رابطه آن‌ها را بررسی کن، نه فقط این فهرست ثابت.

**قواعد محصول غیرقابل نقض:** فارسی/RTL؛ پوسته تمام‌ارتفاع، فضای کار یکپارچه، نوارهای اقدام در دسترس و اسکرول داخلی جدول‌ها؛ Quick-create با حفظ draft؛ gross/tare/fruit-net/contractual-loss/actual-waste/billable جدا؛ وزن سبد یک بار کسر؛ افت قراردادی ضایعات فیزیکی نیست؛ درصد↔وزن افت با علامت manual override؛ سند قطعی دوبل متوازن است؛ Rial/Toman فقط نمایش ۱۰:۱؛ import بانک برابر posting نیست؛ موجودی بر پایه movement ledger و بدون lot تخصیص جعلی؛ offline مستقل و LAN مرکزی دو حالت جدا؛ دیتابیس روی network share به‌عنوان multi-writer استفاده نشود؛ دسترسی remote از VPN/gateway امن؛ پشتیبان زمان‌بندی‌شده/هنگام خروج/پیش از ارتقا و restore آزموده‌شده؛ خروجی Excel تا حد امکان سازگار با Office 2013؛ هر تغییر اثرسنجی و رگرسیون سراسری می‌خواهد.

**قالب پایان همه مراحل:** خلاصه، repo/branch/commit تأییدشده، فایل‌های خوانده‌شده و مفقود، اقدامات/فایل‌های تغییرکرده، دستورهای دقیق و exit code، تست‌ها و نتیجه واقعی، شواهد، فرض‌ها در برابر واقعیت، خطرها، معیار پذیرش، وضعیت نهایی و شماره مرحله بعد پیشنهادی. در همین مرحله فقط قرارداد را تأیید کن، فهرست قواعد را به‌اختصار بازگو کن؛ اگر هر چیز مبهم است توقف کن و هیچ مرحله دیگری را شروع نکن.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-00-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 01 — ممیزی فقط‌خواندنی مخزن واقعی از ابتدا تا انتها

**فایل مستقل متناظر:** `prompts/01_READ_ONLY_AUDIT_FA.md`  
**Mode:** `READ-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 01 — ممیزی فقط‌خواندنی مخزن واقعی از ابتدا تا انتها

**Mode:** `READ-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این مرحله مهم‌ترین دروازه پروژه است و فقط read-only است؛ هیچ فایلی تغییر نکند. هیچ تغییری ایجاد نکن: فایل نساز/ویرایش/حذف نکن؛ branch نساز؛ commit/push نکن؛ dependency نصب نکن؛ migration یا script ناشناخته را اجرا نکن. هدف این است که واقعیت مخزن را با مدرک بشناسیم.

**هویت مخزن:** با GitHub Connector مالک، نام، URL، visibility، default branch، branch/ref قابل مشاهده، commit SHA و زمان commit را از ابزار استخراج کن. هر فیلدی که Connector نمایش نمی‌دهد `UNAVAILABLE_FROM_CONNECTOR` بزن. اگر چند repo شبیه Mazand FOS/MazandHub وجود دارد، همگی را فهرست کن و بدون دلیل انتخاب نکن.

**درخت کامل:** فهرست کامل فایل‌های tracked مخزن را با ابزار GitHub یا `git ls-files` در clone خواندنی به دست آور؛ hidden files، workflowها، templateها، configها، docs، assets، tests، migrations، package manifests، lockfiles، build، deployment، installer و اسکریپت‌ها را شامل کن. دایرکتوری‌های vendor/generated/cache را اگر نادیده می‌گیری، دلیل و تعدادشان را ثبت کن. فایل‌های باینری را به‌جای ادعای «خواندن متن»، از نظر نوع، اندازه، hash، مصرف و حساسیت طبقه‌بندی کن. هیچ محتویات رمز یا PII را در گزارش چاپ نکن.

**ترتیب اسناد اولیه:** واقعاً فایل‌های زیر را در صورت وجود از ابتدا تا انتها بخوان: `PROJECT_SPEC.md`; `docs/10-master-ai-coding-directive.md`; `docs/01-business-workflows.md`; `docs/02-data-model.md`; `docs/03-accounting-settlement-rules.md`; `docs/05-offline-windows7-architecture.md`; `docs/08-testing-acceptance.md`. سپس READMEها، تمام فایل‌های `docs/` و هر Markdown دیگر را فهرست و خواندن آن‌ها را statusدار کن. برای فایل‌های بلند، chunk/rangeهای خوانده‌شده و rangeهای باقی‌مانده را ثبت کن؛ فایل را صرفاً با عنوان یا ابتدای آن «کامل خوانده» ننام.

**ردیابی معماری واقعی:** از entry point تا route/page/component، state management، validation، API/command handler، domain/service، repository/ORM، DB schema/migrations، logging/audit، import/export، backup/restore، sync, print, scale adapter و tests مسیر ارتباط را trace کن. صرف نام فایل شواهد اجرای قابلیت نیست. تفکیک کن: `IMPLEMENTED_VERIFIED`, `IMPLEMENTED_UNVERIFIED`, `MOCK_OR_DEMO_ONLY`, `PARTIAL`, `MISSING`, `UNKNOWN`, `BLOCKED`.

**تست و امنیت:** inventory تست‌ها، CI، lint/typecheck/build/security audit، تست integration/e2e و نحوه اتصال به test DB را گزارش کن. اجرای فرمان تنها اگر کاملاً read-only/ایمن و environment مشخص است؛ اگر شک داری `NOT RUN`. Secret را فقط با مسیر و نوع redacted گزارش کن. بررسی کن آیا فایل Excel/DB/backup/keys/env واقعی به اشتباه tracked است. UI را فقط با اجرای واقعی یا code inspection ارزیابی کن؛ screenshot زنده جعلی نساز.

**تحویل الزامی:** ۱) repo identity؛ ۲) inventory شمارش‌شده همه فایل‌ها با مسیر، نوع، اندازه، نقش، status خواندن، حساسیت و رابطه؛ ۳) جدول فایل‌های core حاضر/مفقود/معادل؛ ۴) dependency graph و data flow؛ ۵) capability matrix؛ ۶) تست‌های موجود/اجرای واقعی و exit code؛ ۷) risks با شدت و evidence؛ ۸) نقشه UX فعلی شامل RTL، full-height، scrolling، keyboard, tooltips, empty/error/offline states؛ ۹) فهرست تصمیمات/تعارض‌ها؛ ۱۰) blockers. هیچ فایل گزارش داخل repo ذخیره نکن؛ فقط پاسخ بده تا read-only واقعی بماند. در پایان `WAITING_FOR_APPROVAL` بده و توقف کن.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-01-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 02 — موجودی کامل اسناد و نقشه مهاجرت/ادغام

**فایل مستقل متناظر:** `prompts/02_DOCS_INVENTORY_AND_MIGRATION_FA.md`  
**Mode:** `ANALYSIS-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 02 — موجودی کامل اسناد و نقشه مهاجرت/ادغام

**Mode:** `ANALYSIS-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

فقط بعد از اینکه گزارش مرحله ۰۱ را مالک تأیید کرد اجرا کن. از repo، branch و commit تأییدشده مرحله ۰۱ استفاده کن و هویت را دوباره verify کن. این مرحله هنوز فقط تحلیل است؛ فایل‌ها را تغییر نده.

تمام فایل‌های مستندات را از inventory مرحله ۰۱ مرور کن: root READMEها، `PROJECT_SPEC.md`, همه Markdown/MDX، docs، prompt files، comments/specs در source، diagrams، UI copy/help catalogs، workflow documents و decision snapshots. هیچ فایلی را صرفاً به‌خاطر قدیمی بودن بی‌ارزش فرض نکن. هر سند را با `canonical`, `supporting`, `legacy`, `duplicate-candidate`, `superseded`, `missing-required`, `unknown` طبقه‌بندی کن.

**نگاشت اجباری:** برای هر فایل یک ردیف بساز: `path`, `title/purpose`, `version/date if supported`, `authority`, `owner/source`, `topics`, `related modules`, `references in/out`, `overlap/conflict`, `proposed action (keep/merge/supersede/archive/create)`, `target path`, `history preservation`, `approval required`. اگر دو سند متناقض‌اند، هر دو متن/تصمیم را نقل‌قول کوتاه یا paraphrase کن، شواهد مسیر بده و نهایی‌سازی را به رجیستر تصمیم‌ها بسپار.

اسناد مرجع محصول شامل snapshots و پاسخ‌های مالک‌اند؛ آن‌ها را با مستندات repo و وضعیت نرم‌افزار یکی فرض نکن. بازنویسی به‌جای حذف تاریخچه، باید با supersession note و link به original انجام شود. برای فایل‌های کد/تنظیمات، در این مرحله فقط وابستگی به مستندات و خطر ناشی از حذف مستند مربوط را مشخص کن.

**خروجی‌ها:** `Document Inventory`; `Legacy-to-Canonical Mapping`; فهرست فایل‌های تکراری/مفقود؛ proposed docs tree؛ link checker results اگر به‌شکل read-only قابل اجراست؛ لیست dead links/orphan docs؛ برنامه مرحله‌ای ادغام؛ اسامی فایل‌هایی که تغییرشان به approval نیاز دارد. برای هر اقدام پیشنهادی شدت و علت بده. هنوز گزارش را در repo ننویس. پایان: `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-02-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 03 — شکاف نیازمندی‌ها، ردیابی و رجیستر تصمیم

**فایل مستقل متناظر:** `prompts/03_GAP_TRACEABILITY_AND_DECISIONS_FA.md`  
**Mode:** `ANALYSIS-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 03 — شکاف نیازمندی‌ها، ردیابی و رجیستر تصمیم

**Mode:** `ANALYSIS-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

با اتکا به گزارش ممیزی، موجودی سند و خط مبنای مالک، traceability را در هر دو جهت بساز؛ هیچ نیازمندی را بی‌صدا حذف یا بازتفسیر نکن. این مرحله هنوز کد را تغییر نمی‌دهد.

برای هر الزام شناسه پایدار بساز، مانند `REQ-UI-...`, `REQ-OPS-...`, `REQ-ACC-...`, `REQ-DATA-...`, `REQ-SEC-...`, `REQ-IMP-...`, `REQ-SYNC-...`, `REQ-REP-...`, `REQ-REL-...`. مسیر ردیابی: `owner source → canonical doc → screen/workflow → domain/API/entity → test → evidence/release`. وضعیت هر پیوند `PRESENT/NOT FOUND/NOT VERIFIED/CONFLICT` باشد.

بررسی خاص لازم: full-height RTL dashboard and form workbench; tooltips/help; dynamic CustomerType; citrus gross/tare/net/loss/billable; operation date per line; party statement selection/allocation; bank matching and residual; aggregate inventory without invented lot tracking; posted journal balanced; trial balance vs balance sheet; rial/toman display; currencies; Office 2013 compatibility; independent offline vs LAN server vs sync; backup at exit/schedule/pre-upgrade; scale adapter; configurable print columns/signature title; permissions; data retention; audit trail; tests; installer/rollback.

**قواعد تعارض:** اسناد قدیمی، mock UI یا پاسخ غیرمستند کد بر مصوبه مالی/عملیاتی تقدم خودکار ندارد. پاسخ خام مالک حفظ شود و تفسیر تخصصی با وضعیت `PROPOSED` ثبت شود تا approval. سند قطعی نامتوازن ممنوع؛ Draft ناقص مجاز. ضایعات واقعی و افت قراردادی جدا. واحد پول نمایشی به stored amount دست نزند. bank import خودکار posting محسوب نشود. برای پایان سال، موجودی اختلافی تنها با شمارش/علت/سند تعدیل تأییدشده بسته شود.

**خروجی‌ها:** ماتریس پوشش با gap IDs؛ top 20 blockers براساس خطر؛ پیشنهاد اولویت (P0/P1/P2/P3)؛ تصمیمات جدید `PROPOSED`; decisions قبلی `APPROVED` یا `SUPERSEDED` فقط با شواهد و ثبت رابطه؛ مواردی که نیاز به آزمون/نمونه واقعی دارند. «سؤال از مالک» فقط وقتی واقعاً غیرقابل استنتاج است؛ از سؤال غیرضروری پرهیز کن و در صورت انتخاب تخصصی، گزینه پیشنهادی و دلیلش را آشکار ثبت کن. هنوز هیچ فایل تغییر نکند. پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-03-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 04 — تصمیم معماری، مرزهای سیستم و استقرار

**فایل مستقل متناظر:** `prompts/04_ARCHITECTURE_DECISION_FA.md`  
**Mode:** `DESIGN-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 04 — تصمیم معماری، مرزهای سیستم و استقرار

**Mode:** `DESIGN-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

معماری را بر پایه repo واقعی، requirements traceability و محدودیت واقعی سیستم طراحی کن؛ فناوری محبوب را پیش‌فرض انتخاب نکن. این مرحله سند/ADR پیشنهادی است و هنوز کد یا فایل‌های repo را تغییر نده.

سه حالت را جدا طراحی کن: A) تک‌دستگاه مستقل با داده محلی؛ B) سرور کاربردی مرکزی داخل LAN با API و دیتابیس پشت سرویس؛ C) چند دستگاه دارای داده محلی که با event/change envelope و conflict queue ادغام می‌شوند. توضیح بده چه داده‌ای در هر mode authoritative است، چه قابلیت‌هایی هنگام قطع اینترنت/سرور در دسترس‌اند، چه نشان وضعیتی روی UI می‌آید و چه کارهایی به‌خاطر تعارض مالی متوقف/صف می‌شوند. دیتابیس را بین کلاینت‌ها از طریق folder share به‌عنوان multi-writer منتشر نکن. Remote access از VPN یا gateway سخت‌شده باشد؛ DB مستقیماً public نشود.

طرح لایه‌ها: presentation/UI shell؛ application/query-command layer؛ domain services؛ persistence/transactions/migrations؛ integration adapters؛ audit/observability; reporting/read models. قواعد مالی در UI تکرار مستقل نداشته باشند. تعریف کن atomicity برای ثبت فاکتور+movement+allocation+journal چطور کار می‌کند و شکست نیمه‌کاره چطور قابل کشف و تعمیر است.

فناوری‌ها را در ADR مقایسه کن (desktop shell/PWA, local DB/server DB, API/runtime, packaging, XLSX library, chart/UI stack, scale adapter) با scorecard شامل offline واقعی، RTL، full-height, Windows target، multiwriter, security, mobile, print, Office 2013, testability, maintenance, licensing. Windows 7 و Office 2013 پایان پشتیبانی دارند؛ بدون آزمون محیط واقعی، سازگاری را ادعا نکن. اگر Windows 7 اجباری است، threat model و محدودیت deployment را صریح کن.

**تحویل:** architecture diagram/ADR، mode matrix، trust boundaries، deployment topology، port/network policy، data-flow diagrams، fault handling، assumptions و rejected options، decisions that require owner approval، ADR supersession map و پیشنهاد vertical slice. هیچ implementation انجام نده. پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-04-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 05 — مدل دامنه، رابطه موجودیت‌ها، schema و migration

**فایل مستقل متناظر:** `prompts/05_DOMAIN_MODEL_AND_DB_FA.md`  
**Mode:** `DESIGN-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 05 — مدل دامنه، رابطه موجودیت‌ها، schema و migration

**Mode:** `DESIGN-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

با توجه به شواهد مخزن و ADR پیشنهادی، مدل داده را طراحی کن؛ schema فعلی را اگر خوانده نشده یا وجود ندارد فرض نکن. این مرحله فقط طراحی است.

مدل پیشنهادی باید روابط پایدار برای Party/PartyType, CustomerType, supplier/grower/barforoosh/buyer, Product/ProductCategory, BasketType/Tare, Warehouse, InventoryMovement, SortingRun, WasteEvent, Purchase/Sales Document & Line, operation dates, payment/bank transaction, allocations, Account/JournalEntry/JournalLine, fiscal period, currency/rate, check, attachment, import batch/row mapping, sync event/device, audit event, print template/version, user/role/permission, backup manifest و scale reading را ارزیابی کند. نام‌گذاری نهایی را با convention موجود هماهنگ کن.

برای هر entity: PK strategy, FK, uniqueness, nullability, status lifecycle, timestamps/time zone, business key, immutable fields, indexes, soft delete/deactivation policy, audit and retention. به‌خصوص Party type پویاست و نباید در branchهای کد hardcode شود؛ پول با decimal/minor-unit دقیق ذخیره شود؛ `displayUnit=RIAL/TOMAN` تنظیم نمایشی است نه تبدیل رکورد؛ FX دارای amount/currency/base amount/rate/rate date/source؛ `operation_date` در line از `document_date` جدا؛ basket tare snapshot شود تا تغییر master data سوابق قبلی را دست‌کاری نکند؛ posted document و journal append-only/correction-based باشند.

Inventory: movement ledger با source document, product, warehouse, quantity, direction, movement type, reason, link to source; lot nullable فقط وقتی lot identity واقعی جمع‌آوری شده؛ aggregation نباید sales را به lot جعلی منتسب کند. Sync: UUID, origin device, idempotency key, schema version, occurred_at, received_at, base revision, conflict state. Import: source file hash, workbook/sheet, row number, normalized hash, mapping version, validation result, duplicate decision and audit.

**تحویل:** ERD، data dictionary، constraints/indexes، lifecycle diagrams، transaction boundary، migration approach، backward compatibility, backup preflight, rollback and repair plan؛ نمونه schema فقط در حد طرح و با علامت `PROPOSED`. طرح را با report/Excel/print/API trace کن. کد migration تولید یا اجرا نکن. پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-05-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 06 — سیستم طراحی بصری، موکاپ‌ها و ممیزی تمام صفحات

**فایل مستقل متناظر:** `prompts/06_DESIGN_SYSTEM_VISUAL_AUDIT_FA.md`  
**Mode:** `DESIGN-ONLY`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 06 — سیستم طراحی بصری، موکاپ‌ها و ممیزی تمام صفحات

**Mode:** `DESIGN-ONLY`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

مرجع بصری و مشخصات تعامل را قبل از هر کدنویسی تثبیت کن. فایل‌های موجود `assets/design-tokens.json`, `assets/design-tokens.css`, `assets/brand/`, تمام SVGهای `assets/visuals/` و `assets/diagrams/` و `docs/46_PAGE_BY_PAGE_INTERACTION_SPEC_FA.md`, `docs/06_DESIGN_SYSTEM_FA.md`, `docs/07_UX_INTERACTION_STATES_FA.md`, `docs/08_PAGE_CATALOG_FA.md`, `docs/40_VISUAL_SPEC_DASHBOARD_FA.md` را واقعاً بخوان و با هم تطبیق بده. اگر وجود ندارند، ثبت کن؛ دارایی مفقود را ادعا نکن.

**قرارداد بصری:** RTL Persian, premium industrial citrus identity, off-white/green/teal with restrained citrus orange, clear hierarchy, legible Persian font stack, numeric typography suitable for kilograms and money, consistent spacing scale, semantic success/warning/danger/neutral, focus ring, disabled/loading/dirty/offline/conflict states. فول‌اسکرین به معنای maximize/full viewport workspace و استفاده درست از ارتفاع است، نه hide کردن OS controls یا اجبار غیرقابل خروج. Shell شامل sidebar راست، top header/context, workspace tabs/breadcrumb, page title/filter/actions, internal scroll container, persistent primary action where appropriate, status area/footer/help.

هر صفحه باید layout grid, min/max widths, responsive breakpoints, header/actions, filter bar, content zones, table column sizing/sticky header/internal scroll, density modes, empty/loading/error/permission/offline/conflict states, keyboard flow, tooltip/help, drill-down and print behavior داشته باشد. کل فرم نباید به صورت چند مرحله عمودی طولانی باشد؛ مراحل در همان workbench عوض شوند. Quick-create باید drawer/modal داشته و draft/focus/filter/selected rows را حفظ کند.

داشبورد باید KPI definitions, date scope, compare period, chart legend, click-through, freshness/source timestamp و empty/partial data را نشان دهد. هر عدد نمودار به منبع traceable باشد. برای همه صفحات کسب‌وکار visual hierarchy جدا و مناسب بساز؛ داشبورد تنها صفحه پرجزئیات نباشد.

**تحویل:** token audit table, visual gaps, page-by-page specification (حداقل صفحه‌های تمام کاتالوگ فعلی), responsive behavior, component inventory, interaction/state matrix, wireframe list, contrast/focus/a11y, screenshots only if genuinely rendered; mark concept images as concepts. کد محصول ننویس و assets را overwrite نکن. پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-06-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 07 — زیرساخت پیاده‌سازی و اولین vertical skeleton

**فایل مستقل متناظر:** `prompts/07_IMPLEMENTATION_FOUNDATION_FA.md`  
**Mode:** `CODE-AFTER-APPROVAL`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 07 — زیرساخت پیاده‌سازی و اولین vertical skeleton

**Mode:** `CODE-AFTER-APPROVAL`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این اولین مرحله کدنویسی است؛ پیش از هر write، در همین مکالمه باید تأیید صریح مالک برای **دقیقاً مرحله ۰۷ و scope زیر** وجود داشته باشد. تأیید مراحل طراحی بدون تأیید شروع کدنویسی کافی نیست. اگر تأیید قابل مشاهده نیست، `BLOCKED` شو و چیزی تغییر نده.

ابتدا repo, branch, commit و working tree را دوباره verify کن. فقط branch feature جدید بساز اگر ابزار/دسترسی مجاز است؛ `main` را تغییر نده. stage scope محدود: foundation موجود را بررسی و فقط حداقل لازم برای اجرای build/test، routing/shell scaffolding یا app bootstrap را اضافه/اصلاح کن، بدون پیاده‌سازی accounting یا business module. اگر foundation موجود مناسب است، دوباره نساز؛ مستند evidence بده و فقط لازم را تغییر بده.

قبل از تغییر، فهرست `planned files`، دلیل هر فایل، dependency impact، migration/data impact (باید none در این مرحله باشد)، tests، rollback و acceptance را اعلام کن. پس از اعلام plan در همان نوبت، فقط اگر مجوز مرحله موجود است اقدام کن؛ scope را توسعه نده. قفل نسخه dependencies، lint/format/typecheck/test/build، error boundary/logging skeleton, environment template fără secrets, config validation, test infrastructure و CI/docs check را ارزیابی کن. حتماً `.env` و داده واقعی را خارج از Git نگه دار.

**پذیرش:** clean install طبق دستور؛ توسعه build؛ تست نمونه؛ app boot؛ route not found/error visible; no secret; no unintended DB migration; `git diff --check`; dependency changes explained. گزارش تمام فایل‌های تغییرکرده و test exit codes بده و diff خلاصه کن. پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-07-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 08 — پوسته تمام‌ارتفاع RTL و داشبورد اصلی

**فایل مستقل متناظر:** `prompts/08_APP_SHELL_DASHBOARD_UX_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 08 — پوسته تمام‌ارتفاع RTL و داشبورد اصلی

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

فقط shell و داشبورد را در scope بساز/تکمیل کن؛ داده مالی واقعی یا business logic جعلی اضافه نکن. قبل از تغییر بررسی کن foundation stage 07 و approval آن برقرار باشد. تمام UI موجود را inspect کن تا تکراری‌سازی نشود.

**Shell:** RTL واقعی در document root؛ direction/text-direction صحیح همراه با logical CSS properties؛ sidebar راست قابل collapse با نام و icon دسترس‌پذیر؛ topbar شامل جستجو، branch/site/workspace context، وضعیت local/server/offline/sync queue، help, notifications, user/role؛ content workspace ارتفاع viewport منهای shell chrome؛ fixed/sticky actions فقط وقتی لازم است؛ internal scrolling برای grid/panels؛ no overall body scroll برای تکمیل کار اصلی. در 1024×768، 1366×768، 1440×900 و viewport تبلت/موبایل behavior را بررسی کن. تمام‌صفحه به معنای workspace استفاده‌کننده از فضای موجود است، نه fullscreen browser API اجباری.

**داشبورد:** KPIها باید source/definition/status/freshness داشته باشند؛ تا زمانی که API واقعی حاضر نیست، synthetic fixture واضحاً `DEMO DATA` باشد. کارت‌های حساب‌های مهم، وزن ورود/خروج/قابل‌حساب، وضعیت سورتینگ، کارهای نیازمند اقدام، چک‌ها/دریافتنی‌ها، اختلاف بانک و انبار و آخرین رویدادها. نمودارها legend, units, period, no-data/error/loading. هر KPI clickable تنها اگر مسیر drilldown واقعی هست؛ در غیر این صورت disabled با توضیح.

**UI کیفیت:** سلسله‌مراتب تایپوگرافی فارسی، contrast، focus, keyboard, reduced motion, tooltips، help drawer آفلاین، empty states با CTA، inline validation و error banner. از gradient/shadows افراطی و micro-font پرهیز. اجزای تکراری از component library موجود استفاده کنند.

تحویل: مسیر فایل‌ها، screenshots واقعی از اجرای app یا دقیقاً اعلام «screenshot not captured»، سناریوهای تست، تفاوت با موکاپ و علت، viewport coverage، accessibility findings، tests/build. فقط shell/dashboard را تغییر بده؛ پایان `WAITING_FOR_APPROVAL`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-08-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 09 — داده پایه، حساب‌ها و طرف‌حساب پویا

**فایل مستقل متناظر:** `prompts/09_MASTER_DATA_PARTIES_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 09 — داده پایه، حساب‌ها و طرف‌حساب پویا

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

ماژول Master Data باید Party, CustomerType, Supplier/Grower, Barforoosh, Buyer, Bank/Account, Product/ProductCategory, BasketType/Tare, Warehouse, Units, Currency/FX Rate, Loss Policy, Payment Terms و document numbering را با مدل دامنه مصوب پوشش دهد. ابتدا قراردادها و implementation فعلی را بررسی؛ اگر entity یا migration موجود است آن را ارتقا بده نه اینکه نسخه دوم ناسازگار بسازی.

**UX:** لیست تمام‌صفحه با جستجوی سریع، فیلتر، column chooser, saved views در صورت وجود، empty/loading/error, bulk-safe actions, pagination/virtualization, side-pane details, deactivate/reactivate, audit history. create/edit در drawer/modal در همان workspace و بازگشت به سند والد با حفظ draft. CustomerType قابل افزودن/ویرایش/غیرفعال‌سازی باشد؛ حذف سخت رکورد دارای گردش ممنوع؛ تغییر type گذشته را خودکار بازنویسی نکند. کد داخلی پایدار و عنوان‌های فارسی/انگلیسی در صورت نیاز.

**اعتبارسنجی:** unique identifiers, normalize phone/account number carefully, dates, active/inactive choices; product defaults versioned/snapshotted at transaction time. Basket tare missing defaults to zero only with conspicuous warning and an auditable override; avoid silent zero. Account mapping for journal posting explicit, never infer financial account from display text alone. custom currencies have ISO/internal stable code, name Persian/English, symbol, scale/rate metadata; display Rial/Toman is not a currency conversion.

**Integration checks:** invoice quick-create, filters/selection and draft persistence; data imports; reports; permissions; templates/print; FK constraints; audit trail. Every control gets Persian tooltips. Tests cover duplicate codes, deactivation with existing references, draft preservation, permission denial and master-data change snapshots. Only this slice, no broader refactor. Report schema/data migration and exact tests; end with gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-09-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 10 — فاکتور مرکبات و موتور دقیق وزن/افت

**فایل مستقل متناظر:** `prompts/10_CITRUS_INVOICE_WEIGHT_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 10 — فاکتور مرکبات و موتور دقیق وزن/افت

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این slice از نظر مالی حیاتی است. منطق محاسبات را در یک domain service واحد تعریف کن و بین UI، API، import و گزارش تکثیر نکن. پیش از write، specification `docs/10_CITRUS_INVOICE_WEIGHT_LOSS_FA.md`, decision register, data model and existing invoice flows را بخوان.

**برای هر ردیف:** محصول، تعداد سبد، نوع سبد، tare per basket، gross scale weight، tare total، fruit net, contractual loss %, contractual loss kg, billable weight, unit, purchase/sales price as applicable, line total, operation date, linked inbound/outbound movement, party, source receipt/scale ticket, notes, override status and audit metadata. Document date و line operation date جدا. اگر tare در master تعریف نشده، صفر پیش‌فرض با هشدار برجسته و تأیید/permission و audit؛ وزن سبد را دو بار کم نکن.

**فرمول قطعی نمونه:** gross 1500 kg; 50 basket; empty tare 10 kg/basket → tare total 500; fruit net 1000; contractual loss 1% → 10; billable 990 kg. اگر عدد 5kg در ورودی کاربر tare خالی نیست و معنی دیگری دارد، نباید فرض کنی. برای loss input: هنگام ویرایش درصد، `lossKg = fruitNet * percent / 100`; هنگام ویرایش دستی kg، درصد از همان base محاسبه شود، نشان `MANUAL_OVERRIDE` روشن، precision/rounding explicit و اختلاف‌های غیرمعقول هشدار. فقط یک طرف در هر لحظه منبع ورودی مستقیم است. Recalculate نباید با blur/refresh مقدار دستی را بی‌هشدار overwrite کند.

فروش، افت قراردادی و ضایعات فیزیکی سه نوع متفاوت‌اند. افت قراردادی فقط مقدار قابل تسویه را طبق policy تغییر می‌دهد؛ خودش movement ضایعات نیست. قیمت line total از billable weight و نرخ تعیین‌شده طبق rounding rule محاسبه شود. ذخیره snapshot فرمول، نرخ، tare، percent, weight loss, result و user override برای بازسازی تاریخی.

**UX:** workbench تمام‌ارتفاع؛ rows اضافه با +، grid داخلی؛ keyboard entry؛ tab order؛ inline calculator; totals pinned; date per line; open party statement selector؛ quick create product/basket بدون از دست رفتن invoice draft؛ warnings بدون قفل سلیقه‌ای. print/export same calculated source.

**آزمون‌های لازم:** مثال 1500/50/10→990؛ tare missing; zero gross; negative; zero baskets; percent 0/100/>100 policy; manual loss edit and percent reverse calculation; precision/rounding; master tare changes after posted invoice; concurrent edit; import same line; permissions; posted correction; no double tare; actual waste not generated from contractual loss. Report test data synthetic only and all tests actually run. Stop at slice gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-10-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 11 — گردش انبار، ورود/خروج، سورتینگ و تلفات

**فایل مستقل متناظر:** `prompts/11_INVENTORY_SORTING_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 11 — گردش انبار، ورود/خروج، سورتینگ و تلفات

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

ماژول موجودی و سورتینگ باید از movement ledger قابل ردیابی و از انواع مقدار جداگانه استفاده کند. ابتدا مدل موجود و مهاجرت‌ها را بخوان؛ هیچ انبار یا کالا را بر مبنای عنوان صفحه فرض نکن.

ورود، خروج، انتقال بین انبار، مصرف سورتینگ، خروجی درجه‌بندی‌شده، ضایعات واقعی، کسری/اضافه ناشی از شمارش، اصلاحات و برگشت را با `movement_type`, source doc/line, product, warehouse, operation date, unit, quantity, direction, reason, state, actor, audit ID ثبت کن. برگشت و اصلاح traceable باشد و رکورد immutable ledger خام به‌صورت silent overwrite حذف نشود. در صورتی که واقعیات کسب‌وکار lot/batch ID نگرفته‌اند، گزارش موجودی aggregate و عدم تخصیص جعلی lot را حفظ کن.

**تراز مقدار:** Opening + inbound + transfers-in + processing yields - outbound - transfers-out - actual waste +/- approved adjustments = calculated closing. Contractual loss در صورتی که فقط تجاری/تسویه‌ای است با ضایعات فیزیکی یکی نشود. خروج فروش بر اساس billable/inventory quantity طبق policy explicit باشد تا double deduction رخ ندهد. تمام تبدیل واحدها با conversion definition مشخص. negative stock policy از تصمیم مصوب پیروی کند: هشدار/exception و override با permission، نه silent acceptance؛ اگر transaction sequencing ناممکن بود، خطا و audit واضح.

**سورتینگ:** input weight, output grades, rework, waste, variance and operator/time/warehouse. Sum rule باید شفاف و configurable policy باشد، اختلاف گزارش شود و هیچ ضایعه‌ای به‌صورت خودکار از اختلاف گردکردن/قراردادی ساخته نشود. در پایان سال، calculated closing با physical count و valuation policy تطبیق داده شود؛ سند تعدیل پیشنهادی با علت و تأیید باشد، نه posting بی‌اجازه.

**صفحات:** stock by product/warehouse, movement ledger, transfer workbench, sorting workbench, daily balance/variance, waste analysis, count/adjustment. Drill down تا منبع. tests: ledger equation, transfers net zero across warehouses, reversal, duplicate command/idempotency, negative-stock override, sorting mass-balance exceptions, no fake lot assignment, reports reconcile to movements. پایان با شواهد.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-11-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 12 — حسابداری دوبل، دفتر کل، بستن دوره و صورت‌های مالی

**فایل مستقل متناظر:** `prompts/12_ACCOUNTING_LEDGER_STATEMENTS_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 12 — حسابداری دوبل، دفتر کل، بستن دوره و صورت‌های مالی

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این ماژول باید بر double-entry domain model باشد؛ هیچ UI total یا جدول گزارش، مرجع ثبت مستقل حسابداری نیست. همه اسناد موجود، chart of accounts, posting service, account mapping, period controls, journal schema, reports and tests را بررسی کن.

**چرخه سند:** Draft → Validated → Posted/Locked → Correction/Reversal (در صورت مجاز). Draft ناقص می‌تواند ذخیره شود؛ posted journal فقط وقتی مجاز است که جمع بدهکار=جمع بستانکار در precision تعریف‌شده، دوره باز است، account active و mapping معتبر است، و permissions/approval برقرارند. Posted را silently edit/delete نکن. Correction/reversal باید سند اصلی، علت، زمان، actor و اثر خالص را در audit حفظ کند. Idempotency جلوی posting دوباره از invoice/bank/import/sync را بگیرد.

**صورت‌ها را مخلوط نکن:** ۴ستونی تراز آزمایشی: گردش بدهکار دوره، گردش بستانکار دوره، مانده بدهکار، مانده بستانکار؛ ۶ستونی: مانده افتتاحیه بدهکار/بستانکار، گردش دوره بدهکار/بستانکار، مانده پایانی بدهکار/بستانکار. صورت وضعیت مالی/ترازنامه از طبقه‌بندی حساب‌های دارایی/بدهی/حقوق مالکانه و تاریخ مبنا می‌آید؛ سود و زیان از درآمد/هزینه و دوره؛ جریان نقد از جریان‌های دسته‌بندی‌شده؛ تغییرات حقوق مالکانه با قواعد جدا. suspense/clearing/accrual/prepayment/drawings حساب‌های موقتِ بی‌قاعده نیستند؛ نوع account و policy صریح باشد.

**کنترل‌ها:** trial balance sum; opening+movement=closing; subledger receivables/payables to GL reconciliation; inventory valuation to inventory control account; bank to book reconciliation; fiscal close lock; comparative periods; rounding policy; null and zero semantics; account classification; source lineage. currency presentation Rial/Toman display-only 10:1; amounts stored once in base unit; FX account and rate policies explicit.

**تحویل:** posting rules matrix per document type, sample balanced journals, account map, schemas, period close, reports definitions/formulas/source fields, audit and correction rules, tests. همه نمونه‌ها synthetic. هیچ سند مالی production نساز. پایان gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-12-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 13 — بانک، دریافت‌وپرداخت، چک، تخصیص و صافی بارفروش

**فایل مستقل متناظر:** `prompts/13_BANK_TREASURY_BARFOROOSH_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 13 — بانک، دریافت‌وپرداخت، چک، تخصیص و صافی بارفروش

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این ماژول سه فعالیت جدا اما linked دارد: واردکردن مدرک بانک؛ تطبیق/تخصیص به رویدادها؛ ثبت حسابداری. Import بانک به خودی خود journal نیست و رد فایل raw source حفظ می‌شود.

**Bank statement:** import batch/file hash, account, bank, statement date/time, bank transaction ID/reference, raw description, normalized description, debit/credit, balance if available, currency, row number and provenance. Duplicate detection by stable bank ID, file hash and candidate fingerprint; ambiguity requires user review. Normalization must preserve raw value. Allocation to one or multiple invoice lines, expenses, receipts/payments or other approved source events, with allocated amounts, remaining amount, linkage type, effective date, and user/approval. Partial allocations permitted per policy; sum allocations must not exceed transaction amount unless explicit split/fee/exchange policy explains it. Double allocation forbidden.

**Reconciliation:** bank balance vs ledger/book, open transactions, outstanding checks, deposits in transit, fees, and unmatched lines shown separately. Matching proposals are explainable and confidence-ranked; never auto-confirm low-confidence match silently. Items unresolved remain on dashboard with owner/age/next action. A matched bank transaction and posted journal are related but status is distinct.

**چک‌ها و خزانه:** issue/receive, due date, payee/payer, amount/currency, bank account, check number, lifecycle state, endorsement/return/bounce, settlement and reminders. Transitions audited; status does not double-post amounts.

**بارفروش و صافی:** connect shipment/weight, sale proceeds, commission, freight, fees, advances, payments and settlement period; show gross, deductions, net amount due and remaining balance with formula. User can select relevant movements/transactions from party history by date range and checkbox; selected records must appear as linked references, not copied duplicate facts. If negative balance occurs, show strong warning but do not lock every workflow; permission/role policy governs override.

Tests: bank import duplicate, two-row allocation, partial/unallocated, over-allocation blocked, correct date semantics, reconciliation, posted journal idempotency, cheque lifecycle, settlement formulas, negative party balance warning, source lineage and correction audit. Stop after report and tests.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-13-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 14 — Import Studio، Excel/CSV، INFO و Export

**فایل مستقل متناظر:** `prompts/14_IMPORT_STUDIO_EXCEL_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 14 — Import Studio، Excel/CSV، INFO و Export

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

Import/Export باید یک ابزار عمومی تطبیق ستون‌ها باشد، نه اسکریپت‌های جداگانه و غیرقابل بازبینی. قبل از توسعه، تمام نمونه‌های ساختگی و ساختار مستند workbookها را بررسی؛ اگر فایل Excel واقعی در repo است، محتوا/PII را گزارش نکن و طبق امنیت برخورد کن.

**مسیر Import:** انتخاب نوع داده و entity → بارگذاری و محدودیت فایل → نمایش workbook/sheet/header/merged cells/formula vs values/date systems/encoding → انتخاب ستون‌ها → map به canonical schema → preview با ردیف‌های نمونه → normalize/validation → duplicate detection → dependency validation/FK resolution → dry-run summary → user review → transactional commit/import batch → full audit/exportable error report. هیچ import خودکار و بی‌preview روی تراکنش مالی انجام نده.

هر row باید status بگیرد: valid, warning, error, duplicate, conflict, ready. کاربر بتواند mapping را ذخیره کند با versioning، اما drift header/column changes تشخیص داده شود. ستون‌های unmatched قابل ملاحظه باشند. برای Excel با سلول فرمولی، raw formula و cached value را از هم تشخیص بده و رفتار روشن داشته باش. نمایش شماره‌های شناسه با leading zeros حفظ شود. تاریخ شمسی/میلادی، timezone، decimal separator، Persian/Arabic digits، negative values، blank vs zero و units به‌صورت صریح normalize شوند.

**Merge/Idempotency:** طبیعی بودن duplicate با hash کافی نیست؛ entity/business key/candidate matching و confirmable resolution را تعریف کن. Import повторی نباید فاکتور یا پرداخت جدید بسازد. Merge update باید فیلدهای تغییرکرده، conflict، winner rule و before/after audit داشته باشد. Delete/overwrite bulk پیش‌فرض ممنوع؛ upsert فقط با preview و policy.

**Office 2013:** خروجی XLSX باید از featureهایی مثل dynamic arrays/XLOOKUP/LET/FILTER وابسته نباشد، مگر alternative tested. از table/filter/freeze panes/print settings/number formats, totals, validations که Office 2013 پشتیبانی می‌کند استفاده کن. Macro/XLSM فقط با امنیت و تأیید مستقل؛ macro فعال پیش‌فرض ممنوع. CSV encoding/BOM را تست کن.

تحویل: import UX, canonical mapping table, supported file matrix, file limits, security scanning, error template, duplicate/conflict strategy, Office 2013 compatibility matrix and tests. Use synthetic fixtures, not owner data. پایان gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-14-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 15 — آفلاین، LAN، Sync چنددستگاهی و صف تعارض

**فایل مستقل متناظر:** `prompts/15_OFFLINE_SYNC_MODES_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 15 — آفلاین، LAN، Sync چنددستگاهی و صف تعارض

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

سه حالت معماری مصوب را جدا پیاده‌سازی کن: independent local mode؛ central LAN API/service; independent-device offline with later exchange. UI باید اتصال، منبع داده، زمان آخرین sync موفق، pending count، error/conflict count، server/local mode و داده stale را آشکار نشان دهد.

**Envelope:** event/change UUID; tenant/site/device ID; entity type/id; event type; schema version; occurred/created/received timestamps; actor; idempotency key; base revision/causation/correlation ID; payload version; checksum; state. Server/client در هر دریافت تکراری باید همان اثر قبلی را برگرداند، نه اثر مالی دوم. Event order و clock skew را با sequence/revision سروری و نه صرف timestamp دستگاه تعیین کن.

**Conflict policy per entity:** immutable financial document/journal append-only; draft edits conflict; master data use version/review policy; stock movement is append-only event with business conflict handling; reference/master deletion uses deactivation; bank allocations require over-allocation guard; sequence numbers allocate safely on server or use device prefix when truly offline. Resolve by user review or deterministic merge only where safe. Never last-write-wins for posted monetary facts without explicit rule. Sync failures resumable and observable. Partial sync, retry, offline during commit and schema version mismatch need defined transitions.

**File-based import/export sync:** package manifest with export ID, source device, scope/date, schema version, counts, hashes/signature if available; data payload; dependencies; receipt. Validate, preview, idempotency, stage, commit transactionally, receipt and rejected/conflicted rows. Encrypt sensitive exports, expiry, avoid email/unsafe USB defaults. Imported document IDs must remain stable across devices.

**Network:** LAN server behind API; database not exposed to clients/share. Remote via VPN/gateway; if not available, disable remote—not fallback to public DB port. Show offline-ready banner, queued transaction state and unsynced watermark. Tests with two clients, duplicate package, partial import, conflict, clock drift, server restart, interrupted sync, schema mismatch and financial postings. If true multiwriter is not safe, mark feature experimental/disabled. Stop after evidence.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-15-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 16 — امنیت، نقش‌ها، ممیزی و پشتیبان/بازیابی

**فایل مستقل متناظر:** `prompts/16_SECURITY_BACKUP_RESTORE_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 16 — امنیت، نقش‌ها، ممیزی و پشتیبان/بازیابی

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

از اصل حداقل دسترسی، threat model و security-by-default پیروی کن. بررسی کن authentication, session, role/permission, server bind, logs, secret management, import validation, file upload, remote access, update chain and dependency audit کجا پیاده شده‌اند. مجوز فقط مخفی‌کردن دکمه در UI نیست؛ server/domain layer هم enforce کند.

**Permission:** role/action/resource policy برای create/edit/post/approve/reverse/export/import/backup/restore/user management/change master data; separation of duties where sensible; no default shared admin; admin-only system-level controls; denied action audited without leaking sensitive payload. posted records immutable except reversal/correction. Audit records actor, action, entity, timestamp, correlation ID, before/after redacted diff, reason where needed; audit log append-only/protected, no secrets/whole personal data logged.

**Security boundaries:** database not public; LAN binding restricted; remote VPN/gateway, strong authentication, MFA if available, TLS for remote paths, update patches/unsupported OS warnings. Windows 7 treated as unsupported/high risk, never silently marketed as secure; provide deployment exception and isolation plan if legacy OS is unavoidable. Validate import file extension/content/size, path traversal, decompression bombs, formula injection when exporting CSV/XLSX, malicious filenames, backups and restore inputs.

**Backup:** at user-requested exit when cleanly possible, schedule by configured policy, pre-upgrade/pre-migration, manual on demand; configurable destination, retention, encryption and access control. Manifest includes backup ID, application/schema versions, created time, source mode, file hashes, included/excluded locations and integrity status. Avoid silent overwrite; atomic temp->verified rename. If destination missing/full, visibly alert and preserve last known valid backup. App shutdown behavior must not claim backup completed before verified result; define forced shutdown interruption.

**Restore proof:** isolated test environment, restore to scratch DB, schema/integrity checks, counts/control totals, opening balances, journal balance, inventory movement totals, party/bank reconciliation samples, login and smoke tests. Record measured RPO/RTO and failure mode; don't claim restore possible from backup file existence alone. Tests use synthetic data and fault injection. End with changed files/test evidence/risk and gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-16-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 17 — داشبورد، گزارش‌ها، Drill-down و Data Lineage

**فایل مستقل متناظر:** `prompts/17_DASHBOARD_REPORTS_LINEAGE_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 17 — داشبورد، گزارش‌ها، Drill-down و Data Lineage

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

هر KPI و گزارش باید تعریف عملیاتی و مسیر بازگشت تا سند منبع داشته باشد. گزارش صرفاً تصویر گرافیکی یا اعداد hardcoded نیست. برای هر metric نام، business definition, equation, unit/currency, grain, filters, date basis, null/zero behavior, rounding, source tables/events, permission, refresh/freshness, drilldown target and test case را مستند کن.

**Dashboard:** وضعیت حساب‌های مهم و مانده طرف‌حساب‌ها، ورود/خروج وزن، وزن قابل‌حساب و افت قراردادی، سورتینگ و ضایعات واقعی، موجودی‌های تجمیعی، bank/cheque commitments, unmatched bank lines, unresolved sync conflicts, pending imports, backup health and exceptions. مقایسه‌ها هم‌مبنای زمانی و واحد باشند. هر card/series نشان دهد داده واقعی، delayed، partial، demo یا stale است. KPI بدون source path یا formula منتشر نشود.

**Reports catalog:** party/customer/supplier/barforoosh statements; sales/purchase; inventory movement and count variance; sorting/yield/waste; bank reconciliation and cash; cheque maturity; general ledger, journal, account ledger, 4/6 column trial balance, balance sheet/statement of financial position, profit/loss, cash flow, changes in equity, receivable/payable ageing, audit/change log, import errors/duplicates, sync state, backup and data-quality report. Trial balance را با balance sheet یکی نکن. Include filters, sorting, grouping, totals/subtotals, saved views if approved, export and print consistency.

**Report engine:** API/server-side query logic; permission filters server-enforced; no unbounded full-table load; pagination/streaming; totals on full filtered data not just current page; stable sorting and tie breakers; timezone/date range edges; Persian calendar if supported through tested calendar adapter; data lineage includes source IDs and `as-of` timestamp. UI chart values and detailed rows reconcile.

Tests: calculations vs ledger, filters, period boundaries, empty dataset, null vs zero, currency display toggle, role hiding, large data, export-print sum equivalence, clickthrough source document, stale indicator. Avoid metrics that imply lot tracing not captured. Gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-17-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 18 — Print Builder و الگوهای فاکتور/صورت‌حساب

**فایل مستقل متناظر:** `prompts/18_PRINT_BUILDER_TEMPLATES_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 18 — Print Builder و الگوهای فاکتور/صورت‌حساب

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

ساختار چاپ باید template-driven اما business logic بیرون template باشد. قالب‌ها بر اساس PartyType/CustomerType و document type انتخاب/پیکربندی می‌شوند؛ برای بارفروش، خریدار، باغدار/تأمین‌کننده و بانک‌محور، ستون‌های مناسب ارائه شود. fallback واضح در صورت نبود قالب؛ قالب جدید نباید data record یا posting logic بسازد.

**Designer:** select paper size/orientation/margins, scale/fit, page breaks, repeated header/footer, column/row visibility for printing, reorder columns, widths, formatting, number/date/unit formatting, logo/title, document number, physical دفتر reference number (شماره دفتر/نسخه کاغذی), signature blocks and editable signature title. Preview for A4/A5/Letter or defined paper sizes; layout changes based on chosen paper and printer constraints. Save templates versioned with owner/role permission; posted document keeps an immutable print snapshot or reproducible version references.

**Row/column controls:** hide from print without deleting source data; distinguish hidden, filtered-out, excluded-from-calculation. Table column chooser must not affect accounting values. Allow column widths and row density to be configured within min/max. Keep critical totals and mandatory legal/internal identifiers visible unless an authorized template specifically changes them. Signature title content configurable but audit version changes.

**Print architecture:** same source calculations as app; no re-entered formula; page totals and carry-forward rules explicit; avoid breaking one invoice line across pages if policy disallows; RTL mixed numerals and long Persian text; preview exactly resembles exported PDF/printer output. Office 2013 considerations for Excel export are separate from print renderer.

Tests: all page sizes/orientations, long party/product names, large row count, RTL, zero/negative amounts, repeated headers, hidden columns, template version, user permission, document number/ledger number visibility, print vs screen total equality. Do not claim printer driver verified unless hardware exists. Stop.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-18-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 19 — ترازو و سخت‌افزار با Adapter و Simulator

**فایل مستقل متناظر:** `prompts/19_SCALE_HARDWARE_ADAPTER_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 19 — ترازو و سخت‌افزار با Adapter و Simulator

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

سخت‌افزار واقعی هنوز نباید حدس زده شود. ابتدا موجودی پورت‌ها و تجهیزات واقعی را از کد/اسناد/کاربر موجود بررسی؛ اگر مدل ترازو/پروتکل/اتصال serial/USB/TCP نامعلوم است، adapter abstraction و simulator بساز، نه driver جعلی که ادعای اتصال واقعی دارد.

**Adapter contract:** discover device, connect/disconnect, read stable measurement, unit, gross/tare/net semantics from protocol, timestamp/device ID, quality/status, error/retry, timeout, checksum if protocol provides, calibration marker where available, duplicate/stale reading handling. Do not assume scale displays fruit gross vs load gross. UI must show live/stable/unstable/offline/error, raw reading and unit; operator explicitly accepts reading into invoice and the accepted value becomes source snapshot with operator/time/device id. Editing captured weight must be permissioned and audited.

**Safe integration:** isolate hardware code from business domain; async/non-blocking; reconnect/backoff; no arbitrary command execution; serial port selection and baud/parity only when device manual supports it. Simulator offers deterministic readings and fault injection; it must be clearly labeled `SIMULATOR`. Never treat simulated value as real scale evidence.

**Tests:** timeout, disconnect, invalid checksum, unstable measurement, kg/lb conversion if supported, zero, negative/tare scenario, repeated measurement idempotency, manual override, stale measurement, reconnect and no device. Hardware acceptance requires model, firmware, communication settings, sample logs and physical test; otherwise status `HARDWARE_NOT_VERIFIED`. Stop.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-19-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 20 — تحویل یک مسیر عمودی واقعی End-to-End

**فایل مستقل متناظر:** `prompts/20_VERTICAL_SLICE_DELIVERY_FA.md`  
**Mode:** `APPROVED-SLICE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 20 — تحویل یک مسیر عمودی واقعی End-to-End

**Mode:** `APPROVED-SLICE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این مرحله یک مسیر کامل و محدود را از UI تا domain/persistence/report/test اجرا می‌کند؛ کل سامانه را یکجا نساز. از ریسک‌ها و dependency graph استفاده کن تا یک slice با ارزش و قابل آزمون انتخاب شود؛ اگر سند قبلی slice را نامیده، همان را ادامه بده مگر evidence مانعش باشد.

مثال slice پیشنهادی در صورت نبود انتخاب: ایجاد/انتخاب Party و Product → فاکتور خرید یک ردیف با وزن/افت → stock movement → balanced journal (اگر posting scope قبلاً مصوب و account mappings آماده‌اند) → Party statement / inventory report → print preview → audit trail. اگر بخش مالی هنوز آماده نیست، scope را کوچک کن و گزارش کن، اما mock backend را production integration ننام.

پیش از coding: list touched files, dependency map, schema impact, migration, permissions, audit, errors, tests, screenshots and rollback. یک domain source of truth باشد. UI validation و server/domain validation هم‌راستا ولی domain authoritative. Transaction boundaries و idempotency کل مسیر آزمایش شود.

معیار پذیرش: new synthetic fixture can traverse every layer; source IDs remain consistent; amounts and quantities reconcile; invalid input rejected; retry creates no duplicate; role permissions enforced; refresh/restart persistence works if persistence claimed; error is visible and recoverable; test results include command, exit code, count; e2e evidence/screenshot truly captured. No feature outside slice. Stop and await approval.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-20-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 21 — بازبینی بصری، RTL، Accessibility و Responsive Regression

**فایل مستقل متناظر:** `prompts/21_VISUAL_REGRESSION_ACCESSIBILITY_FA.md`  
**Mode:** `QA-ONLY-THEN-FIX-APPROVED`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 21 — بازبینی بصری، RTL، Accessibility و Responsive Regression

**Mode:** `QA-ONLY-THEN-FIX-APPROVED`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

ابتدا ممیزی بصری/دسترس‌پذیری انجام بده و قبل از اصلاح، findings را ارائه کن. برای هر صفحه متأثر screenshot از اجرای واقعی بساز اگر اجرا و ابزار capture موجودند؛ concept/wireframe را به‌جای live screenshot جا نزن. اگر browser automation نیست، `SCREENSHOT_NOT_AVAILABLE` ثبت کن و code review را جدا برچسب بزن.

**Viewport matrix:** desktop 1440×900 و 1366×768، laptop 1280×720، tablet widths, mobile widths براساس layout requirements. بررسی: body/page scroll، full-height workspace، sidebar collapse, sticky actions, internal table scroll, viewport clipping, overflow-x, modal/drawer fit, keyboard tab/focus, RTL/LTR mixed strings, Persian digit/number columns, touch target, empty/loading/error/offline/permission/conflict states.

**Visual quality:** spacing scale, font hierarchy, line-height, contrast, semantic statuses (not color alone), chart legend and axes, table density/readability, focus ring, hover/active/disabled/loading, icon meaning and tooltip. Avoid arbitrary pixel nudges that break sibling pages; use tokens and reusable components. No low contrast grey-on-grey and no overcompressed typography for low-literacy floor operators.

**Accessibility:** keyboard-only key flows, screen-reader labels where supported, focus trapping/return in modal, semantic table labels, reduced-motion, non-color state cues, accessible errors, zoom/reflow. Document known limits and browser support. Do not claim WCAG compliance without defined tests/evidence.

Deliver per-screen checklist, severity P0-P3, screenshot paths and capture method, regression comparison, remediation plan. Only fix approved scope; rerun targeted tests and affected module regression. Stop.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-21-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 22 — پروتکل دائمی تحلیل اثر و رگرسیون برای هر تغییر

**فایل مستقل متناظر:** `prompts/22_CHANGE_IMPACT_REGRESSION_FA.md`  
**Mode:** `REUSABLE-EVERY-PR`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 22 — پروتکل دائمی تحلیل اثر و رگرسیون برای هر تغییر

**Mode:** `REUSABLE-EVERY-PR`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این پرامپت از این پس برای هر تغییر/رفع خطا/درخواست جدید دوباره استفاده می‌شود؛ قبل از edit ابتدا impact analysis تحویل بده و تا تصویب scope write نکن.

برای هر تغییر: reproduce problem; identify expected vs actual; repository/branch/commit; linked requirements/ADR; affected files/modules/contracts; inbound/outbound dependencies; DB/schema/data migration; accounting/posting/inventory consequences; API/import/export/sync consequences; permission/audit; reports/print; UI states/RTL/responsive; backup/restore; installer/version; backwards compatibility; test plan; rollback/feature flag. Search whole repo for symbol/field/route/DTO/event/report dependencies, not just immediate file. Trace every consumer and producer.

**Risk matrix:** financial integrity, data loss, migration irreversibility, security/PII, sync duplication/conflict, inventory quantity mismatch, performance, UI regression. Score likelihood/impact and explain. Define minimal diff; no broad refactor unless separately approved. Add tests that fail before fix where possible. For bugfix, include regression test. For accounting, test balanced journals, source link and reconciliation. For weight, test numeric scenarios and no double deduction. For sync/import, test idempotency/retry/conflict. For report, test source totals and filters. For UI, test screenshot/viewport and keyboard.

After implementation run test ladder: formatting/lint/typecheck; unit/domain tests; integration/DB/migrations; API contract; import/export; sync/idempotency; visual/a11y/e2e; build/package; whole relevant regression. Don't claim all tests if any are `NOT RUN`; classify skipped. Inspect final diff and `git diff --check`, no secret/data artifacts, no unintended file deletes. Report changed files, before/after behavior, commands/exit codes/test counts, screenshots, risks, migration/rollback and merge recommendation. If critical regression unresolved, block merge/release. This protocol does not itself authorize a change; wait for approval before writing.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-22-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 23 — مهاجرت داده، Installer، Update و Rollback

**فایل مستقل متناظر:** `prompts/23_MIGRATION_INSTALLER_ROLLBACK_FA.md`  
**Mode:** `RELEASE-PREPARATION`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 23 — مهاجرت داده، Installer، Update و Rollback

**Mode:** `RELEASE-PREPARATION`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این مرحله فقط پس از آزمون sliceها و تأیید owner انجام شود. اول وضعیت پشتیبان و restore drill را verify کن؛ اگر restore واقعی از scratch انجام نشده، انتشار را متوقف کن. هر migration must be versioned, deterministic, auditable and safe for actual dataset volume.

**Migration plan:** source/target schema versions, preflight checks, backup requirement, required free disk, lock strategy, estimated duration, idempotency, interruption recovery, checksum/row counts, financial invariants, incompatibility, rollback approach and forward-repair option. Never auto-delete columns/rows or rewrite posted history. Downgrade migration may be destructive; rollback may require restoring verified snapshot and must be explicit.

**Installer/update:** per target OS tested, app data path separate from program files, permissions/prerequisites, service start/stop, port/firewall, config wizard, initial admin hardening, database init/migration, logs location and diagnostics, uninstall preserving data by default, backup before upgrades, signing/verification if infrastructure available, offline installer dependency bundle if required. Never state Windows 7 compatible unless built and tested on real target environment; unsupported OS risk displayed.

**Rollback:** stop clients/service; verified backup; version mapping; restore to isolated scratch first; confirm app/schema compatible; run smoke/invariants; only then restore live in controlled window. Do not run rollback commands on production without explicit approval. Keep logs free of secrets and personal data.

**Acceptance:** clean install, upgrade from each supported schema version, interrupted migration recovery, no data loss, startup/service restore, backup and rollback rehearsal, uninstall data retention, install without internet if claimed, hash/signature if available, docs/screenshots/exit codes. Missing hardware/OS test means `NOT VERIFIED`. Gate.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-23-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 24 — آزمون پذیرش کاربر، کنترل انتشار و Go/No-Go

**فایل مستقل متناظر:** `prompts/24_UAT_RELEASE_GATE_FA.md`  
**Mode:** `REPORT-FIRST-GATE`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 24 — آزمون پذیرش کاربر، کنترل انتشار و Go/No-Go

**Mode:** `REPORT-FIRST-GATE`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

بر اساس requirement traceability و tests واقعی یک UAT plan به نقش‌های متناسب بساز: مدیر سیستم، حسابدار، مسئول خرید/فروش، مسئول انبار/سورتینگ، خزانه/بانک، مدیر عملیات و کاربر ساده کم‌سواد. داده‌ها synthetic باشد تا رفتار مالی واقعی قابل بازتولید شود.

**سناریوهای end-to-end:** login/permission; master data; purchase invoice with 1500/50/10→990; manual loss override both directions; operation date per line; party history selection/allocation; stock movement and sorting yield; sale/settlement; bank import+matching+partial allocation+unmatched; cheque lifecycle; journal post/period close/reversal; trial balance and balance sheet distinct; Riyal/Toman display-only; Excel Office 2013 compatibility sample; offline create/queue/retry/conflict; backup/restore; print configurations; scale simulator; help/tooltips; keyboard and responsive layout. Attach precondition, steps, expected results, actual results, evidence and severity.

**Release gates:** no open P0/P1; no unbalanced posted journal; no duplicate financial effect under retry; no failed restore; no unmitigated critical security issue; required migrations/backups pass; major UX workflows accessible; traceability for all must-have requirements; supported OS/device matrix honestly reported; no secrets or real business data in git/artifact. Every criterion must be Pass/Fail/Blocked/Not Run with evidence.

Initially return a report and go/no-go recommendation only. Do not tag/release, publish installer, push main or deploy without explicit approval. If any evidence absent, gate fails instead of assuming pass. Stop at `WAITING_FOR_RELEASE_APPROVAL` or `BLOCKED`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-24-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````


## PROMPT 25 — ممیزی نهایی، تحویل، مستندسازی و اثبات وضعیت

**فایل مستقل متناظر:** `prompts/25_FINAL_AUDIT_HANDOFF_FA.md`  
**Mode:** `FINAL-AUDIT`  
**قاعده:** متن داخل کادر زیر را از ابتدا تا انتها به‌عنوان یک پیام واحد در Arena.ai کپی کن.

````text
# 25 — ممیزی نهایی، تحویل، مستندسازی و اثبات وضعیت

**Mode:** `FINAL-AUDIT`  
**Scope:** Mazand FOS / repository واقعاً متصل‌شده؛ نام و شاخه را از روی شواهد verify کن.

تو این فایل را به‌عنوان دستور کامل همین مرحله اجرا کن. ابتدا کل فایل را بخوان؛ سپس فقط کار همین شماره را اجرا کن. اگر مسیر این فایل را از داخل مخزن باز کرده‌ای، ثبت کن که واقعاً همین مسیر باز شده است.

این آخرین مرحله توالی است؛ ادعاهای پایانی را فقط از شواهد جمع‌آوری‌شده بساز. repo/branch/commit را دوباره verify کن، `git status`, diff, PR/change history, CI result, test logs, migration history, artifact hashes and docs traceability را مرور کن. هر معیار بدون شواهد را `NOT VERIFIED` ثبت کن.

**ممیزی فایل به فایل:** inventory نهایی تمام فایل‌های tracked؛ هر مسیر با role, owner/source, requirement IDs, caller/consumer, test, secrets/data classification, status. گزارش فایل‌های orphaned, missing refs, duplicate docs, stale instructions, dead links, generated artifacts unexpectedly tracked, real data in git, uncommitted changes. خواندن کامل همه فایل‌های متنی مرتبط را ثابت کن و باینری‌ها را نوع/size/hash/consumer گزارش کن؛ هر چیزی را که واقعاً کامل نخوانده‌ای با partial status نشان بده.

**تطبیق الزام→پیاده‌سازی→آزمون:** must-have requirements, UI pages, API, data entities, domain events, reports, exports, sync, security, backup/restore, installer, docs. Capability status only from actual evidence. Compare all user requirements and decision register, note superseded decisions. Check formulas and accounting controls: 4/6-column trial balance vs balance sheet; reconciliation of subledger/GL/bank/inventory; currency display; loss vs actual waste; source lineage.

**تحویل:** release notes; known issues; architecture; install/upgrade/backup/restore manual; operator quick guide in Persian; admin/security guide; troubleshooting; support logs; test result with command/exit/test counts; UI screenshots captured; artifact SHA-256; database migration versions; recoverability proof; support matrix; Go/No-Go. Do not report software as production-ready if any critical evidence is missing. State what is actually implemented vs plan.

Final report must include repo URL/branch/commit, PR state, changed files, tests and exit codes, remaining risks, accepted limitations, rollback details and owner handoff. Never auto-merge or deploy. End `WAITING_FOR_FINAL_OWNER_ACCEPTANCE`.


## الزام مشترک تمام مراحل
- این شماره تنها مرحله‌ای است که در این نوبت مجاز به اجرای آن هستی؛ هیچ prompt یا مرحله دیگری را خودکار اجرا نکن.
- از شواهد فعلی مخزن استفاده کن؛ برای repo/branch/commit/فایل/تست/قابلیت هیچ چیز را حدس نزن. وضعیت‌های `NOT FOUND`, `NOT RUN`, `NOT VERIFIED`, `BLOCKED` را دقیق به کار ببر.
- اگر مرحله read-only یا design-only است، هیچ فایل یا branch را تغییر نده. اگر مرحله کدنویسی است، تأیید صریح scope و branch غیر-main را پیش‌شرط کن؛ در غیر این‌صورت متوقف شو.
- قبل از هر write مجاز، فهرست فایل‌های مورد تغییر، علت، اثر بر مصرف‌کنندگان، data/schema/migration، تست‌ها، rollback و acceptance را اعلام و در همان scope بمان.
- هیچ secret، اطلاعات بانکی واقعی، فاکتور/Excel واقعی، database یا backup عملیاتی را در Git commit نکن و محتوای secret را در گزارش paste نکن.
- پس از تغییر، diff را مرور کن؛ آزمون مربوط و رگرسیون تمام ماژول‌های متأثر را اجرا کن اگر محیط اجازه می‌دهد. هر فرمان با exit code گزارش شود؛ آزمایش اجرا نشده Pass نیست.
- خروجی شامل: هدف؛ repo/branch/commit؛ فایل‌های خوانده‌شده/تغییرکرده؛ اقدامات؛ تست‌ها/exit code؛ شواهد؛ ریسک؛ محدودیت؛ acceptance; و `STOP GATE` باشد.
- در پایان مکث کن و فقط یکی از وضعیت‌ها را بده: `WAITING_FOR_APPROVAL`, `READY_FOR_NEXT_PHASE`, `BLOCKED`. تأیید مرحله جاری مجوز اجرای خودکار مرحله بعد نیست.


---
**شناسه توقف:** `MAZAND-FOS-STAGE-25-DONE` — این شناسه فقط بعد از گزارش واقعی همین مرحله صادر شود.
````
