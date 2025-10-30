<div dir="rtl">

# مستندات وب‌سرویس الگوها - دریافت لیست الگوها (PatternCodeList)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/PatternCodeList](https://portal.amootsms.com/rest/PatternCodeList)

## توضیح کلی

از طریق این متد می‌توانید **لیست الگوهای ثبت‌شدهٔ کاربر** را دریافت کنید. خروجی شامل مجموعه‌ای از الگوهاست که هر مورد دارای شناسه، وضعیت، نوع OTP بودن، متن الگو و نام الگوها (Patterns) و نیز فهرست الگوهای عددی (NumberPatterns) است.

---

## پارامترهای ورودی

| نام   | نوع    | الزامی | توضیحات                                                                            |
| ----- | ------ | :----: | ---------------------------------------------------------------------------------- |
| Token | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود |

---

## مقدار خروجی

| نام    | نوع           | الزامی | توضیحات                                                                       |
| ------ | ------------- | :----: | ----------------------------------------------------------------------------- |
| Status | String        |   خیر  | مقدار <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا در غیر این صورت |
| Data   | Array<Object> |   خیر  | آرایه‌ای از الگوها؛ هر شیء نمایندهٔ یک الگو است                               |

### ساختار هر آیتم در <span dir="ltr">Data</span>

| نام فیلد       | نوع           | توضیحات                                                                       |
| -------------- | ------------- | ----------------------------------------------------------------------------- |
| ID             | Long          | شناسهٔ الگو                                                                   |
| Status         | Int           | وضعیت الگو (مثلاً فعال/غیرفعال؛ مقدار عددی ذخیره‌شده)                         |
| IsOTP          | Bool          | آیا الگو از نوع OTP است؟                                                      |
| MessageText    | String        | متن الگو                                                                      |
| Patterns       | Array<String> | نام الگوهای استخراج‌شده از متن (مثلاً <span dir="ltr">["code"]</span>)        |
| NumberPatterns | Array<String> | نام الگوهایی که باید صرفاً عددی باشند (مثلاً <span dir="ltr">["code"]</span>) |

---

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": [
    {
      "ID": 1001,
      "Status": 1,
      "IsOTP": true,
      "MessageText": "کد تایید شما: %code%",
      "Patterns": ["code"],
      "NumberPatterns": ["code"]
    },
    {
      "ID": 1002,
      "Status": 1,
      "IsOTP": false,
      "MessageText": "کاربر گرامی %fullname%، خوش آمدید",
      "Patterns": ["fullname"],
      "NumberPatterns": []
    },
    {
      "ID": 1003,
      "Status": 3,
      "IsOTP": false,
      "MessageText": "سلام %name% عزیز، سفارش شما ثبت شد. کد پیگیری: %tracking%",
      "Patterns": ["name", "tracking"],
      "NumberPatterns": ["tracking"]
    }
  ]
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Token_Invalid",
  "Data": []
}
```

</div>

---

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این الگو، سرویس حتی در حالت خطا نیز با **HTTP 200 (OK)** پاسخ می‌دهد، اما نتیجهٔ واقعی در بدنه بازتاب داده می‌شود:

* **موفق:** وقتی <span dir="ltr">Status = "Success"</span> باشد؛ در این حالت <span dir="ltr">Data</span> شامل آرایهٔ الگوهاست.
* **ناموفق:** هر مقدار دیگر برای <span dir="ltr">Status</span> (مانند <span dir="ltr">Token_Invalid</span>، <span dir="ltr">Token_Expired</span> و …)؛ معمولاً <span dir="ltr">Data</span> تهی یا <span dir="ltr">null</span> است.

---

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection();
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/PatternCodeList", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/PatternCodeList"
headers = {"Authorization": "MyToken"}
res = requests.post(url, headers=headers, data={})
print(res.text)
```

### PHP

```php
$Token = "MyToken";
$ch = curl_init('https://portal.amootsms.com/rest/PatternCodeList');
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([]));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // JSON
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/PatternCodeList', {}, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
URL url = new URL("https://portal.amootsms.com/rest/PatternCodeList");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setRequestProperty("Authorization", "MyToken");
conn.setDoOutput(true);
try(OutputStream os = conn.getOutputStream()) { os.write(new byte[0]); }
try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
  String line; StringBuilder sb = new StringBuilder();
  while((line = br.readLine()) != null) sb.append(line);
  System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/PatternCodeList" \
  -H "Authorization: MyToken"
```

</div>
