<div dir=rtl>

#  جدول کدهای وضعیت آموت‌ اس‌ام‌اس

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## معرفی

این فایل، مرجع اصلی تمامی **کدهای وضعیت (Status Codes)** در وب‌سرویس‌های آموت‌SMS است. هدف از این مستند، یکسان‌سازی ساختار پاسخ‌ها در تمام APIها و تسهیل در تفسیر خروجی‌ها توسط توسعه‌دهندگان می‌باشد.

هر پاسخ از سمت وب‌سرویس ممکن است با HTTP 200 بازگردد اما درون بدنه‌ی پاسخ، وضعیت واقعی عملیات از طریق فیلد `Status` و `Title` مشخص می‌شود.

---

## ساختار فیلدها

| فیلد       | نوع داده      | توضیح                                                                                    |
| ---------- | ------------- | ---------------------------------------------------------------------------------------- |
| **Status** | عددی (int)    | شناسه یکتا و عددی وضعیت. مثبت برای وضعیت‌های عادی، منفی برای خطاهای سیستمی و احراز هویت. |
| **Title**  | متنی (string) | کلید ثابت و انگلیسی برای استفاده در برنامه‌ها.                                           |
| **شرح**    | متنی (string) | توضیح فارسی وضعیت برای نمایش یا گزارش.                                                   |

---

## جدول وضعیت‌ها

| Status | Title                             | شرح                                           |
| ------ | --------------------------------- | --------------------------------------------- |
| 0      | Failed                            | عملیات ناموفق                                 |
| 1      | Success                           | عملیات موفق                                   |
| 2      | AccountIsDemo                     | کاربری دمو می‌باشد                            |
| 4      | CreditNotEnough                   | اعتبار کافی نمی‌باشد                          |
| 5      | LineNumber_NotExist               | خط ارسال وجود ندارد                           |
| 6      | BackupLineNumber_NotExist         | خط ارسال پشتیبان وجود ندارد                   |
| 7      | Avanak_NotAvailable               | سرویس آوانک در دسترس نمی‌باشد                 |
| 10     | UserName_Empty                    | نام کاربری خالی می‌باشد                       |
| 11     | Password_Empty                    | گذرواژه خالی می‌باشد                          |
| 12     | LineNumber_Empty                  | خط ارسال خالی می‌باشد                         |
| 13     | BackupLineNumber_Empty            | خط ارسال پشتیبان خالی می‌باشد                 |
| 14     | SMSMessageText_Empty              | متن پیامک خالی می‌باشد                        |
| 15     | AvanakMessageText_Empty           | متن پیام صوتی خالی می‌باشد                    |
| 16     | Mobile_Empty                      | شماره موبایل خالی می‌باشد                     |
| 17     | Mobiles_Empty                     | لیست شماره موبایل خالی می‌باشد                |
| 18     | Title_Empty                       | عنوان خالی می‌باشد                            |
| 19     | FirstNameOrLastName_Empty         | نام یا نام خانوادگی خالی می‌باشد              |
| 20     | URLAddress_Empty                  | آدرس URL خالی می‌باشد                         |
| 100    | UserNameOrPassword_Invalid        | نام کاربری ویا گذرواژه اشتباه می‌باشد         |
| 101    | Mobile_Invalid                    | شماره موبایل اشتباه یا لیست سیاه می‌باشد      |
| 102    | Mobiles_Invalid                   | لیست شماره موبایل اشتباه یا لیست سیاه می‌باشد |
| 103    | Count_Invalid                     | تعداد اشتباه می‌باشد                          |
| 104    | FromRow_Invalid                   | ردیف شروع اشتباه می‌باشد                      |
| 105    | FromDate_Invalid                  | تاریخ شروع اشتباه می‌باشد                     |
| 106    | FromDateTime_Invalid              | زمان شروع اشتباه می‌باشد                      |
| 107    | ToDate_Invalid                    | تاریخ پایان اشتباه می‌باشد                    |
| 108    | ToDateTime_Invalid                | زمان پایان اشتباه می‌باشد                     |
| 109    | FromDateIsAfterThanToDate         | تاریخ شروع جلوتر از تاریخ پایان می‌باشد       |
| 110    | FromDateTimeIsAfterThanToDateTime | زمان شروع جلوتر از زمان پایان می‌باشد         |
| 111    | MessageID_Invalid                 | کد پیام اشتباه می‌باشد                        |
| 112    | BulkID_Invalid                    | کد بانک اشتباه می‌باشد                        |
| 113    | ContactID_Invalid                 | کد مخاطب اشتباه می‌باشد                       |
| 114    | ContactGroupID_Invalid            | کد گروه مخاطب اشتباه می‌باشد                  |
| 115    | CourseID_Invalid                  | کد ارسال دوره‌ای اشتباه می‌باشد               |
| 116    | CourseGroupID_Invalid             | کد گروه ارسال دوره‌ای اشتباه می‌باشد          |
| 117    | URLAddress_Duplicate              | آدرس URL تکراری می‌باشد                       |
| 118    | RelayMessageDeliveryID_Invalid    | کد انتقال دلیوری پیام اشتباه می‌باشد          |
| 119    | RelayRecieveMessageID_Invalid     | کد انتقال دریافت پیام اشتباه می‌باشد          |
| 120    | Length_Invalid                    | طول اشتباه می‌باشد                            |
| 121    | Length_Exceeded                   | طول بیستر از حداکثر می‌باشد                   |
| 500    | ServerError                       | خطای سرور                                     |
| -300   | System_Disabled                   | سامانه غیرفعال می‌باشد                        |
| -208   | Token_NotAllowMethod              | متد غیرمجاز                                   |
| -207   | Token_NotAllowIP                  | آی پی غیرمجاز                                 |
| -204   | Token_Expired                     | توکن منقضی شده است                            |
| -203   | Token_Disabled                    | توکن غیرفعال است                              |
| -202   | Token_Invalid                     | توکن اشتباه می‌باشد                           |
| -201   | Token_NotExists                   | توکن وجود ندارد                               |
| -109   | User_NotAllowHttp                 | عدم مجوز استفاده از پروتکل HTTP               |
| -108   | User_NotAllowMethod               | متد غیرمجاز                                   |
| -107   | User_NotAllowIP                   | آی پی غیرمجاز                                 |
| -106   | User_NotAllowWebService           | عدم مجوز استفاده از وب سرویس                  |
| -105   | User_WebServiceBanned             | عدم مجوز استفاده از وب سرویس                  |
| -104   | User_Expired                      | کاربری منقضی شده است                          |
| -103   | User_Disabled                     | کاربری غیرفعال می‌باشد                        |
| -102   | User_MobileNotVerified            | کاربری احراز هویت ندارد                       |
| -1     | User_NotExists                    | نام کاربری یا گذرواژه اشتباه می‌باشد          |


</div>