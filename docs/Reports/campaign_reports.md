<div dir="rtl">

# مستندات وب‌سرویس گزارش دلیوری - دریافت بر اساس شماره کمپین (GetDeliveriesByCampaignID)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/GetDeliveriesByCampaignID](https://portal.amootsms.com/rest/GetDeliveriesByCampaignID)

## توضیح کلی

با این متد می‌توانید وضعیت دلیوری (گزارش تحویل) **تمام پیامک‌های یک کمپین** را با ارسال یک شناسهٔ کمپین (کد بالک) بازیابی کنید.

## پارامترهای ورودی

| نام        | نوع    | الزامی | توضیحات                                               |
| ---------- | ------ | :----: | ----------------------------------------------------- |
| Token      | String |   بله  | در هدر <span dir="ltr">Authorization</span> ارسال شود |
| CampaignID | Long   |   بله  | شناسه کمپین (کد بالک)                                 |

> **Content-Type:** <span dir="ltr">application/x-www-form-urlencoded</span>

## مقدار خروجی (واقعی)

| نام    | نوع    | توضیحات                                                                   |
| ------ | ------ | ------------------------------------------------------------------------- |
| Status | String | «Success» در صورت موفقیت؛ در خطا مقادیری مانند «BulkID_Invalid» برمی‌گردد |
| Data   | Array  | آرایه‌ای از ردیف‌های دلیوریِ پیامک‌های کمپین (در خطا معمولاً آرایهٔ خالی) |

### فیلدهای هر آیتم از `Data`

| نام          | نوع    | توضیحات                         |
| ------------ | ------ | ------------------------------- |
| Number       | String | شمارهٔ گیرنده                   |
| MessageID    | Long   | شناسه پیام                      |
| DeliveryType | Int    | نوع دلیوری                      |
| Cost         | Int    | هزینه                           |
| RegDateTime  | String | زمان ثبت                        |
| SendDateTime | String | زمان ارسال                      |
| Status       | String | وضعیت همان پیام (مثل «Success») |

### نمونه خروجی موفق (مطابق اسکرین‌شات)

```json
{
  "Status": "Success",
  "Data": [
    {
      "Number": "9150000000",
      "MessageID": 12345678,
      "DeliveryType": 35,
      "Cost": 0,
      "RegDateTime": "2025-10-21T12:18:10.96",
      "SendDateTime": "2025-10-21T12:24:47.26",
      "Status": "Success"
    },
    {
      "Number": "9120000000",
      "MessageID": 12345677,
      "DeliveryType": 1,
      "Cost": 0,
      "RegDateTime": "2025-10-21T12:18:10.96",
      "SendDateTime": "2025-10-21T12:24:47.26",
      "Status": "Success"
    }
  ]
}
```

### نمونه خروجی ناموفق (شناسه کمپین نامعتبر)

```json
{
  "Status": "BulkID_Invalid",
  "Data": []
}
```

## نمونه کدها

### C#

```csharp
using System;
using System.Net;
using System.Text;
using System.Collections.Specialized;

string token = "MyToken";
long campaignId = 76581373;
using (var wc = new WebClient())
{
    wc.Headers.Add(HttpRequestHeader.Authorization, token);
    wc.Headers.Add(HttpRequestHeader.ContentType, "application/x-www-form-urlencoded");
    var form = new NameValueCollection { { "CampaignID", campaignId.ToString() } };
    var resp = wc.UploadValues("https://portal.amootsms.com/rest/GetDeliveriesByCampaignID", form);
    Console.WriteLine(Encoding.UTF8.GetString(resp));
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/GetDeliveriesByCampaignID"
headers = {"Authorization": "MyToken"}
form = {"CampaignID": 76581373}
res = requests.post(url, data=form, headers=headers)
print(res.json())
```

### PHP

```php
<?php
$token = 'MyToken';
$campaignId = 76581373;
$ch = curl_init('https://portal.amootsms.com/rest/GetDeliveriesByCampaignID');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(['CampaignID' => $campaignId]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
echo curl_exec($ch);
curl_close($ch);
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/GetDeliveriesByCampaignID',
  new URLSearchParams({ CampaignID: '76581373' }).toString(),
  { headers: { Authorization: 'MyToken', 'Content-Type': 'application/x-www-form-urlencoded' } }
).then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

### Java

```java
import java.io.*; import java.net.*;
public class GetDeliveriesByCampaignIDExample {
  public static void main(String[] args) throws Exception {
    URL url = new URL("https://portal.amootsms.com/rest/GetDeliveriesByCampaignID");
    HttpURLConnection c = (HttpURLConnection) url.openConnection();
    c.setRequestMethod("POST");
    c.setRequestProperty("Authorization", "MyToken");
    c.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    c.setDoOutput(true);
    String body = "CampaignID=76581373";
    try(OutputStream os = c.getOutputStream()){ os.write(body.getBytes()); }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(c.getInputStream(), "utf-8"))){
      String line, out = ""; while((line = br.readLine()) != null) out += line; System.out.println(out);
    }
  }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/GetDeliveriesByCampaignID" \
  -H "Authorization: MyToken" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "CampaignID=76581373"
```

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در صورت خطا نیز **HTTP 200 (OK)** بازمی‌گردد و نتیجهٔ عملیات از مقدار <span dir="ltr">Status</span> تشخیص داده می‌شود؛ مانند نمونهٔ «BulkID_Invalid» بالا.

</div>
