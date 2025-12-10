<div dir="rtl">

# مستندات وب‌سرویس الگوها - ایجاد الگو (PatternCodeCreate)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/PatternCodeCreate](https://portal.amootsms.com/rest/PatternCodeCreate)

## توضیح کلی

هدف این متد ایجاد یا ثبت یک «الگوی پیامک» بر اساس ورودی‌های ارسالی از سمت کاربر است. در متن پیام از متغیرهای دارای علامت درصد (مانند <span dir="ltr">%code%</span>) برای تعریف الگوها استفاده می‌شود. احراز هویت از طریق هدر <span dir="ltr">Authorization</span> انجام می‌شود.

---

## پارامترهای ورودی

| نام                | نوع    | الزامی | توضیحات                                                                                             |
| ------------------ | ------ | :----: | --------------------------------------------------------------------------------------------------- |
| Token              | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ باید در هدر Authorization ارسال شود.                                   |
| MessageText        | String |   بله  | متن پیامک حاوی الگوهای تعریف‌شده (مانند <span dir="ltr">%code%</span>)                              |
| NumberPatterns     | String |   خیر  | رشته‌ای از نام الگوهای عددی که در سرور به لیست تبدیل می‌شود؛ اگر خالی باشد، مقدار آن null خواهد شد. |
| IsOTP              | Bool   |   خیر  | مشخص می‌کند الگو از نوع OTP است یا خیر.                                                             |
| RequestDescription | String |   خیر  | توضیح تکمیلی در خصوص درخواست.                                                                       |

---

## مقدار خروجی

| نام              | نوع    | الزامی | توضیحات                                                         |
| ---------------- | ------ | :----: | --------------------------------------------------------------- |
| Status           | String |   خیر  | مقدار Success در صورت موفقیت یا Failed در صورت بروز خطا.        |
| ValidationResult | Object |   خیر  | نتیجهٔ اعتبارسنجی شامل کد وضعیت و پیام توضیحی.                  |
| Data             | Object |   خیر  | اطلاعات الگوی ثبت‌شده در صورت موفقیت یا مقادیر تهی در صورت خطا. |

### نمونه کدها

<div dir="ltr">

#### C#

```csharp
string Token = "MyToken";
string MessageText = "خوش آمدید: کد تایید شما %code%";
string NumberPatterns = "code"; // در صورت نیاز؛ در سرور به لیست تبدیل می‌شود
bool IsOTP = true;
string RequestDescription = "ثبت الگوی OTP خوشامد";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "MessageText", MessageText },
        { "NumberPatterns", NumberPatterns },
        { "IsOTP", IsOTP.ToString() },
        { "RequestDescription", RequestDescription }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/PatternCodeCreate", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json); // خروجی JSON شامل Status, ValidationResult, Data
}
```

---

#### Python

```python
import requests

url = "https://portal.amootsms.com/rest/PatternCodeCreate"
headers = {"Authorization": "MyToken"}
payload = {
    "MessageText": "خوش آمدید: کد تایید شما %code%",
    "NumberPatterns": "code",  # optional
    "IsOTP": True,
    "RequestDescription": "ثبت الگوی OTP"
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```

---

#### PHP

```php
$Token = "MyToken";
$data = [
    "MessageText" => "خوش آمدید: کد تایید شما %code%",
    "NumberPatterns" => "code", // اختیاری
    "IsOTP" => true,
    "RequestDescription" => "ثبت الگوی OTP"
];

$ch = curl_init("https://portal.amootsms.com/rest/PatternCodeCreate");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

---

#### Node.js

```js
const axios = require('axios');

const token = 'MyToken';
axios.post('https://portal.amootsms.com/rest/PatternCodeCreate', {
  MessageText: 'خوش آمدید: کد تایید شما %code%',
  NumberPatterns: 'code', // optional
  IsOTP: true,
  RequestDescription: 'ثبت الگوی OTP'
}, { headers: { Authorization: token } })
.then(res => console.log(res.data))
.catch(err => console.error(err.response ? err.response.data : err.message));
```

---

#### Java

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;

public class PatternCodeCreate {
    public static void main(String[] args) throws Exception {
        String token = "MyToken";
        String data = String.join("&",
            "MessageText=" + URLEncoder.encode("خوش آمدید: کد تایید شما %code%", "UTF-8"),
            "NumberPatterns=code",
            "IsOTP=true",
            "RequestDescription=" + URLEncoder.encode("ثبت الگوی OTP", "UTF-8")
        );

        URL url = new URL("https://portal.amootsms.com/rest/PatternCodeCreate");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Authorization", token);
        conn.setDoOutput(true);

        try (OutputStream os = conn.getOutputStream()) {
            os.write(data.getBytes(StandardCharsets.UTF_8));
        }

        try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = br.readLine()) != null) {
                response.append(line);
            }
            System.out.println(response.toString());
        }
    }
}
```

