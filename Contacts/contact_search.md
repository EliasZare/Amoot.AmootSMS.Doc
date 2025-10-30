<div dir="rtl">

# مستندات وب‌سرویس مخاطبین - جستجوی مخاطبین (ContactSearch)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> <span dir="ltr">[https://portal.amootsms.com/rest/ContactSearch](https://portal.amootsms.com/rest/ContactSearch)</span>

## توضیح کلی

این متد امکان جستجوی مخاطبین را بر اساس فیلدهای مختلف (شماره موبایل، لیبل‌ها، نام/نام‌خانوادگی، عناوین سازمانی، ایمیل، شهر، آدرس، تاریخ تولد/سالگرد، فیلدهای سفارشی متنی و تاریخی) فراهم می‌کند و فهرستی از مخاطبین منطبق را بازمی‌گرداند.

## پارامترهای ورودی

| نام             | نوع         | الزامی | توضیحات                                                                                                                               |                                                                                                                 |
| --------------- | ----------- | :----: | ------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Token           | String      |   بله  | توکن احراز هویت؛ در هدر Authorization ارسال شود: <span dir="ltr">Authorization: {YourToken}</span>                                    |                                                                                                                 |
| Mobile          | String      |   خیر  | شماره موبایل مخاطب. در سمت سرور نرمال‌سازی می‌شود (افزودن پیش‌شماره پیش‌فرض و …) و به‌صورت «برابری دقیق» بررسی می‌گردد.               |                                                                                                                 |
| Labels          | String      |   خیر  | لیست عنوانِ لیبل‌ها به‌صورت رشته با جداکننده‌های مجاز (مانند <span dir="ltr">,</span> یا <span dir="ltr">;</span> یا <span dir="ltr"> | </span>)؛ در سرور به آرایه تفکیک می‌شود و هر مخاطبی که حداقل یکی از این لیبل‌ها را داشته باشد برگردانده می‌شود. |
| FName           | String      |   خیر  | نام؛ تطابق «برابری دقیق».                                                                                                             |                                                                                                                 |
| LName           | String      |   خیر  | نام‌خانوادگی؛ تطابق «برابری دقیق».                                                                                                    |                                                                                                                 |
| CompanyTitle    | String      |   خیر  | عنوان شرکت؛ تطابق «برابری دقیق».                                                                                                      |                                                                                                                 |
| JobTitle        | String      |   خیر  | عنوان شغلی؛ تطابق «برابری دقیق».                                                                                                      |                                                                                                                 |
| Email           | String      |   خیر  | ایمیل؛ تطابق «برابری دقیق».                                                                                                           |                                                                                                                 |
| CityName        | String      |   خیر  | نام شهر؛ تطابق «برابری دقیق».                                                                                                         |                                                                                                                 |
| AddressText     | String      |   خیر  | آدرس؛ تطابق «برابری دقیق».                                                                                                            |                                                                                                                 |
| BornDate        | String/Date |   خیر  | تاریخ تولد؛ قابل ارسال به‌صورت میلادی <span dir="ltr">yyyy-MM-dd</span> یا شمسی (تبدیل به میلادی در سرور). تطابق «برابری دقیق تاریخ». |                                                                                                                 |
| AnniversaryDate | String/Date |   خیر  | تاریخ سالگرد؛ همان قواعد BornDate.                                                                                                    |                                                                                                                 |
| CustomText1     | String      |   خیر  | فیلد سفارشی متنی ۱؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomText2     | String      |   خیر  | فیلد سفارشی متنی ۲؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomText3     | String      |   خیر  | فیلد سفارشی متنی ۳؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomText4     | String      |   خیر  | فیلد سفارشی متنی ۴؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomText5     | String      |   خیر  | فیلد سفارشی متنی ۵؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomText6     | String      |   خیر  | فیلد سفارشی متنی ۶؛ تطابق «برابری دقیق».                                                                                              |                                                                                                                 |
| CustomDate1     | String/Date |   خیر  | فیلد سفارشی تاریخی ۱؛ همان قواعد تاریخ.                                                                                               |                                                                                                                 |
| CustomDate2     | String/Date |   خیر  | فیلد سفارشی تاریخی ۲؛ همان قواعد تاریخ.                                                                                               |                                                                                                                 |
| CustomDate3     | String/Date |   خیر  | فیلد سفارشی تاریخی ۳؛ همان قواعد تاریخ.                                                                                               |                                                                                                                 |

