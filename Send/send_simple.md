<div dir="rtl">

#  مستندات وب‌سرویس ارسال - ارسال پیامک ساده (SendSimple)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/SendSimple](https://portal.amootsms.com/rest/SendSimple)

## توضیحات

این متد برای **ارسال پیامک متنی ساده** به یک یا چند شماره استفاده می‌شود. در صورت نیاز می‌توانید زمان ارسال را تعیین کرده یا از خط پیش‌فرض استفاده نمایید. اگر زمان مشخص نشود، ارسال بلافاصله انجام خواهد شد.

## پارامترهای ورودی

| نام            | نوع    | الزامی | توضیحات                                                                    |
| -------------- | ------ | :----: | -------------------------------------------------------------------------- |
| Token          | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود                |
| SendDateTime   | String |   بله  | تاریخ و زمان ارسال پیامک (فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span>) |
| SMSMessageText | String |   بله  | متن پیامک موردنظر                                                          |
| LineNumber     | String |   بله  | شماره خط ارسال یا مقدار `public` برای خط عمومی                             |
| Mobiles        | Array  |   بله  | آرایه‌ای از شماره‌های گیرندگان پیامک                                       |

## مقدار خروجی

| نام           | نوع    | الزامی | توضیحات                                        |
| ------------- | ------ | :----: | ---------------------------------------------- |
| MessageText   | String |   خیر  | متن نهایی پیامک ارسالی                         |
| SMSPagesCount | Int    |   خیر  | تعداد صفحات پیامک                              |
| CampaignID    | Int    |   خیر  | شناسه کمپین ایجادشده برای ارسال                |
| Price         | Int    |   خیر  | هزینه کل پیامک‌ها (برحسب ریال)                 |
| Status        | String |   خیر  | وضعیت نهایی عملیات (Success یا کد خطا)         |
| Data          | Array  |   خیر  | شامل نتایج ارسال برای هر شماره به‌صورت جداگانه |


## نمونه کدها

### C#

```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[] { "9120000000", "9150000000" };

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString("yyyy-MM-dd HH:mm:ss") },
        { "SMSMessageText", SMSMessageText },
        { "LineNumber", LineNumber },
        { "Mobiles", string.Join(",", Mobiles) }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendSimple", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json);
}
```

### Python

```python
import requests

token = "MyToken"
url = "https://portal.amootsms.com/rest/SendSimple"
data = {
    "SendDateTime": "2025-10-21 14:25:00",
    "SMSMessageText": "پیام تستی من",
    "LineNumber": "public",
    "Mobiles": ["9120000000", "9150000000"]
}
headers = {"Authorization": token}
response = requests.post(url, json=data, headers=headers)
print(response.json())
```

### PHP

```php
$token = "MyToken";
$data = [
    "SendDateTime" => date("Y-m-d H:i:s"),
    "SMSMessageText" => "پیامک تستی من",
    "LineNumber" => "public",
    "Mobiles" => ["9120000000", "9150000000"]
];

$ch = curl_init("https://portal.amootsms.com/rest/SendSimple");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```js
const axios = require("axios");

const token = "MyToken";
axios.post("https://portal.amootsms.com/rest/SendSimple", {
  SendDateTime: new Date(),
  SMSMessageText: "پیامک تستی من",
  LineNumber: "public",
  Mobiles: ["9120000000", "9150000000"]
}, {
  headers: { Authorization: token }
}).then(res => console.log(res.data))
  .catch(err => console.error(err));
```

### Java

```java
import java.io.*;
import java.net.*;

public class SendSimpleExample {
    public static void main(String[] args) throws Exception {
        String token = "MyToken";
        URL url = new URL("https://portal.amootsms.com/rest/SendSimple");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Authorization", token);
        conn.setRequestProperty("Content-Type", "application/json; utf-8");
        conn.setDoOutput(true);

        String jsonInputString = "{" +
            "\"SendDateTime\": \"2025-10-21 14:25:00\"," +
            "\"SMSMessageText\": \"پیامک تستی من\"," +
            "\"LineNumber\": \"public\"," +
            "\"Mobiles\": [\"9120000000\", \"9150000000\"]" +
        "}";

        try(OutputStream os = conn.getOutputStream()) {
            byte[] input = jsonInputString.getBytes("utf-8");
            os.write(input, 0, input.length);
        }

        BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"));
        StringBuilder response = new StringBuilder();
        String responseLine;
        while ((responseLine = br.readLine()) != null) {
            response.append(responseLine.trim());
        }
        System.out.println(response.toString());
    }
}
```

### cURL

```bash
curl -X POST https://portal.amootsms.com/rest/SendSimple \
-H "Authorization: MyToken" \
-H "Content-Type: application/json" \
-d '{
  "SendDateTime": "2025-10-21 14:25:00",
  "SMSMessageText": "پیامک تستی من",
  "LineNumber": "public",
  "Mobiles": ["9120000000", "9150000000"]
}'
```


### نمونه خروجی موفق

```json
{
  "MessageText": "نمونه پیام تستی",
  "SMSPagesCount": 1,
  "CampaignID": 12345678,
  "Price": 1234,
  "Status": "Success",
  "Data": [
    {
      "Mobile": 9120000000,
      "MessageID": 1234568,
      "Status": "Success"
    },
    {
      "Mobile": 9150000000,
      "MessageID": 1234567,
      "Status": "Success"
    }
  ]
}
```

### نمونه خروجی ناموفق

```json
{
  "MessageText": null,
  "SMSPagesCount": 1,
  "CampaignID": 0,
  "Price": 0,
  "Status": "SMSMessageText_Empty",
  "Data": []
}
```

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در صورت بروز خطا، پاسخ وب‌سرویس همچنان با **HTTP 200 (OK)** برمی‌گردد ولی فیلد <span dir="ltr">Status</span> مقدار خطا را مشخص می‌کند.


> نکته: **HTTP 200** نشان‌دهنده موفقیت درخواست از نظر شبکه است، نه لزوماً موفقیت عملیات ارسال پیامک.

</div>
