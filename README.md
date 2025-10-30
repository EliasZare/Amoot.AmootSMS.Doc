<div dir="rtl">

<div dir="ltr" align="center">
  <img src="/Images/logo.svg" alt="AmootSMS Logo" width="300"/>
</div>

---

#  درباره اموت SMS
پیامک آموت (پیشرفته‌ترین سامانه پیامکی ایران) یک سامانه دارای وب سرویس کامل برای ارسال و دریافت پیامک و مدیریت کامل خدمات دیگر است که به راحتی میتوانید از آن استفاده کنید.

---

##  امکانات و سرویس‌های ارائه‌شده
- ارسال پیامک تکی، گروهی و زمان‌بندی‌شده
- پشتیبانی از الگوهای پیامک و کدهای OTP
- دریافت دلیوری، گزارش ارسال و وضعیت پیامک‌ها
- مدیریت مخاطبین و گروه‌ها
- ارسال خودکار رویدادهای ارسال و دریافت به آدرس (URL) تعیین‌شده توسط کاربر

---

#  **مستندات وب سرویس**


##  ۱. احراز هویت و حساب کاربری 

- **احراز هویت**  
  🔹 نحوه احراز هویت  
  🔗 [مشاهده مستند »](./Account/authentication.md)  
  بررسی نحوه احراز هویت در آموت اس‌ام‌اس.

- **وضعیت حساب کاربری**  
  🔹 متد: `AccountStatus`  
  🔗 [مشاهده مستند »](./Account/account_status.md)  
  بررسی موجودی، تاریخ انقضا و وضعیت خط اختصاصی کاربر.

---

## ۲. ارسال پیامک 
###  حالت‌های ارسال

- **ارسال پیامک ساده**  
  🔹 متد: `SendSimple`  
  🔗 [مشاهده مستند »](./Send/send_simple.md)  
  ارسال پیامک متنی به یک یا چند شماره از خطوط عمومی یا اختصاصی.

- **ارسال سریع کد اعتبارسنجی (OTP)**  
  🔹 متد: `SendQuickOTP`  
  🔗 [مشاهده مستند »](./Send/send_quick_otp.md)  
  ارسال خودکار کد احراز هویت برای ثبت‌نام و ورود کاربران.

- **ارسال از طریق الگو (SendWithPattern)**  
  🔹 متد: `SendWithPattern`  
  🔗 [مشاهده مستند »](./Send/send_pattern.md)  
  استفاده از قالب‌های آماده پیامک برای ارسال‌های تکراری.

- **ارسال با خط اختصاصی از طریق الگو**  
  🔹 متد: `SendWithPatternOWN`  
  🔗 [مشاهده مستند »](./Send/send_pattern_own.md)  
  مشابه متد قبل ولی با استفاده از خط اختصاصی شما انجام می‌شود.

- **ارسال نظر به نظیر (Peer To Peer)**  
  🔹 متد: `SendPeerToPeer`  
  🔗 [مشاهده مستند »](./Send/send_peer_to_peer.md)  
  ارسال متن‌های متفاوت برای هر مخاطب در یک درخواست واحد.

- **ارسال از طریق بانک اطلاعاتی**  
  🔹 متد: `SendBank`  
  🔗 [مشاهده مستند »](./Send/send_bank.md)  
  ارسال هدفمند به شماره‌های موجود در دیتابیس تعریف‌شده.

- **ارسال از طریق بانک اطلاعاتی با گارانتی خط تبلیغاتی**  
  🔹 متد: `SendBankWithBackupLine`  
  🔗 [مشاهده مستند »](./Send/send_bank_backupline.md)  
  ارسال هدفمند به شماره‌های موجود در دیتابیس تعریف‌شده.


### ابزارهای جانبی ارسال

- **محاسبه هزینه ارسال الگو**  
  🔹 متد: `CalculatePatternMessagePrice`  
  🔗 [مشاهده مستند »](./Utils/calculate_pattern_message_price.md)  
  محاسبه هزینه بر اساس کاراکتر، گیرنده و نوع الگو.

- **محاسبه هزینه پیامک‌ها**  
  🔹 متد: `CalculateSMSCost`  
  🔗 [مشاهده مستند »](./Utils/calculate_message_price.md)  
  بررسی مجموع هزینه ارسال در بازه زمانی مشخص.

---

## ۳. ارسال هوشمند دوره‌ای

- **ایجاد ارسال هوشمند دوره‌ای**  
  🔹 متد: `CourseCreateSimple`  
  🔗 [مشاهده مستند »](./Send/course_create_simple.md)  
  تعریف ارسال خودکار پیامک در بازه‌های زمانی مشخص.

- **ایجاد ارسال هوشمند دوره‌ای با گارانتی خط تبلیغاتی**  
  🔹 متد: `CourseCreateWithBackupLine`  
  🔗 [مشاهده مستند »](./Send/send_course_backupline.md)  
  مشابه حالت قبل اما با استفاده از خطوط تبلیغاتی.

- **دریافت لیست برنامه‌های ارسال هوشمند**  
  🔹 متد: `CourseList`  
  🔗 [مشاهده مستند »](./Send/course_list.md)  
  مشاهده تمامی ارسال‌های زمان‌بندی‌شده.

- **فعال کردن برنامه ارسال هوشمند دوره‌ای**  
  🔹 متد: `CourseSetActive`  
  🔗 [مشاهده مستند »](./Send/course_set_active.md)  
  فعال‌سازی برنامه‌های غیرفعال‌شده.

---

##  ۴. دریافت پیامک