## مقدار خروجی

| نام    | نوع          | الزامی | توضیحات                                                                                                                                                                |
| ------ | ------------ | :----: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status | String       |   خیر  | در موفقیت مقدار <span dir="ltr">Success</span>؛ در غیر این‌ صورت یکی از کدهای خطا (مانند <span dir="ltr">Token_Invalid</span>، <span dir="ltr">ServerError</span> و …) |
| Data   | Object/Array |   خیر  | در موفقیت: آرایه‌ای از مخاطبین منطبق؛ در ناموفق: <span dir="ltr">null</span>/<span dir="ltr">[]</span>                                                                 |

### ساختار هر آیتم در <span dir="ltr">Data</span>

| نام فیلد             | نوع          | توضیحات                                 |
| -------------------- | ------------ | --------------------------------------- |
| ID                   | Long         | شناسه یکتا مخاطب                        |
| Active               | Boolean      | وضعیت فعال/غیرفعال بودن مخاطب           |
| ContactGroupID       | Long         | شناسه گروه (در صورت نیاز/نمایش)         |
| Mobile               | Long         | شماره موبایل (نرمال‌سازی‌شده در سامانه) |
| FName                | String       | نام                                     |
| LName                | String       | نام‌خانوادگی                            |
| GenderType           | Boolean/Null | مقدار بولی جنسیت (در صورت استفاده)      |
| GenderTypeStr        | String       | متن جنسیت ("آقا"/"خانم" یا خالی)        |
| CompanyTitle         | String/Null  | عنوان شرکت                              |
| JobTitle             | String/Null  | عنوان شغلی                              |
| Email                | String/Null  | ایمیل                                   |
| CityName             | String/Null  | نام شهر                                 |
| AddressText          | String/Null  | آدرس                                    |
| BornDate             | Date/Null    | تاریخ تولد (میلادی)                     |
| BornDatePer          | String       | تاریخ تولد به‌صورت شمسی (رشته‌ای)       |
| AnniversaryDate      | Date/Null    | تاریخ سالگرد (میلادی)                   |
| AnniversaryDatePer   | String       | تاریخ سالگرد به‌صورت شمسی (رشته‌ای)     |
| CustomText1..6       | String/Null  | فیلدهای سفارشی متنی ۱ تا ۶              |
| CustomDate1..3       | Date/Null    | فیلدهای سفارشی تاریخی ۱ تا ۳ (میلادی)   |
| CustomDate1Per..3Per | String       | تاریخ‌های سفارشی به شمسی (رشته‌ای)      |
| Deleted              | Boolean      | حذف منطقی                               |
| UpdateDateTime       | DateTime     | تاریخ/زمان آخرین بروزرسانی              |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
    "Status": "Success",
    "Data": [
        {
            "ID": 12345678,
            "Active": true,
            "ContactGroupID": 0,
            "Mobile": 989029999999,
            "FName": "عباس",
            "LName": "قبصثب",
            "GenderType": true,
            "GenderTypeStr": "آقا",
            "CompanyTitle": null,
            "JobTitle": null,
            "Email": null,
            "CityName": null,
            "AddressText": null,
            "BornDate": null,
            "BornDatePer": "",
            "AnniversaryDate": null,
            "AnniversaryDatePer": "",
            "CustomText1": null,
            "CustomText2": null,
            "CustomText3": null,
            "CustomText4": null,
            "CustomText5": null,
            "CustomText6": null,
            "CustomDate1": null,
            "CustomDate1Per": "",
            "CustomDate2": null,
            "CustomDate2Per": "",
            "CustomDate3": null,
            "CustomDate3Per": "",
            "Deleted": false,
            "UpdateDateTime": "2025-10-20T13:11:03.35"
        }
    ]
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
    "Status": "Token_Invalid",
    "Data": []
}
```

</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا (الگوی نوع A)

در این الگو، سرویس حتی در صورت خطا نیز با **HTTP 200 (OK)** پاسخ می‌دهد، اما نتیجه در بدنهٔ پاسخ مشخص می‌شود:

* اگر مقدار <span dir="ltr">Status = "Success"</span> باشد، عملیات موفق است و <span dir="ltr">Data</span> شامل دادهٔ معتبر خواهد بود.
* در غیر این صورت، <span dir="ltr">Status</span> یکی از کدهای خطاست و <span dir="ltr">Data</span> معمولاً تهی/مقدار پیش‌فرض است.

> نکته: همواره فیلد <span dir="ltr">Status</span> را بررسی کنید؛ **HTTP 200** به‌تنهایی نشانهٔ موفقیت نیست.

## نمونه کدها

### C#

```csharp
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using System.Collections.Generic;

