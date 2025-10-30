<div dir="rtl">

# مستندات وب سرویس مخاطبین - حذف مخاطب (ContactDelete)


**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

---

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/ContactDelete](https://portal.amootsms.com/rest/ContactDelete)

## توضیحات

از طریق این متد می‌توانید یک مخاطب را از دفترچه تلفن پنل کاربری خود حذف کنید. احراز هویت با ارسال توکن در هدر انجام می‌شود و بدنهٔ درخواست به‌صورت فرم (x-www-form-urlencoded یا multipart/form-data) شامل شناسهٔ مخاطب است.

## پارامترهای ورودی

| نام       | نوع    | الزامی | توضیحات                                                                            |
| --------- | ------ | :----: | ---------------------------------------------------------------------------------- |
| Token     | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود |
| ContactID | Long   |   بله  | کد یکتای مخاطب برای حذف                                                            |

## مقدار خروجی

| نام         | نوع    | الزامی | توضیحات                                                                 |
| ----------- | ------ | :----: | ----------------------------------------------------------------------- |
| ReturnValue | Object |   خیر  | نتیجهٔ عملیات حذف (true/false یا شیء شامل جزئیات)                       |
| Status      | String |   خیر  | مقدار <span dir="ltr">Success</span> در صورت موفقیت یا یکی از کدهای خطا |

## نمونه کدها

<div dir=ltr>

### C#

```csharp
string Token = "MyToken";
long ContactID = 0;

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactDelete", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/ContactDelete"
headers = {"Authorization": "MyToken"}
data = {"ContactID": 0}

response = requests.post(url, headers=headers, data=data)
print(response.text)  # خروجی JSON
```

### PHP

```php
$Token = "MyToken";
$ContactID = 0;

$data = array(
    "ContactID" => $ContactID
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "https://portal.amootsms.com/rest/ContactDelete");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    "Authorization: $Token"
));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);

echo $response; // خروجی JSON
```

### Node.js

```js
const axios = require('axios');
const token = 'MyToken';
const ContactID = 0;

axios.post(
  'https://portal.amootsms.com/rest/ContactDelete',
  { ContactID },
  { headers: { Authorization: token } }
)
.then(res => console.log(res.data))
.catch(err => console.error(err.response ? err.response.data : err.message));
```

### Java

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;

public class ContactDelete {
    public static void main(String[] args) {
        String token = "MyToken";
        long contactID = 0;
        String urlString = "https://portal.amootsms.com/rest/ContactDelete";

        try {
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("POST");
            conn.setRequestProperty("Authorization", token);
            conn.setDoOutput(true);

            String data = "ContactID=" + contactID;

            try(OutputStream os = conn.getOutputStream()) {
                byte[] input = data.getBytes(StandardCharsets.UTF_8);
                os.write(input, 0, input.length);
            }

            BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8));
            StringBuilder response = new StringBuilder();
            String line;
            while ((line = br.readLine()) != null) {
                response.append(line.trim());
            }

            System.out.println(response.toString());

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactDelete" \
  -H "Authorization: MyToken" \
  -F "ContactID=0"
```
</div>


## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در این الگو، متدهای وب‌سرویس در صورت بروز خطا **همیشه با کد وضعیت HTTP = 200 (OK)** پاسخ می‌دهند، اما بدنهٔ پاسخ نشان‌دهندهٔ خطاست. 


‌ بدنه یک شیء JSON است که در آن فیلد Status برابر **کد خطا** و فیلد Data معمولاً با مقادیر پیش‌فرض/خالی است؛ مثال:

<div dir="ltr">

```json
{
  "Status": "ContactID_Invalid",
  "Data": { /* ... */ }
}
```

</div>

> نتیجه‌گیری: **HTTP 200** الزاماً به معنی موفقیت نیست. همواره مقدار <span dir="ltr">Status</span> یا متن بدنه را بررسی کنید.

---

### الگوی تشخیص موفق/ناموفق

* **موفق:** وقتی <span dir="ltr">Status == "Success"</span> (در پاسخ JSON) یا وقتی بدنهٔ پاسخ طبق مستند، خروجی موفق را برگرداند.
* **ناموفق:** هر مقدار دیگری در <span dir="ltr">Status</span> (مانند <span dir="ltr">Token_Invalid</span>) یا وقتی بدنهٔ پاسخ **رشتهٔ متنیِ کد خطا** باشد.

</div>
