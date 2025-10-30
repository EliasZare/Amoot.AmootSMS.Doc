# متد وب سرویس ارسال سریع کد اعتبارسنجی (SendQuickOTP)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

### آدرس وب سرویس

> https://portal.amootsms.com/rest/SendQuickOTP

### توضیح متد

این متد به شما امکان می‌دهد پیامک حاوی رمز یکبار مصرف (OTP) را به صورت فوری به شماره موبایل مورد نظر ارسال کنید. برای استفاده از این متد، ابتدا باید یک الگو با متغیر `code` در پنل کاربری ایجاد کنید و پس از تایید توسط اپراتور، امکان ارسال فعال می‌شود.

> نکته: الگوی پیامک خود را در پنل کاربری پیامک --> امکانات --> پیامک‌های با الگو ثبت و گزینه otp را فعال نمایید.

### پارامترهای ورودی

| نام          | نوع    | توضیحات                                                                  |
| ------------ | ------ | ------------------------------------------------------------------------ |
| Token        | String | الزامی، توکن ثبت شده در سامانه پیامک آموت                                |
| Mobile       | String | الزامی، شماره موبایل دریافت‌کننده پیامک                                  |
| CodeLength   | Short  | الزامی، طول کد (حداقل 4 رقم و حداکثر 8 رقم)                              |
| OptionalCode | String | الزامی، رمز یکبار مصرف سفارشی. اگر خالی باشد، رمز توسط سرور تولید می‌شود |

### مقدار خروجی

| نام         | نوع    | توضیحات                                 |
| ----------- | ------ | --------------------------------------- |
| ReturnValue | Object | شامل آرایه‌ای از یک کلاس با مقادیر زیر: |

```
{
  Status = وضعیت,
  MessageID = کد پیامک (کد شماره کمپین),
  Mobile = شماره موبایل,
  Code = رمز یکبار مصرف
}
```

### نمونه کد

#### C#

```csharp
string Token = "MyToken";
string Mobile = "09120000000";
short CodeLength = 6;
string OptionalCode = ""; // اگر خالی باشد کد سمت سرور تولید می‌شود

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "CodeLength", CodeLength.ToString() },
        { "OptionalCode", OptionalCode }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendQuickOTP", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
}
```

#### PHP

```php
$token = 'MyToken';
$mobile = '09120000000';
$codeLength = 6;
$optionalCode = '';

$ch = curl_init('https://portal.amootsms.com/rest/SendQuickOTP');
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Authorization: ' . $token));
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(array(
    'Mobile' => $mobile,
    'CodeLength' => $codeLength,
    'OptionalCode' => $optionalCode
)));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
```

#### NodeJS

```javascript
const axios = require('axios');

const token = 'MyToken';
const mobile = '09120000000';
const codeLength = 6;
const optionalCode = '';

axios.post('https://portal.amootsms.com/rest/SendQuickOTP', {
  Mobile: mobile,
  CodeLength: codeLength,
  OptionalCode: optionalCode
}, {
  headers: { Authorization: token }
})
.then(res => console.log(res.data))
.catch(err => console.error(err));
```

#### CURL

```bash
curl -X POST https://portal.amootsms.com/rest/SendQuickOTP \
  -H 'Authorization: MyToken' \
  -d 'Mobile=09120000000' \
  -d 'CodeLength=6' \
  -d 'OptionalCode='
```
