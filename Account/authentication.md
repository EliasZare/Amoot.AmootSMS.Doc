<div dir="rtl">

#  مستندات وب سرویس - احراز هویت (Authentication)

**تاریخ بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

**آدرس پایه (Base URL):** <span dir="ltr">[https://portal.amootsms.com/rest](https://portal.amootsms.com/rest)</span>

---

## توضیح کلی

تمامی متدهای وب‌سرویس <span dir="ltr">Amoot SMS</span> نیاز به احراز هویت دارند تا از دسترسی غیرمجاز جلوگیری شود.

احراز هویت از طریق **توکن دسترسی (API Token)** انجام می‌شود که در هنگام ثبت‌نام در پنل پیامک دریافت می‌گردد.

توکن جایگزین نام کاربری و رمز عبور است و باید در تمامی درخواست‌ها از طریق **هدر HTTP** ارسال شود.

---

## روش احراز هویت

### ۱. ارسال توکن در هدر Authorization

تمام درخواست‌های API باید شامل هدر زیر باشند:

<div dir="ltr">

```
Authorization: <YourToken>
```

</div>

**مثال:**
`Authorization: MyToken12345`

---

### ۲. شرایط اعتبار توکن

* توکن باید **فعال و معتبر** باشد.
* در صورت **منقضی شدن** یا **غیرفعال شدن**، پاسخ API شامل خطا خواهد بود.
* توکن‌ها ممکن است به **کاربر خاص** یا **آی‌پی مشخص** محدود شوند (در صورت تنظیم در پنل).
* می‌توان برای هر توکن **تاریخ انقضا** و **لیست متدهای مجاز** را تعیین کرد.

> ⚠️ پیشنهاد می‌شود توکن‌ها را در سمت سرور نگهداری و از ارسال مستقیم از سمت کلاینت خودداری کنید.

---

## محدودیت‌ها و تنظیمات توکن

در پنل پیامک آموت، هنگام ایجاد یا ویرایش توکن، می‌توان تنظیمات زیر را اعمال کرد:

| ویژگی                             | توضیح                                                                                                                                     |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **فعال (Active)**                 | در صورت فعال بودن، توکن قابل استفاده است. در غیر این صورت، درخواست‌ها با خطای `Token_Disabled` مواجه می‌شوند.                             |
| **عنوان (Title)**                 | توضیح یا نام دلخواه برای تشخیص راحت‌تر توکن.                                                                                              |
| **تاریخ انقضا (Expire Date)**     | تاریخ و زمان پایان اعتبار توکن. پس از رسیدن به این تاریخ، پاسخ شامل خطای `Token_Expired` خواهد بود.                                       |
| **آی‌پی‌های مجاز (Allowed IPs)**  | فهرستی از آی‌پی‌هایی که مجاز به استفاده از توکن هستند. در صورت ارسال درخواست از آی‌پی غیرمجاز، خطای `Token_NotAllowIP` بازگردانده می‌شود. |
| **متدهای مجاز (Allowed Methods)** | امکان تعیین متدهایی از API که مجاز به استفاده با این توکن هستند. در صورت فراخوانی متد غیرمجاز، پاسخ `Token_NotAllowMethod` خواهد بود.     |

---

## نحوه عملکرد محدودیت‌ها

1. در هر درخواست، سرور ابتدا توکن را از هدر دریافت و بررسی می‌کند.
2. اگر توکن فعال نباشد یا منقضی شده باشد، پاسخ با کد خطا بازمی‌گردد.
3. در نهایت، متد فراخوانی‌شده با لیست مجاز توکن تطبیق داده می‌شود.
4. در صورتی که تمام موارد معتبر باشند، درخواست اجرا می‌شود؛ در غیر این صورت، پاسخ خطای مناسب (مانند `Token_NotAllowIP` یا `Token_NotAllowMethod`) بازمی‌گردد.

---

## نمونه وضعیت‌ها و خطاهای مرتبط با توکن

| وضعیت (Status)       | توضیح                                                    |
| -------------------- | -------------------------------------------------------- |
| Token_Expired        | توکن منقضی شده و باید توکن جدید ایجاد شود.               |
| Token_Disabled       | توکن توسط کاربر یا مدیر غیرفعال شده است.                 |
| Token_NotAllowIP     | آی‌پی ارسال‌کننده در فهرست مجاز نیست.                    |
| Token_NotAllowMethod | متد در تنظیمات مجاز توکن وجود ندارد.                     |
| Token_Invalid        | مقدار ارسال‌شده در هدر Authorization اشتباه یا ناقص است. |
| Token_NotExists      | توکن یافت نشد یا حذف شده است.                            |

> در تمامی خطاهای فوق، مقدار `Status` حاوی نوع خطا بوده و معمولاً فیلد `ReturnValue` خالی است.

---

## نمونه درخواست با احراز هویت

<div dir="ltr">

### cURL

```bash
curl -H "Authorization: MyToken12345" \
     https://portal.amootsms.com/rest/SomeMethod
```

### C#

```csharp
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, "MyToken12345");
    var response = client.DownloadString("https://portal.amootsms.com/rest/SomeMethod");
    Console.WriteLine(response);
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/SomeMethod"
headers = {"Authorization": "MyToken12345"}

response = requests.get(url, headers=headers)
print(response.text)
```

### PHP

```php
<?php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "https://portal.amootsms.com/rest/SomeMethod");
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    "Authorization: MyToken12345"
));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
?>
```

### Node.js

```js
const axios = require('axios');

axios.get('https://portal.amootsms.com/rest/SomeMethod', {
    headers: { 'Authorization': 'MyToken12345' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```

### Java

```java
import java.io.*;
import java.net.*;

HttpURLConnection conn = (HttpURLConnection) new URL("https://portal.amootsms.com/rest/SomeMethod").openConnection();
conn.setRequestProperty("Authorization", "MyToken12345");
BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
StringBuilder response = new StringBuilder();
String line;
while ((line = in.readLine()) != null) response.append(line);
in.close();
System.out.println(response);
```

</div>

---

## نکات امنیتی پیشنهادی

* برای هر سیستم یا پروژه، **توکن اختصاصی** ایجاد کنید.
* از توکن‌هایی با دسترسی حداقلی استفاده کنید (تنها متدهای مورد نیاز).
* در بازه‌های زمانی مشخص، توکن‌ها را بازبینی و در صورت نیاز غیرفعال کنید.
* آی‌پی‌های مجاز را تا حد ممکن محدود نمایید.
* هرگز توکن‌ها را در کدهای سمت کاربر (JavaScript یا اپ موبایل) نگهداری نکنید.


</div>
