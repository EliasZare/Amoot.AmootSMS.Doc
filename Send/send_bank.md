<div dir="rtl">

# مستندات وب‌سرویس بانک اطلاعاتی - ارسال از بانک اطلاعاتی (SendBank)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/SendBank](https://portal.amootsms.com/rest/SendBank)

## توضیح کلی

این متد به شما امکان می‌دهد پیامک را با استفاده از **بانک‌های اطلاعاتی ذخیره‌شده در سامانه** ارسال کنید. می‌توانید ترتیب برداشت شماره‌ها، ردیف شروع و تعداد گیرندگان را مشخص کنید. اگر `SendDateTime` تعیین نشود، ارسال بلافاصله انجام می‌شود.

## پارامترهای ورودی

| نام            | نوع    | الزامی | توضیحات                                                                                                           |
| -------------- | ------ | :----: | ----------------------------------------------------------------------------------------------------------------- |
| Token          | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود                                                       |
| SendDateTime   | String |   بله  | تاریخ و زمان ارسال پیامک (فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span>)                                        |
| SMSMessageText | String |   بله  | متن پیامک                                                                                                         |
| LineNumber     | String |   بله  | شماره خط ارسال یا مقدار `public` برای خط عمومی                                                                    |
| BankID         | Long   |   بله  | شناسه بانک اطلاعاتی انتخاب‌شده                                                                                    |
| Random         | Bool   |   بله  | تعیین تصادفی بودن انتخاب شماره‌ها؛ اگر `true` باشد شماره‌ها به‌صورت تصادفی انتخاب می‌شوند، در غیر این صورت ترتیبی |
| FromRow        | Int    |   بله  | شروع ارسال از ردیف مشخص‌شده (0-مبنـا)                                                                             |
| Count          | Int    |   بله  | تعداد گیرندگان موردنظر که از بانک انتخاب می‌شوند                                                                  |

> فیلد `Random` نوع انتخاب شماره‌ها را تعیین می‌کند:
>
> * `true` → انتخاب تصادفی از بانک اطلاعاتی
> * `false` → انتخاب ترتیبی از بانک اطلاعاتی

## مقدار خروجی

| نام           | نوع    | الزامی | توضیحات                                               |
| ------------- | ------ | :----: | ----------------------------------------------------- |
| MessageText   | String |   خیر  | متن نهایی پیامک                                       |
| SMSPagesCount | Int    |   خیر  | تعداد صفحات پیامک                                     |
| CampaignID    | Int    |   خیر  | شناسه کمپین ایجادشده                                  |
| Price         | Int    |   خیر  | هزینه کل (ریال)                                       |
| Status        | String |   خیر  | وضعیت عملیات (Success یا کد خطا)                      |
| Data          | Array  |   خیر  | نتایج ارسال برای شماره‌های منتخب (ممکن است خالی باشد) |


## نمونه کدها

### C#

```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
long BankID = 123;
bool Random = true; // انتخاب تصادفی شماره‌ها
int FromRow = 0;
int Count = 100;

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString("yyyy-MM-dd HH:mm:ss") },
        { "SMSMessageText", SMSMessageText },
        { "LineNumber", LineNumber },
        { "BankID", BankID.ToString() },
        { "Random", Random.ToString().ToLower() },
        { "FromRow", FromRow.ToString() },
        { "Count", Count.ToString() }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendBank", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests

token = "MyToken"
url = "https://portal.amootsms.com/rest/SendBank"
payload = {
    "SendDateTime": "2025-10-21 16:05:00",
    "SMSMessageText": "پیامک تستی من",
    "LineNumber": "public",
    "BankID": 123,
    "Random": True,  # True برای انتخاب تصادفی، False برای ترتیبی
    "FromRow": 0,
    "Count": 100
}
headers = {"Authorization": token}
res = requests.post(url, data=payload, headers=headers)
print(res.text)
```

### PHP

```php
$token = "MyToken";
$data = [
    "SendDateTime" => date("Y-m-d H:i:s"),
    "SMSMessageText" => "پیامک تستی من",
    "LineNumber" => "public",
    "BankID" => 123,
    "Random" => true, // true برای انتخاب تصادفی، false برای ترتیبی
    "FromRow" => 0,
    "Count" => 100
];

$ch = curl_init("https://portal.amootsms.com/rest/SendBank");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```js
const axios = require("axios");

const token = "MyToken";
axios.post("https://portal.amootsms.com/rest/SendBank", {
  SendDateTime: "2025-10-21 16:05:00",
  SMSMessageText: "پیامک تستی من",
  LineNumber: "public",
  BankID: 123,
  Random: true, // true برای انتخاب تصادفی، false برای ترتیبی
  FromRow: 0,
  Count: 100
}, { headers: { Authorization: token } })
.then(r => console.log(r.data))
.catch(e => console.error(e.response?.data || e.message));
```

### cURL

```bash
curl -X POST https://portal.amootsms.com/rest/SendBank \
  -H "Authorization: MyToken" \
  -d "SendDateTime=2025-10-21 16:05:00" \
  -d "SMSMessageText=پیامک تستی من" \
  -d "LineNumber=public" \
  -d "BankID=123" \
  -d "Random=true" \
  -d "FromRow=0" \
  -d "Count=100"
```
### نمونه خروجی موفق

```json
{
  "MessageText": "نمونه پیام تستی",
  "SMSPagesCount": 1,
  "CampaignID": 1234545,
  "Price": 125000,
  "Status": "Success",
  "Data": [
    {
      "Number": 9120000000,
      "MessageID": 600010001,
      "Status": "Success"
    },
    {
      "Number": 9150000000,
      "MessageID": 600010002,
      "Status": "Success"
    }
  ]
}
```

### نمونه خروجی ناموفق

```json
{
  "MessageText": null,
  "SMSPagesCount": 1,
  "CampaignID": 0,
  "Price": 0,
  "Status": "BulkID_Invalid",
  "Data": []
}
```


## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این الگو، سرویس حتی در خطا نیز **HTTP 200 (OK)** برمی‌گرداند و مقدار فیلد <span dir="ltr">Status</span> نوع خطا را مشخص می‌کند.


> نکته: خطای `BulkID_Invalid` معمولاً به معنی **نامعتبر بودن شناسه بانک** (BankID) یا عدم دسترسی به آن است.

## تغییرات (Changelog)

| تاریخ      | نسخه  | توضیح                                             |
| ---------- | ----- | ------------------------------------------------- |
| 1404/07/29 | 1.0.2 | افزودن پارامتر اجباری Random از نوع بولی در ورودی |

</div>
