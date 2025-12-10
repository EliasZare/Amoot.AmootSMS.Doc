<div dir="rtl">

# مستندات وب‌سرویس خطوط - استعلام بلک‌لیست بر اساس لغو (IsBlackList_ByCancelation)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/IsBlackList_ByCancelation](https://portal.amootsms.com/rest/IsBlackList_ByCancelation)

## توضیح کلی

با این متد می‌توانید بررسی کنید که **یک شمارهٔ موبایل** برای **یک خطِ مشخص** در فهرست بلک‌لیست ناشی از لغو دریافت (Cancelation) قرار دارد یا خیر. اگر مقدار خط را به‌صورت <span dir="ltr">"OTP"</span> ارسال کنید، سامانه به‌صورت خودکار **یکی از خطوط مجاز الگو/OTP** را انتخاب کرده و بر همان خط استعلام را انجام می‌دهد.

---

## پارامترهای ورودی

| نام        | نوع    | الزامی | توضیحات                                                                                    |
| ---------- | ------ | :----: | ------------------------------------------------------------------------------------------ |
| Token      | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود         |
| Mobile     | String |   بله  | شمارهٔ موبایل مقصد؛ باید معتبر باشد (در سرور نرمال‌سازی می‌شود)                            |
| LineNumber | String |   بله  | شمارهٔ خط ارسال؛ اگر <span dir="ltr">OTP</span> ارسال شود، یکی از خطوط الگو انتخاب می‌گردد |

> اعتبارسنجی ورودی‌ها: در صورت خالی بودن یا نامعتبر بودن موبایل، به‌ترتیب <span dir="ltr">Mobile_Empty</span> و <span dir="ltr">Mobile_Invalid</span> برگردانده می‌شود. اگر خط وجود نداشته باشد: <span dir="ltr">LineNumber_NotExist</span>.

---

## مقدار خروجی

| نام        | نوع           | الزامی | توضیحات                                                                                              |
| ---------- | ------------- | :----: | ---------------------------------------------------------------------------------------------------- |
| Status     | String        |   خیر  | مقدار <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا در غیر این صورت                        |
| LineNumber | String        |   خیر  | شمارهٔ خطی که استعلام روی آن انجام شده (ممکن است به‌جای <span dir="ltr">OTP</span>، خط واقعی برگردد) |
| Data       | Array<Object> |   خیر  | آرایه‌ای با یک آیتم شامل نتیجهٔ استعلام                                                              |

### ساختار آیتم‌های <span dir="ltr">Data</span>

| نام فیلد    | نوع  | توضیحات                                                 |
| ----------- | ---- | ------------------------------------------------------- |
| Number      | Long | شمارهٔ موبایل نرمال‌شده (با پیش‌شمارهٔ استاندارد)       |
| IsBlackList | Bool | آیا شماره برای این خط در بلک‌لیست لغو دریافت قرار دارد؟ |

---

</div>

---

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
string Mobile = "09120000000";
string LineNumber = "OTP"; // یا شمارهٔ خط واقعی مثل "5000xxxx"

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "LineNumber", LineNumber }
    };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/IsBlackList_ByCancelation", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/IsBlackList_ByCancelation"
headers = {"Authorization": "MyToken"}
payload = {"Mobile": "09120000000", "LineNumber": "OTP"}
res = requests.post(url, headers=headers, data=payload)
print(res.text)
```

### PHP

```php
$Token = "MyToken";
$data = [
  'Mobile' => '09120000000',
  'LineNumber' => 'OTP' // یا خط واقعی
];
$ch = curl_init('https://portal.amootsms.com/rest/IsBlackList_ByCancelation');
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/IsBlackList_ByCancelation', {
  Mobile: '09120000000',
  LineNumber: 'OTP'
}, { headers: { Authorization: 'MyToken' }})
.then(r => console.log(r.data))
.catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
String token = "MyToken";
String data = String.join("&",
  "Mobile=09120000000",
  "LineNumber=OTP"
);
URL url = new URL("https://portal.amootsms.com/rest/IsBlackList_ByCancelation");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setRequestProperty("Authorization", token);
conn.setDoOutput(true);
try(OutputStream os = conn.getOutputStream()) { os.write(data.getBytes(StandardCharsets.UTF_8)); }
try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
  String line; StringBuilder sb = new StringBuilder();
  while((line = br.readLine()) != null) sb.append(line);
  System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/IsBlackList_ByCancelation" \
  -H "Authorization: MyToken" \
  -d "Mobile=09120000000" \
  -d "LineNumber=OTP"
```


---

## نمونه خروجی موفق

<div dir="ltr">

```json
{
    "Status": "Success",
    "LineNumber": "Public",
    "Data": [
        {
            "Number": 989123456789,
            "IsBlackList": false
        }
    ]
}
```

</div>

## نمونه خروجی ناموفق (پارامتر نامعتبر)

<div dir="ltr">

```json
{
  "Status": "Mobile_Invalid",
  "LineNumber": null,
  "Data": []
}
```

</div>

## نمونه خروجی ناموفق (خط وجود ندارد)

<div dir="ltr">

```json
{
    "Status": "LineNumber_NotExist",
    "LineNumber": null,
    "Data": null
}
```

</div>

## نمونه خروجی ناموفق (احراز هویت)

<div dir="ltr">

```json
{
  "Status": "Token_Invalid",
  "LineNumber": null,
  "Data": []
}
```

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این الگو، پاسخ HTTP همواره **200 (OK)** است؛ موفقیت/خطا از طریق فیلد <span dir="ltr">Status</span> مشخص می‌شود.

* **موفق**: <span dir="ltr">Status = "Success"</span> و فیلد <span dir="ltr">Data</span> شامل نتیجهٔ استعلام است.
* **ناموفق**: مانند <span dir="ltr">Mobile_Empty</span>، <span dir="ltr">Mobile_Invalid</span>، <span dir="ltr">LineNumber_NotExist</span>، یا خطاهای احراز هویت؛ معمولاً <span dir="ltr">Data</span> تهی یا خالی است.


</div>
