<div dir="rtl">

# مستندات وب‌سرویس بانک اطلاعاتی - ارسال با خط پشتیبان (SendBankWithBackupLine)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/SendBankWithBackupLine](https://portal.amootsms.com/rest/SendBankWithBackupLine)

## توضیح کلی

با این متد می‌توانید پیامک‌های خود را به مخاطبان یک **بانک اطلاعاتی** ارسال کنید. در صورتی‌که ارسال از خط اصلی ممکن نباشد (مثلاً شماره در بلک‌لیست است)، پیامک به‌طور خودکار از **خط پشتیبان تبلیغاتی** (مانند `98`) ارسال می‌شود.

## پارامترهای ورودی

| نام              | نوع    | الزامی | توضیحات                                                                |
| ---------------- | ------ | :----: | ---------------------------------------------------------------------- |
| Token            | String |   بله  | توکن احراز هویت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود |
| SendDateTime     | String |   بله  | تاریخ و زمان ارسال به فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span>  |
| SMSMessageText   | String |   بله  | متن پیامک                                                              |
| LineNumber       | String |   بله  | شماره خط ارسال‌کننده (اصلی)                                            |
| BackupLineNumber | String |   بله  | شماره خط پشتیبان (تبلیغاتی)؛ مانند `98`                                |
| BankID           | Long   |   بله  | شناسه بانک اطلاعاتی انتخاب‌شده                                         |
| Random           | Bool   |   بله  | **اجباری.** شیوه انتخاب مخاطبان از بانک: `true`=تصادفی، `false`=ترتیبی |
| FromRow          | Int    |   بله  | ردیف شروع انتخاب از بانک (۰-مبنا)                                      |
| Count            | Int    |   بله  | تعداد گیرندگان مدنظر                                                   |

> توجه: از این نسخه به بعد، پارامتر `OrderType` استفاده نمی‌شود و به‌جای آن **`Random`** (بولی) الزامی است.

## مقدار خروجی

| نام           | نوع    | الزامی | توضیحات                                      |
| ------------- | ------ | :----: | -------------------------------------------- |
| MessageText   | String |   خیر  | متن نهایی پیامک                              |
| SMSPagesCount | Int    |   خیر  | تعداد صفحات پیامک                            |
| CampaignID    | Int    |   خیر  | شناسه کمپین ایجاد شده                        |
| Price         | Int    |   خیر  | هزینه کل (ریال)                              |
| Status        | String |   خیر  | وضعیت عملیات (Success یا کد خطا)             |
| Data          | Array  |   خیر  | نتایج ریز برای هر شماره (ممکن است خالی باشد) |

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تبلیغاتی تست";
string LineNumber = "public";
string BackupLineNumber = "98";
long BankID = 123;
bool Random = true; // true = تصادفی، false = ترتیبی
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
        { "BackupLineNumber", BackupLineNumber },
        { "BankID", BankID.ToString() },
        { "Random", Random.ToString().ToLower() },
        { "FromRow", FromRow.ToString() },
        { "Count", Count.ToString() }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendBankWithBackupLine", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests

token = "MyToken"
url = "https://portal.amootsms.com/rest/SendBankWithBackupLine"
payload = {
    "SendDateTime": "2025-10-21 16:20:00",
    "SMSMessageText": "پیامک تبلیغاتی تست",
    "LineNumber": "public",
    "BackupLineNumber": "98",
    "BankID": 123,
    "Random": True,  # True=تصادفی، False=ترتیبی
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
  "SMSMessageText" => "پیامک تبلیغاتی تست",
  "LineNumber" => "public",
  "BackupLineNumber" => "98",
  "BankID" => 123,
  "Random" => true, // true=تصادفی، false=ترتیبی
  "FromRow" => 0,
  "Count" => 100
];

$ch = curl_init("https://portal.amootsms.com/rest/SendBankWithBackupLine");
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
axios.post("https://portal.amootsms.com/rest/SendBankWithBackupLine", {
  SendDateTime: "2025-10-21 16:20:00",
  SMSMessageText: "پیامک تبلیغاتی تست",
  LineNumber: "public",
  BackupLineNumber: "98",
  BankID: 123,
  Random: true,
  FromRow: 0,
  Count: 100
}, { headers: { Authorization: token } })
.then(r => console.log(r.data))
.catch(e => console.error(e.response?.data || e.message));
```

### cURL

```bash
curl -X POST https://portal.amootsms.com/rest/SendBankWithBackupLine \
  -H "Authorization: MyToken" \
  -d "SendDateTime=2025-10-21 16:20:00" \
  -d "SMSMessageText=پیامک تبلیغاتی تست" \
  -d "LineNumber=public" \
  -d "BackupLineNumber=98" \
  -d "BankID=123" \
  -d "Random=true" \
  -d "FromRow=0" \
  -d "Count=100"
```


### نمونه خروجی موفق (نمونه تستی)

```json
{
  "MessageText": "پیامک تبلیغاتی تست",
  "SMSPagesCount": 1,
  "CampaignID": 77441001,
  "Price": 84000,
  "Status": "Success",
  "Data": [
    { "Mobile": "09120000000", "MessageID": 700010001, "Status": "Success" },
    { "Mobile": "09121112222", "MessageID": 700010002, "Status": "Success", "UsedLine": "98" }
  ]
}
```

### نمونه خروجی ناموفق (نمونه مطابق اسکرین‌شات)

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

سرویس حتی در خطا نیز **HTTP 200 (OK)** را برمی‌گرداند و فیلد <span dir="ltr">Status</span> نوع خطا را مشخص می‌کند.

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

## تغییرات (Changelog)

| تاریخ      | نسخه  | توضیح                                    |
| ---------- | ----- | ---------------------------------------- |
| 1404/07/29 | 1.0.0 | ایجاد سند و افزودن پارامتر اجباری Random |

</div>