var url = "https://portal.amootsms.com/rest/ContactSearch";
using (var http = new HttpClient())
{
    http.DefaultRequestHeaders.Add("Authorization", "{YourToken}");
    var form = new FormUrlEncodedContent(new[]
    {
        new KeyValuePair<string,string>("Mobile","09121112233"),
        new KeyValuePair<string,string>("Labels","VIP|Important"),
        new KeyValuePair<string,string>("FName","علی"),
        // سایر فیلدهای اختیاری…
    });
    var res = await http.PostAsync(url, form);
    var body = await res.Content.ReadAsStringAsync();
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/ContactSearch"
headers = {"Authorization": "{YourToken}"}
data = {
    "Mobile": "09121112233",
    "Labels": "VIP|Important",
    "FName": "علی",
}
r = requests.post(url, headers=headers, data=data)
print(r.text)
```

### PHP

```php
<?php
$ch = curl_init("https://portal.amootsms.com/rest/ContactSearch");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_POSTFIELDS => http_build_query([
    "Mobile" => "09121112233",
    "Labels" => "VIP|Important",
    "FName" => "علی"
  ]),
  CURLOPT_HTTPHEADER => ["Authorization: {YourToken}"],
  CURLOPT_RETURNTRANSFER => true
]);
$resp = curl_exec($ch);
curl_close($ch);
echo $resp;
```

### Node.js

```js
const axios = require("axios");

(async () => {
  const url = "https://portal.amootsms.com/rest/ContactSearch";
  const res = await axios.post(
    url,
    {
      Mobile: "09121112233",
      Labels: "VIP|Important",
      FName: "علی"
    },
    { headers: { Authorization: "{YourToken}" } }
  );
  console.log(res.data);
})();
```

### Java

```java
import java.net.*;
import java.io.*;
import java.nio.charset.StandardCharsets;

String url = "https://portal.amootsms.com/rest/ContactSearch";
String data = "Mobile=09121112233&Labels=VIP|Important&FName=%D8%B9%D9%84%DB%8C";

HttpURLConnection conn = (HttpURLConnection) new URL(url).openConnection();
conn.setRequestMethod("POST");
conn.setDoOutput(true);
conn.setRequestProperty("Authorization", "{YourToken}");
conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded; charset=UTF-8");

try (OutputStream os = conn.getOutputStream()) {
    os.write(data.getBytes(StandardCharsets.UTF_8));
}
try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
    String line; StringBuilder sb = new StringBuilder();
    while ((line = br.readLine()) != null) sb.append(line);
    System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactSearch" \
  -H "Authorization: {YourToken}" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  --data-urlencode "Mobile=09121112233" \
  --data-urlencode "Labels=VIP" \
  --data-urlencode "FName=علی"
```

</div>
