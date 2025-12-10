<div dir="rtl">

# مستندات وب‌سرویس دریافتی‌ها - دریافت پیامک‌های دریافتی (RecieveMessages)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RecieveMessages](https://portal.amootsms.com/rest/RecieveMessages)

## توضیح کلی

با این متد می‌توانید پیامک‌های **دریافتی** خود را در بازهٔ زمانی مشخص از سامانه آموت دریافت کنید. در صورت تعیین «LineNumber»، فقط پیامک‌های دریافتی همان خط بازگردانده می‌شود.

## پارامترهای ورودی

| نام          | نوع    | الزامی | توضیحات                                                                             |
| ------------ | ------ | :----: | ----------------------------------------------------------------------------------- |
| Token        | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود  |
| FromDateTime | String |   بله  | تاریخ/زمان آغاز بازه با فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span> یا ISO8601  |
| ToDateTime   | String |   بله  | تاریخ/زمان پایان بازه با فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span> یا ISO8601 |
| LineNumber   | String |   خیر  | شماره خط دلخواه؛ در صورت ارسال، فیلتر بر اساس همین خط اعمال می‌شود                  |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                      |
| ------ | ------ | :----: | ------------------------------------------------------------ |
| Status | String |   خیر  | «Success» در صورت موفقیت یا یکی از کدهای خطا                 |
| Data   | Array  |   خیر  | در حالت موفق شامل آرایهٔ پیام‌ها؛ در خطا معمولاً آرایهٔ خالی |


### نمونه خروجی‌های واقعیِ ناموفق 

#### 1) توکن ناموجود

```json
{
  "Status": "Token_NotExists",
  "Data": []
}
```

#### 2) خطای عمومی

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
var fromDate = "2024-09-01 00:00:00";
var toDate   = "2024-10-01 00:00:00";
var line     = ""; // اختیاری

using (var wc = new WebClient())
{
    wc.Headers.Add(HttpRequestHeader.Authorization, token);
    var form = new NameValueCollection {
        {"FromDateTime", fromDate},
        {"ToDateTime", toDate},
        {"LineNumber", line}
    };
    var bytes = wc.UploadValues("https://portal.amootsms.com/rest/RecieveMessages", form);
    var body = Encoding.UTF8.GetString(bytes);
    Console.WriteLine(body); // شامل Status و Data
}
```

### Python

```python
import requests

token = "MyToken"
url = "https://portal.amootsms.com/rest/RecieveMessages"
form = {
    "FromDateTime": "2024-09-01 00:00:00",
    "ToDateTime": "2024-10-01 00:00:00",
    "LineNumber": ""  # اختیاری
}
headers = {"Authorization": token}
resp = requests.post(url, data=form, headers=headers)
print(resp.json())  # {"Status": ..., "Data": [...]}
```

### PHP

```php
<?php
$token = "MyToken";
$url = "https://portal.amootsms.com/rest/RecieveMessages";
$data = [
  "FromDateTime" => "2024-09-01 00:00:00",
  "ToDateTime"   => "2024-10-01 00:00:00",
  "LineNumber"   => ""
];
$ch = curl_init($url);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```js
const fetch = (...args) => import('node-fetch').then(({default: fetch}) => fetch(...args));

const token = 'MyToken';
const url = 'https://portal.amootsms.com/rest/RecieveMessages';

const body = new URLSearchParams({
  FromDateTime: '2024-09-01 00:00:00',
  ToDateTime: '2024-10-01 00:00:00',
  LineNumber: '' // اختیاری
});

const res = await fetch(url, {
  method: 'POST',
  headers: { 'Authorization': token, 'Content-Type': 'application/x-www-form-urlencoded' },
  body
});
console.log(await res.text());
```

### Java

```java
import java.io.*;
import java.net.*;

public class RecieveMessagesExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RecieveMessages");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
    conn.setDoOutput(true);
    String body = "FromDateTime=2024-09-01 00:00:00&ToDateTime=2024-10-01 00:00:00&LineNumber=";
    try(OutputStream os = conn.getOutputStream()) { os.write(body.getBytes()); }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"))) {
      String line; StringBuilder sb = new StringBuilder();
      while((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString());
    }
  }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/RecieveMessages" \
  -H "Authorization: MyToken" \
  -d "FromDateTime=2024-09-01 00:00:00" \
  -d "ToDateTime=2024-10-01 00:00:00" \
  -d "LineNumber="
```

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در صورت خطا نیز کُد HTTP، مقدار **200 (OK)** دارد و خطا با مقدار <span dir="ltr">Status</span> مشخص می‌شود. نمونه‌های مشاهده‌شده:

```json
{ "Status": "Token_NotExists", "Data": [] }
```

```json
{ "Status": "Failed", "Data": [] }
```

</div>
