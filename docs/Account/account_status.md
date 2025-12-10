<div dir="rtl" >

#  مستندات وب سرویس حساب کاربری - وضعیت حساب کاربری (AccountStatus)

**تاریخ بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest](https://portal.amootsms.com/rest)

---

##  توضیح کلی

از طریق این متد می‌توانید جزئیات حساب کاربری خود را مشاهده کنید. این اطلاعات شامل:

* اعتبار باقی‌مانده (به ریال)
* تعرفه پیامک فارسی و انگلیسی (پایه، خدماتی، تبلیغاتی)
* تعرفه تماس صوتی آوانک (بر حسب ثانیه)
* لیست خطوط پیامکی فعال

می‌باشد.

---

##  آدرس وب‌سرویس

<div dir="ltr">

```
POST https://portal.amootsms.com/rest/AccountStatus
```

</div>

---

##  پارامترهای ورودی

<div dir="rtl">

| نام   | نوع    | الزامی | توضیحات                           |
| ----- | ------ | :----: | --------------------------------- |
| Token | string |    بله   | توکن ثبت‌شده در سامانه پیامک آموت |

</div>

> این متد پارامتر دیگری نیاز ندارد و تنها با ارسال هدر Authorization فراخوانی می‌شود.

---

##  خروجی (Response)

<div dir="rtl">

| نام                     | نوع    | توضیحات                                |
| ----------------------- | ------ | -------------------------------------- |
| Status                  | string | وضعیت پاسخ (مثلاً `Success` یا کد خطا) |
| AccountName             | string | نام حساب کاربری                        |
| RemaindCredit           | int    | باقیمانده اعتبار به ریال               |
| BaseSMS_PersianPrice    | float  | تعرفه پایه پیامک فارسی                  |
| BaseSMS_EnglishPrice    | float  | تعرفه پایه پیامک انگلیسی                |
| ServiceSMS_PersianPrice | float  | تعرفه پیامک خدماتی فارسی               |
| ServiceSMS_EnglishPrice | float  | تعرفه پیامک خدماتی انگلیسی             |
| AdsSMS_PersianPrice     | float  | تعرفه پیامک تبلیغاتی فارسی             |
| AdsSMS_EnglishPrice     | float  | تعرفه پیامک تبلیغاتی انگلیسی           |
| Avanak_SecondPrice      | float  | تعرفه هر ثانیه پیام صوتی آوانک         |
| ListLineNumbers         | array  | لیست خطوط پیامکی فعال                  |

</div>

---

##  نمونه کدها

<div dir="ltr">

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/AccountStatus" \
     -H "Authorization: MyToken" \
```

###  C#

```csharp
string token = "MyToken";
using var client = new System.Net.WebClient();
client.Headers.Add(System.Net.HttpRequestHeader.Authorization, token);

var response = client.UploadValues("https://portal.amootsms.com/rest/AccountStatus", new System.Collections.Specialized.NameValueCollection());
string json = System.Text.Encoding.UTF8.GetString(response);
Console.WriteLine(json); // خروجی JSON
```

###  PHP

```php
$token = "MyToken";
$ch = curl_init("https://portal.amootsms.com/rest/AccountStatus");
curl_setopt_array($ch, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => ["Authorization: $token"],
    CURLOPT_POST => true,
]);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // خروجی JSON
```

###  Python

```python
import requests

url = "https://portal.amootsms.com/rest/AccountStatus"
headers = {"Authorization": "MyToken"}

response = requests.post(url, headers=headers)
print(response.text)  # خروجی JSON
```

###  Node.js

```js
const axios = require('axios');

const url = 'https://portal.amootsms.com/rest/AccountStatus';
const token = 'MyToken';

axios.post(url, {}, { headers: { 'Authorization': token } })
  .then(res => console.log(res.data))
  .catch(err => console.error(err));
```

###  Java

```java
import java.io.*;
import java.net.*;

public class AccountStatus {
    public static void main(String[] args) throws Exception {
        String token = "MyToken";
        URL url = new URL("https://portal.amootsms.com/rest/AccountStatus");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Authorization", token);
        conn.setDoOutput(true);

        conn.getOutputStream().write(new byte[0]);

        try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
            StringBuilder response = new StringBuilder();
            String line;
            while ((line = reader.readLine()) != null) response.append(line);
            System.out.println(response);
        }
    }
}
```

</div>

---

##  نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "AccountName": "AccountName",
  "RemaindCredit": 25000,
  "BaseSMS_PersianPrice": 1000,
  "BaseSMS_EnglishPrice": 5000,
  "ServiceSMS_PersianPrice": 2050,
  "ServiceSMS_EnglishPrice": 6670,
  "AdsSMS_PersianPrice": 3870,
  "AdsSMS_EnglishPrice": 11560,
  "Avanak_SecondPrice": 23,
  "ListLineNumbers": [
    "98",
    "Public",
    "Service"
  ]
}
```

</div>

---

## ⚠️ خروجی ناموفق (Error Response)

در صورت بروز خطا، فیلد **Status** شامل کد خطا خواهد بود و سایر مقادیر صفر یا خالی خواهند بود.

<div dir="ltr">

```json
{
  "Status": "User_NotExists",
  "AccountName": null,
  "RemaindCredit": 0,
  "BaseSMS_PersianPrice": 0,
  "BaseSMS_EnglishPrice": 0,
  "ServiceSMS_PersianPrice": 0,
  "ServiceSMS_EnglishPrice": 0,
  "AdsSMS_PersianPrice": 0,
  "AdsSMS_EnglishPrice": 0,
  "Avanak_SecondPrice": 0,
  "ListLineNumbers": []
}
```

</div>

> **نکته:** کد خطای `User_NotExists` به معنی نامعتبر بودن توکن یا عدم وجود کاربر است.

---

##  نکات تکمیلی

* تنها از روش <span dir="ltr">POST</span> پشتیبانی می‌شود.
* مقدار توکن باید در هدر **Authorization** ارسال شود.
* هیچ داده‌ای در بدنه درخواست لازم نیست.

</div>
