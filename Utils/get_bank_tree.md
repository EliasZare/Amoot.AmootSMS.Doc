<div dir="rtl">

# مستندات وب‌سرویس بانک اطلاعاتی - دریافت لیست بانک اطلاعاتی (GetBankTree)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/GetBankTree](https://portal.amootsms.com/rest/GetBankTree)

## توضیح کلی

با استفاده از این متد می‌توانید **لیست درختی بانک اطلاعاتی شماره‌های همراه** را دریافت کنید. ساختار خروجی به‌صورت درخت (Tree) است و معمولاً سطوحی مانند استان/شهر/محله یا شاخه‌های موضوعی را شامل می‌شود. از این داده‌ها می‌توان برای نمایش سلسله‌مراتبی و انتخاب هدف ارسال استفاده کرد.

## پارامترهای ورودی

| نام   | نوع    | الزامی | توضیحات                                                                            |
| ----- | ------ | :----: | ---------------------------------------------------------------------------------- |
| Token | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                       |
| ------ | ------ | :----: | ------------------------------------------------------------- |
| Status | String |   خیر  | مقدار <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا |
| Data   | Object |   خیر  | در موفقیت شامل آرایه/درختِ گره‌ها (BankTree)                  |

### نمونه خروجی موفق (نمونهٔ ساختار درخت)

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": {
    "BankTree": [
      {
        "Id": 1,
        "Title": "استان تهران",
        "Count": 350000,
        "Children": [
          {
            "Id": 11,
            "Title": "تهران",
            "Count": 280000,
            "Children": [
              { "Id": 111, "Title": "منطقه 1", "Count": 15000, "Children": [] },
              { "Id": 112, "Title": "منطقه 2", "Count": 18000, "Children": [] }
            ]
          },
          { "Id": 12, "Title": "ری", "Count": 20000, "Children": [] }
        ]
      },
      { "Id": 2, "Title": "استان اصفهان", "Count": 210000, "Children": [] }
    ]
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Token_Invalid",
  "Data": null
}
```

</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا (الگوی نوع A)

در این الگو، متد در صورت بروز خطا با **HTTP 200 (OK)** پاسخ می‌دهد اما بدنهٔ پاسخ نشان‌دهندهٔ خطاست. اگر مقدار <span dir="ltr">Status = Success</span> باشد، عملیات موفق بوده و <span dir="ltr">Data</span> شامل ساختار درختی بانک اطلاعاتی است.

---

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection();
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetBankTree", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/GetBankTree"
headers = {"Authorization": "MyToken"}
res = requests.post(url, headers=headers, data={})
print(res.text)
```

### PHP

```php
$Token = "MyToken";
$ch = curl_init("https://portal.amootsms.com/rest/GetBankTree");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // JSON
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/GetBankTree', {}, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
URL url = new URL("https://portal.amootsms.com/rest/GetBankTree");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setRequestProperty("Authorization", "MyToken");
conn.setDoOutput(true);
try(OutputStream os = conn.getOutputStream()){ os.write(new byte[0]); }
try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))){
  String line; StringBuilder sb = new StringBuilder();
  while((line = br.readLine()) != null) sb.append(line);
  System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/GetBankTree" \
  -H "Authorization: MyToken"
```

## نکات اجرایی

* پاسخ این متد ممکن است حجیم باشد؛ در کلاینت خود از **paging/virtualization** در نمایش درخت استفاده کنید.
* نودها معمولاً شامل فیلدهایی نظیر <span dir="ltr">Id</span>، <span dir="ltr">Title</span>، <span dir="ltr">Count</span> و <span dir="ltr">Children</span> هستند (ممکن است با پیاده‌سازی شما تفاوت جزئی داشته باشد).

## تغییرات (Changelog)

| تاریخ      | نسخه  | توضیح                        |
| ---------- | ----- | ---------------------------- |
| ۱۴۰۲/۰۵/۰۱ | 1.0.0 | ایجاد سند بر پایهٔ قالب واحد |

</div>