- **دریافت پیامک‌های دریافتی**  
  🔹 متد: `RecieveMessages`  
  🔗 [مشاهده مستند »](./Receive/recieve_messages.md)  
  دریافت لیست پیامک‌های ورودی از خطوط اختصاصی شما.

- **دریافت پیامک‌های جدید**  
  🔹 متد: `RecieveNewMessages`  
  🔗 [مشاهده مستند »](./Receive/recieve_new_messages.md)  
  فقط پیامک‌هایی که پس از آخرین دریافت ارسال شده‌اند را بازمی‌گرداند.

- **مدیریت مسیرهای انتقال دلیوری**  
  🔹 متدها: `RelayMessageDeliveryCreate`,`RelayMessageDeliveryList`,`RelayMessageDeliverySetActive` 
  
  🔗 [مشاهده مستند »](./Receive/manage_relay_message_delivery.md)  
  افزودن، حذف یا مدیریت مسیرهای ارسال گزارش تحویل.

- **مدیریت مسیرهای انتقال پیام های دریافتی**  
🔹 متدها: `RelayRecieveMessageCreate`,`RelayRecieveMessageList`,`RelayRecieveMessageSetActive` 

  🔗 [مشاهده مستند »](./Receive/manage_relay_recieve_message.md)  
  افزودن، حذف یا مدیریت مسیرهای پیام های دریافتی.

---

##  ۵. گزارش‌ها و آمار

- **دریافت دلیوری پیامک‌ها**  
  🔹 متد: `GetDeliveries`  
  🔗 [مشاهده مستند »](./Reports/delivery_reports.md)  
  بررسی وضعیت تحویل پیام‌ها با استفاده از MessageID.

- **دریافت گزارش کمپین‌ها**  
  🔹 متد: `GetDeliveriesByCampaignID`  
  🔗 [مشاهده مستند »](./Reports/campaign_reports.md)  
  دریافت وضعیت تحویل تمامی پیامک‌های یک کمپین خاص.

- **آمار ارسال‌ها**  
  🔹 متد: `Statistics`  
  🔗 [مشاهده مستند »](./Reports/sms_statistics.md)  
  جمع‌آوری آمار تعداد، وضعیت و هزینه پیامک‌ها.

---

##  ۶. مدیریت مخاطبین 

- **ایجاد مخاطب جدید**  
  🔹 متد: `ContactCreate`  
  🔗 [مشاهده مستند »](./Contacts/create_contact.md)  
  افزودن مخاطب جدید با شماره و اطلاعات شخصی.

- **ویرایش اطلاعات مخاطب**  
  🔹 متد: `ContactEdit`  
  🔗 [مشاهده مستند »](./Contacts/edit_contact.md)  
  به‌روزرسانی اطلاعات، تگ‌ها یا گروه‌های مرتبط.

- **حذف مخاطب**  
  🔹 متد: `ContactDelete`  
  🔗 [مشاهده مستند »](./Contacts/delete_contact.md)  
  حذف کامل مخاطب از دفترچه آدرس.

- **دریافت اطلاعات مخاطب**  
  🔹 متدها: `ContactGet`, `ContactGroupList`,`ContactList`  
  🔗 [مشاهده مستند »](./Contacts/get_contact_info.md)  
  نمایش جزئیات کامل مخاطب شامل نام، شماره و گروه‌ها.

- **مدیریت تگ مخاطب**  
  🔹 متد: `ContactChangeLabel`  
  🔗 [مشاهده مستند »](./Contacts/change_contact_tag.md)  
  افزودن یا حذف برچسب برای فیلترهای سفارشی.

---

##  ۷. مدیریت الگوها

- **ایجاد مخاطب جدید**  
  🔹 متد: `PatternCodeCreate`  
  🔗 [مشاهده مستند »](./Utils/pattern_code_create.md)  
  محاسبه هزینه پیامک‌ها در بازه زمانی خاص یا کمپین.

-  **لیست الگوهای کاربر**  
  🔹  متد: `PatternCodeList` 
  🔗 [مشاهده مستند »](./Utils/pattern_code_list.md) 
  دریافت لیست الگو های کاربر

---

##  ۸. ابزارها و متفرقه

- **محاسبه هزینه کلی پیامک‌ها**  
  🔹 متد: `CalculateMessagePrice`  
  🔗 [مشاهده مستند »](./Utils/calculate_message_price.md)  
  محاسبه هزینه پیامک‌ها در بازه زمانی خاص یا کمپین.

   **مدیریت خطا**  
  🔹  جدول کدهای وضعیت آموت‌ اس‌ام‌اس
  
  🔗 [مشاهده مستند »](./Utils/status_codes.md)  
---

## 🔗 لینک‌های مفید
- [ مستندات کامل API](https://portal.amootsms.com/documentation)  
- [ پرتال آموت SMS](https://portal.amootsms.com)
- [ ارسال تیکت](https://hub.amootsoft.com/dashboard/ticket/create)
- [ تاریخچه تغییرات](Changelog.md)
- [ سوالات متداول ](FAQ.md)

---

## 📞 تماس با ما
- تلفن: ۰۵۱-۳۱۷۷۷۲۰۰  
- ایمیل: info@amootsoft.com

---

  
<div dir="ltr" align="center" style="display: flex; justify-content: center; gap: 20px;">
  <a href="https://www.alavan.co.ir" target="_blank">
    <img src="/Images/alavan.svg" alt="Alavan" width="40"/>
  </a>
  <a href="https://www.owj.io" target="_blank">
    <img src="/Images/owjCloud.svg" alt="OwjCloud" height="45"/>
  </a>
  <a href="https://avanak.ir" target="_blank">
    <img src="/Images/avanak.svg" alt="Avanak" width="40"/>
  </a>
</div>

</div>