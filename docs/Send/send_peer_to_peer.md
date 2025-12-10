<div dir="rtl">

# مستندات وب‌سرویس پیامک - ارسال پیامک نظیر به نظیر (SendPeerToPeer)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/SendPeerToPeer](https://portal.amootsms.com/rest/SendPeerToPeer)

## توضیح کلی

این متد برای ارسال پیامک‌های **متفاوت به چند شماره به‌صورت همزمان** استفاده می‌شود. هر شماره متن اختصاصی خود را دریافت می‌کند. این روش برای پیام‌های شخصی‌سازی‌شده مانند نام مخاطب، کد تخفیف، یا پیام‌های انبوه سفارشی مناسب است.

## پارامترهای ورودی

| نام          | نوع    | الزامی | توضیحات                                                                                                             |
| ------------ | ------ | :----: | ------------------------------------------------------------------------------------------------------------------- |
| Token        | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود                                                         |
| SendDateTime | String |   بله  | تاریخ و زمان ارسال پیامک (فرمت <span dir="ltr">yyyy-MM-dd HH:mm:ss</span>)                                          |
| LineNumber   | String |   بله  | شماره خط ارسال یا مقدار `public` برای خط عمومی                                                                      |
| Input        | Array  |   بله  | آرایه‌ای از آبجکت‌ها شامل فیلدهای زیر:<br>• `Number` (شماره موبایل)<br>• `MessageText` (متن پیامک مخصوص همان شماره) |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                |
| ------ | ------ | :----: | ------------------------------------------------------ |
| Data   | Object |   خیر  | شامل اطلاعات کمپین ایجادشده و نتایج ارسال برای هر پیام |
| Status | String |   خیر  | وضعیت عملیات (Success یا یکی از کدهای خطا)             |

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string LineNumber = "public";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    client.Headers.Add(System.Net.HttpRequestHeader.ContentType, "application/json; charset=utf-8");
    client.Encoding = System.Text.Encoding.UTF8;

    var data = new
    {
        SendDateTime = SendDateTime.ToString("yyyy-MM-dd HH:mm:ss"),
        LineNumber = LineNumber,
        Input = new object[]
        {
            new { Number = "09150000000", MessageText = "پیامک شماره 1" },
            new { Number = "09120000000", MessageText = "پیامک شماره 2" }
        }
    };

    string jsonInput = Newtonsoft.Json.JsonConvert.SerializeObject(data);
    string json = client.UploadString("https://portal.amootsms.com/rest/SendPeerToPeer", jsonInput);
    Console.WriteLine(json);
}
```

### Python

```python
import requests

token = "MyToken"
url = "https://portal.amootsms.com/rest/SendPeerToPeer"
data = {
    "SendDateTime": "2025-10-21 15:10:00",
    "LineNumber": "public",
    "Input": [
        {"Number": "09150000000", "MessageText": "پیامک شماره 1"},
        {"Number": "09120000000", "MessageText": "پیامک شماره 2"}
    ]
}
headers = {"Authorization": token, "Content-Type": "application/json"}
response = requests.post(url, json=data, headers=headers)
print(response.json())
```

### PHP

```php
$token = "MyToken";
$data = [
    "SendDateTime" => date("Y-m-d H:i:s"),
    "LineNumber" => "public",
    "Input" => [
        ["Number" => "09150000000", "MessageText" => "پیامک شماره 1"],
        ["Number" => "09120000000", "MessageText" => "پیامک شماره 2"]
    ]
];

$ch = curl_init("https://portal.amootsms.com/rest/SendPeerToPeer");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token", "Content-Type: application/json"]);
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
axios.post("https://portal.amootsms.com/rest/SendPeerToPeer", {
  SendDateTime: new Date().toISOString(),
  LineNumber: "public",
  Input: [
    { Number: "09150000000", MessageText: "پیامک شماره 1" },
    { Number: "09120000000", MessageText: "پیامک شماره 2" }
  ]
}, {
  headers: { Authorization: token, "Content-Type": "application/json" }
}).then(res => console.log(res.data))
  .catch(err => console.error(err.response.data));
```

### Java

```java
import java.io.*;
import java.net.*;

public class SendPeerToPeerExample {
    public static void main(String[] args) throws Exception {
        String token = "MyToken";
        URL url = new URL("https://portal.amootsms.com/rest/SendPeerToPeer");
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("POST");
        conn.setRequestProperty("Authorization", token);
        conn.setRequestProperty("Content-Type", "application/json; utf-8");
        conn.setDoOutput(true);

        String jsonInputString = "{" +
            "\"SendDateTime\": \"2025-10-21 15:10:00\"," +
            "\"LineNumber\": \"public\"," +
            "\"Input\": [{\"Number\": \"09150000000\", \"MessageText\": \"پیامک شماره 1\"}, {\"Number\": \"09120000000\", \"MessageText\": \"پیامک شماره 2\"}]" +
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
curl -X POST https://portal.amootsms.com/rest/SendPeerToPeer \
-H "Authorization: MyToken" \
-H "Content-Type: application/json" \
-d '{
  "SendDateTime": "2025-10-21 15:10:00",
  "LineNumber": "public",
  "Input": [
    {"Number": "09150000000", "MessageText": "پیامک شماره 1"},
    {"Number": "09120000000", "MessageText": "پیامک شماره 2"}
  ]
}'
```


### نمونه خروجی موفق

```json
{
  "Data": {
    "CampaignsCreated": 1,
    "TotalCampaignNumbersCreated": 1,
    "FailedCount": 0,
    "Campaigns": [
      {
        "Numbers": [
          {
            "ID": 12345678,
            "Number": 09120000000
          }
        ],
        "Status": "Success",
        "MessageText": "پیام نمونه تستی شماره 1",
        "ID": 12345678,
        "SMSPagesCount": 1
      }
    ]
  },
  "Status": "Success"
}
```

### نمونه خروجی ناموفق

```json
{
  "Data": null,
  "Status": "Token_NotExists"
}
```


## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در صورت بروز خطا، پاسخ وب‌سرویس با **HTTP 200 (OK)** برمی‌گردد، اما مقدار فیلد <span dir="ltr">Status</span> نشان‌دهنده نوع خطا است.


> نکته: مقدار `Token_NotExists` نشان‌دهنده نادرست بودن یا عدم وجود توکن ارسالی است.


</div>
