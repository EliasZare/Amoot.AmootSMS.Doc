#  متد محاسبه هزینه پیامک — CalculateMessagePrice


>**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

---

##  آدرس وب‌سرویس
>https://portal.amootsms.com/rest/CalculateMessagePrice


---

##  توضیح متد
با استفاده از این متد می‌توانید هزینه تقریبی ارسال پیامک خود را پیش از ارسال واقعی محاسبه کنید.  
کافی است متن پیامک، شماره خط ارسال و شماره‌های دریافت‌کننده را مشخص نمایید تا سامانه آموت هزینه‌ی کل ارسال را برگرداند.

این متد معمولاً برای نمایش هزینه در فرم‌های ارسال یا برآورد مالی کمپین‌ها استفاده می‌شود.

---

##  پارامترهای ورودی
| نام | نوع | الزامی | توضیحات |
|---|---:|:---:|---|
| Token | String | بله | توکن احراز هویت ثبت‌شده در سامانه پیامک آموت |
| SMSMessageText | String | بله | متن کامل پیامک |
| LineNumber | String | بله | شماره خط ارسال‌کننده |
| Mobiles | Object | بله | آرایه‌ای از شماره‌های موبایل دریافت‌کنندگان پیامک |

---

##  مقدار خروجی
`ReturnValue` — عددی از نوع `Int` که هزینه نهایی ارسال پیامک (برحسب ریال) را نشان می‌دهد.

### مثال خروجی موفق:
```json
{
  "Status": true,
  "Message": "محاسبه با موفقیت انجام شد",
  "ReturnValue": 12345
}
```
## مثال خروجی ناموفق:
```json
{
  "Status": false,
  "Message": "توکن معتبر نیست",
  "ReturnValue": 0
}
```

# نمونه کدها

## C#
```CSHARP
string Token = "MyToken";
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[] { "9120000000", "9150000000" };

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SMSMessageText", SMSMessageText },
        { "LineNumber", LineNumber },
        { "Mobiles", string.Join(",", Mobiles) }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CalculateMessagePrice", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی
    Console.WriteLine(json);
}

```
## PHP 
```PHP
$token = "MyToken";
$data = [
  "SMSMessageText" => "پیامک تستی من",
  "LineNumber" => "public",
  "Mobiles" => "9120000000,9150000000"
];

$ch = curl_init("https://portal.amootsms.com/rest/CalculateMessagePrice");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;

```

## NodeJS
```JS
const axios = require("axios");
const token = "MyToken";

axios.post("https://portal.amootsms.com/rest/CalculateMessagePrice", {
  SMSMessageText: "پیامک تستی من",
  LineNumber: "public",
  Mobiles: ["9120000000", "9150000000"]
}, {
  headers: { Authorization: token }
}).then(res => console.log(res.data))
  .catch(err => console.error(err.response ? err.response.data : err.message));
```
## CURL
```BASH
curl -X POST "https://portal.amootsms.com/rest/CalculateMessagePrice" \
  -H "Authorization: MyToken" \
  -d "SMSMessageText=پیامک تستی من" \
  -d "LineNumber=public" \
  -d "Mobiles=9120000000,9150000000"
```
