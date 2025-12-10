<div dir="rtl">

# مستندات وب‌سرویس گزارش دلیوری - دریافت دلیوری چند پیامک (GetDeliveries)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

---

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/GetDeliveries](https://portal.amootsms.com/rest/GetDeliveries)

## توضیح کلی

این متد وضعیت تحویل (Delivery) چند پیامک را به‌صورت گروهی برمی‌گرداند. شما لیستی از **MessageID**‌ها (کد پیامک/شماره کمپین) ارسال می‌کنید و سرویس برای هر کدام یک رکورد دلیوری بازمی‌گرداند.

## پارامترهای ورودی

| نام           | نوع    | الزامی | توضیحات                                                                                |
| ------------- | ------ | :----: | -------------------------------------------------------------------------------------- |
| Token         | String |   بله  | در هدر <span dir="ltr">Authorization</span> ارسال شود                                  |
| ListMessageID | String |   بله  | لیست شناسه‌ها به‌صورت رشتهٔ کاماگذاری‌شده (مثال: <span dir="ltr">"123,124,125"</span>) |

> **Content-Type:** <span dir="ltr">application/x-www-form-urlencoded</span>

## مقدار خروجی (واقعی)

| نام    | نوع    | توضیحات                                                                 |
| ------ | ------ | ----------------------------------------------------------------------- |
| Status | String | مقدار «Success» در صورت موفقیت یا «Failed» در خطا                       |
| Data   | Array  | آرایه‌ای از نتایج دلیوری برای هر MessageID (در خطا معمولاً آرایهٔ خالی) |

### فیلدهای هر آیتم از `Data`

| نام          | نوع    | توضیحات                                                                      |
| ------------ | ------ | ---------------------------------------------------------------------------- |
| MessageID    | Long   | شناسهٔ پیامک                                                                 |
| DeliveryType | Int    | نوع دلیوری (کد عددی)                                                         |
| Cost         | Int    | هزینه                                                                        |
| RegDateTime  | String | زمان ثبت   |
| SendDateTime | String | زمان ارسال |
| Status       | String | وضعیت آن آیتم                                              |

### نمونه خروجی موفق

```json
{
    "Status": "Success",
    "Data": [
        {
            "MessageID": 123456789,
            "DeliveryType": 1,
            "Cost": 0,
            "RegDateTime": "2025-10-07T11:26:26.31",
            "SendDateTime": "2025-10-07T11:31:34.51",
            "Status": "Success"
        }
    ]
}
```

### نمونه خروجی ناموفق

```json
{
  "Status": "Failed",
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
long[] ids = { 76641768, 76641769 };
using (var wc = new WebClient())
{
    wc.Headers.Add(HttpRequestHeader.Authorization, token);
    var form = new NameValueCollection { { "ListMessageID", string.Join(",", ids) } };
    var resp = wc.UploadValues("https://portal.amootsms.com/rest/GetDeliveries", form);
    Console.WriteLine(Encoding.UTF8.GetString(resp));
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/GetDeliveries"
headers = {"Authorization": "MyToken"}
form = {"ListMessageID": "76641768,76641769"}
res = requests.post(url, data=form, headers=headers)
print(res.json())
```

### PHP

```php
<?php
$token = 'MyToken';
$ids = '76641768,76641769';
$ch = curl_init('https://portal.amootsms.com/rest/GetDeliveries');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(['ListMessageID' => $ids]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
echo curl_exec($ch);
curl_close($ch);
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/GetDeliveries',
  new URLSearchParams({ ListMessageID: '76641768,76641769' }).toString(),
  { headers: { Authorization: 'MyToken', 'Content-Type': 'application/x-www-form-urlencoded' } }
).then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

### Java

```java
import java.io.*; import java.net.*;
public class GetDeliveriesExample {
  public static void main(String[] args) throws Exception {
    URL url = new URL("https://portal.amootsms.com/rest/GetDeliveries");
    HttpURLConnection c = (HttpURLConnection) url.openConnection();
    c.setRequestMethod("POST");
    c.setRequestProperty("Authorization", "MyToken");
    c.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    c.setDoOutput(true);
    String body = "ListMessageID=76641768,76641769";
    try(OutputStream os = c.getOutputStream()){ os.write(body.getBytes()); }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(c.getInputStream(), "utf-8"))){
      String line, out = ""; while((line = br.readLine()) != null) out += line; System.out.println(out);
    }
  }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/GetDeliveries" \
  -H "Authorization: MyToken" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "ListMessageID=76641768,76641769"
```

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

این متد حتی در خطا نیز **HTTP 200 (OK)** بازمی‌گردد و نتیجه از فیلد <span dir="ltr">Status</span> تشخیص داده می‌شود.

</div>
