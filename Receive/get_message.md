<div dir="rtl">

# مستندات وب‌سرویس گزارش‌ها - دریافت پیام (GetMessage)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> <span dir="ltr">[https://portal.amootsms.com/rest/GetMessage](https://portal.amootsms.com/rest/GetMessage)</span>

## توضیح کلی

این متد جزئیات یک پیامک را بر اساس شناسهٔ پیام (<span dir="ltr">MessageID</span>) برمی‌گرداند. تنها پیام‌های متعلق به کاربرِ احراز‌شده قابل مشاهده‌اند. اگر پیام هنوز ارسال نشده باشد، مقدار <span dir="ltr">SentDateTime</span> برابر با مقدار پیش‌فرض .NET یعنی <span dir="ltr">0001-01-01T00:00:00</span> خواهد بود.

## پارامترهای ورودی

| نام       | نوع    | الزامی | توضیحات                                                                                                       |
| --------- | ------ | :----: | ------------------------------------------------------------------------------------------------------------- |
| Token     | String |   بله  | توکن احراز هویت؛ در هدر Authorization ارسال شود: <span dir="ltr">Authorization: {YourToken}</span>            |
| MessageID | Long   |   بله  | شناسهٔ عددی پیام؛ باید بزرگ‌تر از صفر باشد. تنها در صورتی داده بازگردانده می‌شود که پیام متعلق به کاربر باشد. |

## مقدار خروجی

| نام    | نوع          | الزامی | توضیحات                                                                                                                                                                                                    |
| ------ | ------------ | :----: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status | String       |   خیر  | در موفقیت مقدار <span dir="ltr">Success</span>؛ در غیر این‌ صورت یکی از کدهای خطا مانند <span dir="ltr">MessageID_Invalid</span>، <span dir="ltr">Token_Invalid</span>، <span dir="ltr">ServerError</span> |
| Data   | Object/Array |   خیر  | در موفقیت: شیء اطلاعات پیام؛ در ناموفق: <span dir="ltr">null</span>                                                                                                                                        |

### ساختار هر آیتم در <span dir="ltr">Data</span>

| نام فیلد       | نوع      | توضیحات                                                                                                    |
| -------------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| MessageID      | Long     | شناسهٔ یکتای پیام                                                                                          |
| Mobile         | String   | شمارهٔ مقصد (فرمت‌شده بر اساس نوع ارسال)                                                                   |
| LineNumber     | String   | شمارهٔ خط ارسال‌کننده                                                                                      |
| SMSMessageText | String   | متن پیام                                                                                                   |
| RegDateTime    | DateTime | زمان ثبت/ایجاد پیام/کمپین                                                                                  |
| SentDateTime   | DateTime | زمان ارسال؛ اگر ارسال نشده باشد مقدار پیش‌فرض <span dir="ltr">0001-01-01T00:00:00</span> بازگردانده می‌شود |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": {
    "MessageID": 600000001,
    "Mobile": "9120000000",
    "LineNumber": "500030005",
    "SMSMessageText": "سلام کاربر عزیز، کد تخفیف شما: B123\nلغو11",
    "RegDateTime": "2025-10-25T15:16:59.59",
    "SentDateTime": "0001-01-01T00:00:00"
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "MessageID_Invalid",
  "Data": null
}
```

</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این الگو، سرویس حتی در صورت خطا نیز با **HTTP 200 (OK)** پاسخ می‌دهد، اما نتیجه در بدنهٔ پاسخ مشخص می‌شود:

* اگر مقدار <span dir="ltr">Status = "Success"</span> باشد، عملیات موفق است و <span dir="ltr">Data</span> شامل دادهٔ معتبر خواهد بود.
* در غیر این صورت، <span dir="ltr">Status</span> یکی از کدهای خطاست و <span dir="ltr">Data</span> معمولاً تهی/مقدار پیش‌فرض است.

> نکته: همواره فیلد <span dir="ltr">Status</span> را بررسی کنید؛ **HTTP 200** به‌تنهایی نشانهٔ موفقیت نیست.

## نمونه کدها

<div dir=ltr>

### C#

```csharp
using System.Net.Http;
using System.Threading.Tasks;
using System.Collections.Generic;

var url = "https://portal.amootsms.com/rest/GetMessage";
using (var http = new HttpClient())
{
    http.DefaultRequestHeaders.Add("Authorization", "{YourToken}");
    var form = new FormUrlEncodedContent(new []
    {
        new KeyValuePair<string,string>("MessageID","600000001")
    });

    var res = await http.PostAsync(url, form);
    var body = await res.Content.ReadAsStringAsync();
    // مقدار Status را بررسی کنید
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/GetMessage"
headers = {"Authorization": "{YourToken}"}
data = {"MessageID": "600000001"}

r = requests.post(url, headers=headers, data=data)
print(r.text)  # Status را بررسی کنید
```

### PHP

```php
<?php
$ch = curl_init("https://portal.amootsms.com/rest/GetMessage");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => http_build_query(["MessageID" => "600000001"]),
  CURLOPT_HTTPHEADER => ["Authorization: {YourToken}"],
  CURLOPT_RETURNTRANSFER => true
]);
$resp = curl_exec($ch);
curl_close($ch);
echo $resp; // Status را بررسی کنید
```

### Node.js

```js
const axios = require("axios");

(async () => {
  const url = "https://portal.amootsms.com/rest/GetMessage";
  const res = await axios.post(
    url,
    { MessageID: 600000001 },
    { headers: { Authorization: "{YourToken}" } }
  );
  console.log(res.data); // Status را بررسی کنید
})();
```

### Java

```java
import java.net.*;
import java.io.*;
import java.nio.charset.StandardCharsets;

String url = "https://portal.amootsms.com/rest/GetMessage";
String data = "MessageID=600000001";

HttpURLConnection conn = (HttpURLConnection) new URL(url).openConnection();
conn.setRequestMethod("POST");
conn.setDoOutput(true);
conn.setRequestProperty("Authorization", "{YourToken}");
conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded; charset=UTF-8");

try (OutputStream os = conn.getOutputStream()) {
    os.write(data.getBytes(StandardCharsets.UTF_8));
}
try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
    String line; StringBuilder sb = new StringBuilder();
    while ((line = br.readLine()) != null) sb.append(line);
    System.out.println(sb.toString()); // Status را بررسی کنید
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/GetMessage" \
  -H "Authorization: {YourToken}" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "MessageID=600000001"
```
</div>

</div>