---

#### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/PatternCodeCreate" \
  -H "Authorization: MyToken" \
  -d "MessageText=خوش آمدید: کد تایید شما %code%" \
  -d "NumberPatterns=code" \
  -d "IsOTP=true" \
  -d "RequestDescription=ثبت الگوی OTP"
```

</div>

### نمونه خروجی موفق


```json
{
  "Status": "Success",
  "ValidationResult": {
    "Status": 1,
    "Message": null
  },
  "Data": {
    "ID": 1234,
    "Status": 0,
    "IsOTP": true,
    "MessageText": "خوش آمدید: کد تایید شما %code%",
    "Patterns": ["code"],
    "NumberPatterns": ["code"]
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Failed",
  "ValidationResult": {
    "Status": 3,
    "Message": "در متن پیامک الگو تعریف نشده است."
  },
  "Data": {
    "ID": 0,
    "Status": 0,
    "IsOTP": false,
    "MessageText": null,
    "Patterns": null,
    "NumberPatterns": null
  }
}
```

</div>

---

## منطق اعتبارسنجی ورودی‌ها

سیستم در هنگام ثبت الگو، بررسی‌های زیر را انجام می‌دهد:

1. **خالی نبودن متن پیامک** – در صورت تهی بودن فیلد <span dir="ltr">MessageText</span>، عملیات با خطا پایان می‌یابد.
2. **وجود حداقل یک الگو در متن** – پیام باید حداقل یک نام الگو مانند <span dir="ltr">%code%</span> داشته باشد.
3. **تکراری نبودن نام الگوها** – اگر نام الگوها تکراری باشند، خطا صادر می‌شود.
4. **تطابق NumberPatterns با الگوهای متن** – در صورت ارسال <span dir="ltr">NumberPatterns</span>، همهٔ اقلام باید از میان الگوهای استخراج‌شده از متن باشند.
5. **تکراری نبودن متن پیام برای کاربر** – در صورت ثبت قبلی همان متن برای همان کاربر، عملیات رد می‌شود.

در صورت موفقیت، پاسخ با وضعیت «Success» بازگردانده می‌شود.

### مقادیر وضعیت اعتبارسنجی

مقادیر وضعیت اعتبارسنجی می‌تواند شامل موارد زیر باشد:

| مقدار                                 | عدد | شرح                                                           |
| ------------------------------------- | --- | ------------------------------------------------------------- |
| Success                               | 1   | عملیات با موفقیت انجام شده است.                               |
| MessageText_Empty                     | 2   | متن پیامک خالی است.                                           |
| Patterns_Empty                        | 3   | در متن پیامک هیچ الگویی تعریف نشده است.                       |
| Patterns_HasDuplicateNames            | 4   | در متن پیامک الگوهای تکراری وجود دارد.                        |
| NumberPatterns_HasOutsidePatternNames | 5   | الگوهای عددی ارسال‌شده با الگوهای موجود در متن مطابقت ندارند. |
| MessageText_IsDuplicated              | 6   | متن پیامک تکراری است.                                         |

---

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در این الگو، سرویس حتی در صورت بروز خطا نیز با HTTP 200 پاسخ می‌دهد، اما بدنهٔ پاسخ مشخص‌کنندهٔ نتیجه است.

* در صورت موفقیت: مقدار <span dir="ltr">Status = Success</span> است.
* در صورت خطا: مقدار <span dir="ltr">Status = Failed</span> و جزئیات در ValidationResult نمایش داده می‌شود.

---

## قوانین استفاده

* متن پیام نباید خالی باشد و باید حداقل یک نام الگو در آن وجود داشته باشد.
* نام الگوها نباید تکراری باشند.
* اگر NumberPatterns ارسال شود، باید کاملاً با نام الگوهای داخل متن منطبق باشد.
* متن تکراری برای همان کاربر ثبت نخواهد شد.
* اگر NumberPatterns تهی باشد، مقدار آن در سرور null تنظیم می‌شود.


</div>
