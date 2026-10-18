# ماتریس رویداد تا خزانه/موجودی/دفترکل — قرارداد ثبت حسابداری

> این ماتریس برای تعیین اثرهاست، نه کدینگ نهایی. عناوین حساب باید از PostingTemplate و Chart of Accounts واقعی resolver شوند. مثال‌ها ساده‌اند و مالیات، برگشت، ارز، تخفیف یا سیاست خاص ممکن است خطوط اضافه داشته باشند. ثبت مالی واقعی بدون بازبینی حسابدار و کدینگ مصوب Go-Live نمی‌شود.

## جدول اثرهای متعارف
| رویداد منبع | اثر عملیاتی/موجودی | اثر خزانه | الگوی GL نمونه | شرط اثر قطعی و منع دوباره‌شماری |
|---|---|---|---|---|
| قبض باسکول پیش از فاکتور | ثبت وزن/ورود موقت یا انتظار؛ به‌تنهایی ورود موجودی فقط اگر قرارداد عملیات چنین مقرر کند | ندارد | ندارد یا Draft proposal | مقدار باسکول به تنهایی خرید/بدهی نیست؛ سند ورود قطعی مشخص باشد |
| خرید نسیه با ورود کالا | inbound movement طبق سند/روش تایید | ندارد | بدهکار موجودی/خرید طبق سیاست؛ بستانکار payable طرف | فاکتور و قبض لینک؛ فقط یک movement و یک payable |
| فاکتور خرید دستی بدون قبض قبلی | هنگام Post یک inbound movement از سطرهای فاکتور ایجاد شود | ندارد | همان الگوی خرید | idempotency و لینک دوطرفه؛ اگر movement قبلی وجود دارد دوباره ساخته نشود |
| برداشت باغ خودی | inbound harvest movement؛ ممکن است هزینه برداشت/مرکز هزینه داشته باشد | ممکن است پرداخت هزینه جدا باشد | بدهی به تامین‌کننده خارجی به‌طور پیش‌فرض ندارد؛ هزینه به حساب/مرکز مصوب | رکورد برداشت را خرید از خود/مالک فرض نکن؛ روش هزینه تایید شود |
| فروش نسیه مستقیم | outbound movement یا reservation/dispatch طبق وضعیت و policy | ندارد | بدهکار receivable؛ بستانکار sales/contra/tax طبق policy؛ COGS جدا بر اساس cost engine | درآمد، خروج موجودی و دریافت وجه سه اثر متمایز؛ هر کدام idempotent |
| فروش نقدی | خروج کالا | treasury receipt هم‌زمان ممکن است، ولی رویداد جدا | همان GL فروش + GL دریافت/settlement یا posting template مرکب اما قابل trace | یک کلیک می‌تواند هر دو را ایجاد کند ولی هر اثر فقط یک بار و دو شناسه رویداد |
| پیش‌دریافت پیش از فاکتور | ندارد | receipt | بدهکار bank/cash؛ بستانکار customer advance/unallocated receipt | درآمد محقق نمی‌شود تا فاکتور/شرط انتقال مشخص؛ تخصیص بعدی receipt جدید نیست |
| دریافت تخصیص به فاکتور | ندارد | receipt event قبلی یا تازه | بدهکار bank/cash هنگام receipt؛ تخصیص ممکن است صرفاً subledger link باشد؛ جابه‌جایی advance به receivable/payable طبق policy | از تخصیص مجدد اثر بانک نساز؛ GL posting policy صریح باشد |
| پرداخت بدهی تامین‌کننده | ندارد | payment | بدهکار payable طرف؛ بستانکار bank/cash یا clearing account برحسب instrument | چک صادره با پول تسویه‌شده یکسان نیست؛ lifecycle حاکم است |
| چک دریافت‌شده | ندارد | cheque instrument event; not necessarily cleared cash | حساب کنترل چک/دریافتنی طبق policy | تا وضعیت وصول تایید نشده در cash-cleared جمع نشود |
| واگذاری چک دریافتی به بانک | ندارد | cheque lifecycle transition | انتقال بین حساب کنترل چک/وجوه در راه طبق سیاست مصوب | اصل چک به دریافت جدید تبدیل نشود |
| برگشت چک | ندارد | failed/returned cheque state | reversal/adjustment مناسب و بازگشت بدهی/طلب طبق policy | اثر قبلی حذف نشود؛ به ثبت اولیه و علت لینک شود |
| انتقال حساب بانکی خود سازمان | ندارد | یک transfer مرتبط با دو سمت | بدهکار حساب مقصد؛ بستانکار حساب مبدا (کارمزد جدا) | هیچ فروش/هزینه/درآمد؛ صورت جریان نقدی با classification درست |
| Bank statement import | هیچ حرکت موجودی خودکار | raw statement line | بدون GL خودکار | match پیشنهادی نیاز تایید؛ اگر با پرداخت موجود match شد GL دوم ممنوع |
| کارمزد بانک | ندارد | component of transaction | بدهکار هزینه کارمزد؛ بستانکار بانک/حساب واسط | مدرک بانک و source-line حفظ؛ مبلغ اصلی جدا از fee |
| برگشت از فروش | inbound return movement طبق دریافت کالا | refund ممکن است بعداً | reversal/contra revenue و inventory/COGS طبق policy | سند برگشت مرتبط با فاکتور اصلی؛ مبلغ منفی دست‌ساز در سطر فروش جایگزین نشود |
| برگشت خرید | outbound supplier return movement | refund/credit note ممکن است | بدهکار payable/cash حسب حالت؛ بستانکار موجودی/خرید طبق policy | ربط به purchase original و اثر دوباره ممنوع |
| سورتینگ خام به درجه‌ها | کاهش ورودی خام و افزایش هر درجه + پرت واقعی/WIP | ندارد | هزینه/ارزش بین طبقات طبق cost policy؛ waste account فقط اگر مصوب | مجموع وزنی ورودی/خروجی و costing مستقل از افت قراردادی |
| ضایعات واقعی | movement نوع physical waste | ندارد | حساب ضایعات/COGS طبق policy و تایید | دلیل/وزن/مسئول/مرجع؛ با افت قرارداد یا اختلاف count یکی نیست |
| تعدیل موجودی | adjustment movement تأییدشده | ندارد | حساب تعدیل مصوب در برابر موجودی | گزارش discrepancy خودکار Journal نسازد؛ approval و count لازم |
| صافی بارفروش تأییدشده | تطبیق بارها/فروش گزارش‌شده/کسورات | پرداخت تخصیص‌یافته یا آزاد جدا | posting template بارفروش؛ حساب‌های بدهی/طلب و کمیسیون/هزینه از rule | هر صافی source UUID/نسخه؛ Draft/approval/Post جدا؛ amendment به اصل لینک |
| تسویه جزئی یک بار | مانده open بار کاهش می‌یابد | payment allocation | اثر GL دریافت/پرداخت فقط برای treasury؛ allocation مانده معین را طبق policy تطبیق می‌دهد | مانده هر محموله و aggregate safi جدا؛ مجموع تخصیص بیش از مانده ممنوع |
| هزینه نقدی عملیاتی | ندارد مگر مصرف موجودی مواد | payment | بدهکار هزینه/دارایی مطابق ماهیت؛ بستانکار نقد/بانک | رسید/دسته هزینه/مرکز هزینه؛ دریافت بانکی raw خودکار هزینه محسوب نشود |
| پیش‌پرداخت به تامین‌کننده | ممکن است هیچ موجودی تا تحویل نباشد | payment | بدهکار advance; بستانکار bank/cash | هنگام دریافت فاکتور، reclass/reconcile با تخصیص؛ پرداخت دوم ساخته نشود |
| سند دستی journal | طبق lineهای مصوب؛ ممکن است موجودی/خزانه مستقیم نسازد | هیچ مگر عملیات تخصصی | خطوط debit/credit متوازن | دسترسی ویژه؛ account control dimensions؛ بدون دور زدن فرایندهای تخصصی بانک/موجودی |
| بستن/بازگشایی دوره | ندارد | ندارد | فقط closing rules; carry-forward حسب policy | دوره بسته posting نمی‌پذیرد؛ reopen دارای audit و مجوز |

## قرارداد محاسبه `PostingTemplate`
حداقل کلیدها: `template_id`, `version`, `source_event_type`, `conditions`, `effective_from/to`, `debit_account_resolver`, `credit_account_resolver`, `amount_source`, `currency_policy`, `rounding_policy`, `required_dimensions`, `approval_policy`, `post_timing`, `reversal_template`, `idempotency_scope`, `report_mappings`, `author`, `reviewer`, `status`. template منتشرشده immutable است؛ نسخه جدید می‌سازد. تست هر نسخه شامل happy path، missing account/dimension، duplicate source, reversal, closed period, multi-currency and rounding boundary است.

## پیش‌شرط Go-Live مالی
- chart of accounts حقیقی و account mappings تایید شده؛ account opening balances وارد و reconcile شده.
- واحد پول/تاریخ/گردکردن و قواعد مالیاتی/فاکتور طبق نیاز واقعی تایید شده.
- روش قیمت تمام‌شده و تخصیص هزینه سورت/حمل/نگهداری تایید شده.
- گردش چک، بارفروش، پیش‌دریافت، حساب واسط و برداشت مالک توسط حسابدار بررسی شده.
- کنترل جمع subledger ↔ GL و balance sheet/P&L/cash flow در داده آزمون سازگار.
