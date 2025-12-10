<div dir="rtl">

# مستندات وب‌سرویس گزارش‌ها - دریافت دلیوری پیام (GetDelivery)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> <span dir="ltr">[https://portal.amootsms.com/rest/GetDelivery](https://portal.amootsms.com/rest/GetDelivery)</span>

## توضیح کلی

این متد وضعیت دلیوری آخرین‌بار ثبت‌شده برای یک پیام مشخص را بر اساس <span dir="ltr">MessageID</span> برمی‌گرداند. اگر زمان ارسال ثبت نشده باشد، مقدار زمان ارسال با «زمان ایجاد کمپین/پیام» پر می‌شود.

## پارامترهای ورودی

| نام       | نوع    | الزامی | توضیحات                                                                                                  |
| --------- | ------ | :----: | -------------------------------------------------------------------------------------------------------- |
| Token     | String |   بله  | توکن احراز هویت؛ در هدر Authorization ارسال شود: <span dir="ltr">Authorization: {YourToken}</span>       |
| MessageID | Long   |   بله  | شناسهٔ عددی پیام؛ باید بزرگ‌تر از صفر باشد. فقط برای پیام‌های متعلق به کاربرِ احرازشده پاسخ داده می‌شود. |

## مقدار خروجی

| نام    | نوع          | الزامی | توضیحات                                                                                                                                                                                          |
| ------ | ------------ | :----: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Status | String       |   خیر  | در موفقیت مقدار <span dir="ltr">Success</span>؛ در غیر این‌صورت یکی از مقادیر خطا مانند <span dir="ltr">Failed</span>، <span dir="ltr">Token_Invalid</span>، <span dir="ltr">ServerError</span>. |
| Data   | Object/Array |   خیر  | در موفقیت: شیء حاوی اطلاعات دلیوری؛ در ناموفق: <span dir="ltr">null</span>.                                                                                                                      |

### ساختار هر آیتم در <span dir="ltr">Data</span>

| نام فیلد     | نوع      | توضیحات                                                                                     |
| ------------ | -------- | ------------------------------------------------------------------------------------------- |
| MessageID    | Long     | شناسهٔ پیام درخواست‌شده                                                                     |
| DeliveryType | Int      | کُد وضعیت دلیوری نهایی (مقدار عددی داخلی سامانه)                                            |
| Cost         | Long     | هزینهٔ پیام (در این پیاده‌سازی مقدار <span dir="ltr">0</span> برگردانده می‌شود)             |
| RegDateTime  | DateTime | زمان ثبت/ایجاد پیام (زمان ساخت کمپین)                                                       |
| SendDateTime | DateTime | زمان ارسال؛ اگر ثبت نشده باشد، برابر با <span dir="ltr">RegDateTime</span> برگردانده می‌شود |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": {
    "MessageID": 123456789,
    "DeliveryType": 2,
    "Cost": 0,
    "RegDateTime": "2025-10-25T15:16:59.590",
    "SendDateTime": "2025-10-25T15:16:59.590"
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Failed",
  "Data": null
}
```

</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا (الگوی نوع A)

در این الگو، سرویس حتی در صورت خطا نیز با **HTTP 200 (OK)** پاسخ می‌دهد، اما نتیجه در بدنهٔ پاسخ مشخص می‌شود:

* اگر مقدار <span dir="ltr">Status = "Success"</span> باشد، عملیات موفق است و <span dir="ltr">Data</span> شامل دادهٔ معتبر خواهد بود.
* در غیر این صورت، <span dir="ltr">Status</span> یکی از کدهای خطاست و <span dir="ltr">Data</span> معمولاً تهی/مقدار پیش‌فرض است.

> نکته: همواره فیلد <span dir="ltr">Status</span> را بررسی کنید؛ **HTTP 200** به‌تنهایی نشانهٔ موفقیت نیست.

## نمونه کدها

### C#

```csharp
using System.Net.Http;
using System.Threading.Tasks;
using System.Collections.Generic;

var url = "https://portal.amootsms.com/rest/GetDelivery";
using (var http = new HttpClient())
{
    http.DefaultRequestHeaders.Add("Authorization", "{YourToken}");
    var form = new FormUrlEncodedContent(new []
    {
        new KeyValuePair<string,string>("MessageID","600477358")
    });

    var res = await http.PostAsync(url, form);
    var body = await res.Content.ReadAsStringAsync();
    // Status را بررسی کنید
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/GetDelivery"
headers = {"Authorization": "{YourToken}"}
data = {"MessageID": "600477358"}

r = requests.post(url, headers=headers, data=data)
print(r.text)  # Status را بررسی کنید
```

### PHP

```php
<?php
$ch = curl_init("https://portal.amootsms.com/rest/GetDelivery");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => http_build_query(["MessageID" => "600477358"]),
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
  const url = "https://portal.amootsms.com/rest/GetDelivery";
  const res = await axios.post(
    url,
    { MessageID: 600477358 },
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

String url = "https://portal.amootsms.com/rest/GetDelivery";
String data = "MessageID=600477358";

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
curl -X POST "https://portal.amootsms.com/rest/GetDelivery" \
  -H "Authorization: {YourToken}" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "MessageID=123456789"
```


</div>
