# طرح بررسی Excelهای قدیمی و نگاشت داده‌های مازندشت

## قبل از Import
هیچ workbook واقعی را مستقیماً به محیط production وارد نکنید. یک کپی محدود/ناشناس‌شده در محیط staging تهیه شود. فایل‌های اصلی read-only حفظ شوند و hash، زمان دریافت، نام شیت، تعداد سطر/ستون، فرمول‌ها، named ranges، hidden rows/columns، merged cells و نوع داده ستون‌ها ثبت شود.

## فهرست کشف هر workbook
| فیلد | ثبت شود |
|---|---|
| Workbook ID | شناسه پرونده مهاجرت، نه نام فایل به‌تنهایی |
| Filename/version/date | نام، نسخه/تاریخ دریافت و checksum |
| Sheet | نام دقیق و وضعیت hidden/visible |
| Business owner | شخصی که معنی ستون/فرمول را تایید می‌کند |
| Row semantics | هر ردیف نماینده شخص، سطر فاکتور، محموله، پرداخت یا snapshot است؟ |
| Keys | شناسه واقعی، کد محلی، شماره سند؛ آیا تکرارپذیر است؟ |
| Money/weight units | ریال/تومان، kg/تن، gross/net/tare؛ شواهد معنای اعداد |
| Dates | شمسی/میلادی، تاریخ ثبت/تاریخ رویداد، time zone و format |
| Formula dependency | فرمول‌ها، hard-code و وابستگی بین sheetها |
| Candidate canonical entity | Party/Product/Load/Invoice/Payment/Settlement/Movement/Journal |
| Mapping decision | مستقیم/تبدیل/شکافتن به چند موجودیت/عدم مهاجرت/نیاز به بررسی |
| Validation/control total | count، sum وزن، sum ریال، duplicate counts و reconciliation |
| Approval | بازبین عملیات/مالی و نتیجه |

## نگاشت پیشنهادی نام‌های شناخته‌شده
`Harvest/برداشت` را ابتدا به ورود از باغ و/یا قبض باسکول نگاشت کنید، نه به Purchase payable به‌طور خودکار. `Transfer/انتقال` ممکن است انتقال انبار/محموله باشد؛ مشخص شود آیا جفت ارسال/دریافت دارد. `Morteza Sales` و `Qasem Sales` نام sheet فروش هستند ولی نقش seller/warehouse/owner باید از داده و گردش واقعی تایید شود. `Settlements/تسویه` ممکن است صافی بارفروش باشد یا پرداخت/settlement؛ نوع هر ردیف از ستون/سند منبع تعیین شود. `Dashboard` عموما خروجی aggregation است و به‌عنوان سند اصلی import نشود. `INFO` احتمالاً داده پایه است؛ duplicate/key relationships بررسی شود. اینها فرضیه نگاشت‌اند نه حقیقت تاییدشده.

## تطبیق و گزارش مهاجرت
پیش/پس از واردسازی: تعداد طرف‌حساب یکتا؛ کالاها/انبارها/سبدها؛ تعداد و مجموع وزن قبض‌ها/فاکتورها؛ فروش/خرید به تفکیک بازه؛ مبالغ دریافت/پرداخت؛ مانده باز هر شخص؛ ردیف‌های صافی و هزینه‌ها؛ موجودی تجمعی/کسری؛ bank statement rows؛ چک‌های باز؛ بدهکار/بستانکار؛ حساب‌های واسط. اختلاف صفر فرض نشود؛ اختلاف‌ها بر اساس علت (duplicate، missing link، unit conversion، rounding، legacy formula) دسته‌بندی شوند. مهاجرت واقعی فقط پس از dry-run و sign-off انجام شود.

## قانون شناسه
شناسه legacy در فیلد `legacy_source_id` و نگاشت به UUID مرجع حفظ شود. اگر کلید مطمئن نیست، با composite identity و review استفاده شود. شماره ردیف Excel شناسه یکتای تجاری نیست مگر رابطه و پایداری آن اثبات شده باشد. هیچ رکورد مالی صرفا از روی شباهت نام خودکار merge نشود.
