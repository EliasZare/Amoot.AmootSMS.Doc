# متد دریافت پیامک‌های جدید دریافتی (RecieveNewMessages)


**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

##  آدرس وب‌سرویس
> https://portal.amootsms.com/rest/RecieveNewMessages

---
از طریق این متد می‌توانید **پیامک‌های جدید دریافتی** خود را از سامانه پیامک آموت دریافت کنید.  
> ⚠️ توجه: در هر بار فراخوانی، فقط یک‌بار پیامک‌های جدید را دریافت می‌کنید (پیامک‌ها پس از دریافت، مجدد بازگردانده نمی‌شوند).



## پارامترهای ورودی

| نام | نوع | الزامی | توضیحات |
|-----|------|---------|----------|
| `Token` | String | بله | توکن ثبت‌شده در سامانه پیامک آموت |
| `Count` | Int | بله | تعداد پیامک‌هایی که قصد دریافت آن‌ها را دارید |

---

## مقدار خروجی

| نام | نوع | توضیحات |
|-----|------|----------|
| `ReturnValue` | Object | شامل مجموعه پیامک‌های جدید دریافت‌شده |

---

##  ساختار خروجی موفق (JSON)

```json
{
  "Status": "Success",
  "Message": "پیامک‌های جدید با موفقیت دریافت شدند",
  "Data": [
    {
      "MessageID": "123456789",
      "Mobile": "09120000000",
      "MessageText": "سلام وقت بخیر",
      "ReceiveDateTime": "2024-11-20T12:19:00"
    }
  ]
}
```
##  ساختار خروجی ناموفق (JSON)
```json
{
  "Status":  "Success",
  "Message": "هیچ پیام جدیدی یافت نشد",
  "Data": []
}
```
# نمونه کدها
## C#

```CSHARP
string Token = "MyToken";
int Count = 10;

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Count", Count.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RecieveNewMessages", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی
}
```

## PHP
```PHP
    <?php
$token = "MyToken";
$count = 10;

$url = "https://portal.amootsms.com/rest/RecieveNewMessages";
$data = ["Count" => $count];

$options = [
    "http" => [
        "header" => "Authorization: $token\r\n",
        "method" => "POST",
        "content" => http_build_query($data),
    ],
];

$context = stream_context_create($options);
$result = file_get_contents($url, false, $context);

echo $result;

```

## Nodejs

```JS
import fetch from "node-fetch";

const token = "MyToken";
const url = "https://portal.amootsms.com/rest/RecieveNewMessages";

const data = new URLSearchParams({ Count: "10" });

const response = await fetch(url, {
  method: "POST",
  headers: {
    "Authorization": token,
    "Content-Type": "application/x-www-form-urlencoded"
  },
  body: data
});

const result = await response.text();
console.log(result);
```

## CURL
```BASH
curl -X POST "https://portal.amootsms.com/rest/RecieveNewMessages" \
     -H "Authorization: MyToken" \
     -d "Count=10"
```