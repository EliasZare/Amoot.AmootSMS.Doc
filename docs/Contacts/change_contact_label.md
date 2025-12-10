<div dir="rtl">

# مستندات وب سرویس مخاطبین - تغییر عنوان برچسب (ContactChangeLabel)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

---

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/ContactChangeLabel](https://portal.amootsms.com/rest/ContactChangeLabel)

---

## توضیحات

با استفاده از این متد می‌توان عنوان برچسب تمام مخاطبین مربوط به همان برچسب را تغییر داد.

---

## پارامترهای ورودی

| نام       | نوع    | الزامی | توضیحات                           |
| --------- | ------ | :----: | --------------------------------- |
| Token     | String |    بله   | توکن ثبت‌شده در سامانه پیامک آموت |
| FromLabel | String |    بله   | عنوان فعلی برچسب                  |
| ToLabel   | String |    بله   | عنوان جدید برچسب                  |

---

## مقدار خروجی

| نام         | نوع    | الزامی | توضیحات                    |
| ----------- | ------ | :----: | -------------------------- |
| - | String |    خیر   | نتیجه عملیات (موفق یا خطا) |

---

## نمونه کدها

<div dir=ltr>

###  C#

```csharp
string Token = "MyToken";
string FromLabel = "OldLabel";
string ToLabel = "NewLabel";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "FromLabel", FromLabel },
        { "ToLabel", ToLabel }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactChangeLabel", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json); // خروجی JSON
}
```

---

###  Python

```python
import requests

Token = "MyToken"
FromLabel = "OldLabel"
ToLabel = "NewLabel"

url = "https://portal.amootsms.com/rest/ContactChangeLabel"
headers = {
    "Authorization": Token
}
data = {
    "FromLabel": FromLabel,
    "ToLabel": ToLabel
}

response = requests.post(url, headers=headers, data=data)
print(response.text)  # خروجی JSON
```

---

###  PHP

```php
$Token = "MyToken";
$FromLabel = "OldLabel";
$ToLabel = "NewLabel";

$data = array(
    "FromLabel" => $FromLabel,
    "ToLabel" => $ToLabel
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "https://portal.amootsms.com/rest/ContactChangeLabel");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, array("Authorization: $Token"));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);

echo $response; // خروجی JSON
```

---

###  Node.js

```javascript
const axios = require('axios');

const Token = 'MyToken';
const FromLabel = 'OldLabel';
const ToLabel = 'NewLabel';

axios.post('https://portal.amootsms.com/rest/ContactChangeLabel', {
    FromLabel,
    ToLabel
}, {
    headers: {
        'Authorization': Token
    }
})
.then(response => {
    console.log(response.data); // خروجی JSON
})
.catch(error => {
    console.error(error);
});
```

---

###  Java

```java
import java.io.*;
import java.net.*;
import java.net.URLEncoder;

public class ContactChangeLabelExample {
    public static void main(String[] args) throws Exception {
        String Token = "MyToken";
        String FromLabel = "OldLabel";
        String ToLabel = "NewLabel";

        String data = "FromLabel=" + URLEncoder.encode(FromLabel, "UTF-8") +
                      "&ToLabel=" + URLEncoder.encode(ToLabel, "UTF-8");

        URL url = new URL("https://portal.amootsms.com/rest/ContactChangeLabel");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Authorization", Token);
        conn.setDoOutput(true);

        conn.getOutputStream().write(data.getBytes("UTF-8"));

        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        StringBuilder response = new StringBuilder();
        String line;
        while ((line = in.readLine()) != null) {
            response.append(line);
        }
        in.close();
        System.out.println(response.toString()); // خروجی JSON
    }
}
```

---

###  cURL

```bash
curl --location 'https://portal.amootsms.com/rest/ContactChangeLabel' \
--header 'Authorization: MyToken1234' \
--header 'Content-Type: application/json' \
--data '{
    "FromLabel":"برچسب فعلی",
    "ToLabel":"برچسب جدید"
}'
```
</div>

---



## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در صورتی که درخواست ارسال‌شده دارای اشکال باشد یا توکن دسترسی معتبر نباشد، وب‌سرویس پاسخ را با **کد وضعیت HTTP = 200 (OK)** برمی‌گرداند اما مقدار خروجی آن یک **رشته متنی** شامل **کد خطا (Error Code)** است.

یعنی ممکن است پاسخ ظاهراً موفق (HTTP 200) باشد، اما در بدنه‌ی پاسخ، محتوای خطا بازگردانده شود.

---

## نمونه خروجی خطا

<div dir="ltr">

```json
"Token_NotExists"
```

یا:

```json
"FromRow_Invalid"
```

</div>

---


## نکات مهم در مدیریت خطاها

1. پاسخ‌ها همیشه با **HTTP 200 OK** برمی‌گردند، بنابراین بررسی خطا باید بر اساس مقدار **بدنه پاسخ (Body)** انجام شود.
2. در پاسخ‌های موفق، بدنه معمولاً شامل رشته‌ای از نوع `Success` یا مقدار JSON با `ReturnValue` معتبر است.
3. در پاسخ‌های خطا، مقدار بازگشتی همیشه به‌صورت **رشته متنی ساده (String)** است.
4. توصیه می‌شود پس از هر فراخوانی متد، مقدار بازگشتی بررسی و در صورت دریافت هر مقدار غیر از `Success`، منطق مناسب مدیریت خطا اجرا شود.


</div>
