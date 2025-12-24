<div dir="rtl">

# مستندات وب‌سرویس برنامه‌های دوره‌ای - ایجاد ارسال هوشمند دوره‌ای (CourseCreateSimple)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/CourseCreateSimple](https://portal.amootsms.com/rest/CourseCreateSimple)

## توضیح کلی

از طریق این متد می‌توانید یک **برنامهٔ ارسال دوره‌ای هوشمند** ایجاد کنید. در صورت وجود اعتبار کافی در پنل، ارسال‌ها طبق زمان‌بندی به‌صورت خودکار انجام می‌شوند.

> نکته: مقدار <span dir="ltr">CourseType</span> در این متد به‌صورت **رشته‌ای** دریافت می‌شود (مانند <span dir="ltr">Daily</span>). در صورت پشتیبانی، معادل‌های عددی نیز به شکل زیر هستند: <span dir="ltr">Yearly=0, Monthly=1, Weekly=2, Daily=3</span>.

## پارامترهای ورودی

| نام            | نوع      | الزامی | توضیحات                                                                                                                                               |
| -------------- | -------- | :----: | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Token          | String   |   بله  | توکن ثبت‌شده؛ در هدر <span dir="ltr">Authorization</span> ارسال شود                                                                                   |
| CourseGroupID  | Long     |   بله  | کد گروه برنامهٔ ارسال دوره‌ای                                                                                                                         |
| CourseType     | String   |   بله  | نوع برنامهٔ هوشمند: یکی از <span dir="ltr">Yearly</span>، <span dir="ltr">Monthly</span>، <span dir="ltr">Weekly</span>، <span dir="ltr">Daily</span> |
| CourseDateTime | DateTime |   بله  | زمان شروع برنامه (فرمت پیشنهادی: <span dir="ltr">yyyy-MM-dd HH:mm</span>)                                                                             |
| Title          | String   |   بله  | عنوان برنامه                                                                                                                                          |
| Mobile         | String   |   بله  | شمارهٔ موبایل مقصد                                                                                                                                    |
| SMSMessageText | String   |   بله  | متن پیامک                                                                                                                                             |
| LineNumber     | String   |   بله  | شمارهٔ خط ارسال                                                                                                                                       |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                           |
| ------ | ------ | :----: | ----------------------------------------------------------------- |
| Status | String |   خیر  | مقدار <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا     |
| Data   | Object |   خیر  | شیء نتیجهٔ ایجاد برنامه (در صورت موفقیت شامل شناسه/جزئیات برنامه) |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": {
    "CourseID": 987654,
    "Title": "کمپین Daily",
    "CourseType": "Daily",
    "StartAt": "2025-10-28 09:00",
    "LineNumber": "public"
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Token_Invalid",
  "Data": null
}
```

</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در این الگو، پاسخ HTTP در خطا نیز **200 (OK)** است اما بدنهٔ پاسخ وضعیت را مشخص می‌کند.
**موفق** وقتی است که <span dir="ltr">Status = "Success"</span> باشد؛ در غیر این صورت مقدار <span dir="ltr">Status</span> کد خطا است و معمولاً <span dir="ltr">Data</span> تهی خواهد بود.

---

## نمونه کدها

<div dir=ltr>

### C#

```csharp
string Token = "MyToken";
long CourseGroupID = 0;
string CourseType = "Daily"; // Yearly | Monthly | Weekly | Daily
DateTime CourseDateTime = DateTime.Now.Date.AddHours(9);
string Title = "کمپین روزانه";
string Mobile = "9120000000";
string SMSMessageText = "متن پیامک تستی";
string LineNumber = "public";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "CourseGroupID", CourseGroupID.ToString() },
        { "CourseType", CourseType },
        { "CourseDateTime", CourseDateTime.ToString("yyyy-MM-dd HH:mm") },
        { "Title", Title },
        { "Mobile", Mobile },
        { "SMSMessageText", SMSMessageText },
        { "LineNumber", LineNumber }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CourseCreateSimple", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests, datetime
url = "https://portal.amootsms.com/rest/CourseCreateSimple"
headers = {"Authorization": "MyToken"}
payload = {
  "CourseGroupID": 0,
  "CourseType": "Daily",
  "CourseDateTime": datetime.datetime.now().strftime('%Y-%m-%d %H:%M'),
  "Title": "کمپین روزانه",
  "Mobile": "9120000000",
  "SMSMessageText": "متن پیامک تستی",
  "LineNumber": "public"
}
res = requests.post(url, headers=headers, data=payload)
print(res.text)
```

### PHP

```php
$Token = "MyToken";
$data = [
  'CourseGroupID' => 0,
  'CourseType' => 'Daily',
  'CourseDateTime' => date('Y-m-d H:i'),
  'Title' => 'کمپین روزانه',
  'Mobile' => '9120000000',
  'SMSMessageText' => 'متن پیامک تستی',
  'LineNumber' => 'public'
];
$ch = curl_init('https://portal.amootsms.com/rest/CourseCreateSimple');
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
axios.post('https://portal.amootsms.com/rest/CourseCreateSimple', {
  CourseGroupID: 0,
  CourseType: 'Daily',
  CourseDateTime: '2025-10-28 09:00',
  Title: 'کمپین روزانه',
  Mobile: '9120000000',
  SMSMessageText: 'متن پیامک تستی',
  LineNumber: 'public'
}, { headers: { Authorization: 'MyToken' }})
.then(r => console.log(r.data))
.catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
String token = "MyToken";
String data = String.join("&",
  "CourseGroupID=0",
  "CourseType=" + URLEncoder.encode("Daily", "UTF-8"),
  "CourseDateTime=" + URLEncoder.encode("2025-10-28 09:00", "UTF-8"),
  "Title=" + URLEncoder.encode("کمپین روزانه", "UTF-8"),
  "Mobile=9120000000",
  "SMSMessageText=" + URLEncoder.encode("متن پیامک تستی", "UTF-8"),
  "LineNumber=public"
);
URL url = new URL("https://portal.amootsms.com/rest/CourseCreateSimple");
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
curl -X POST "https://portal.amootsms.com/rest/CourseCreateSimple" \
  -H "Authorization: MyToken" \
  -d "CourseGroupID=0" \
  -d "CourseType=Daily" \
  -d "CourseDateTime=2025-10-28 09:00" \
  -d "Title=کمپین روزانه" \
  -d "Mobile=9120000000" \
  -d "SMSMessageText=متن پیامک تستی" \
  -d "LineNumber=public"
```

</div>
</div>
