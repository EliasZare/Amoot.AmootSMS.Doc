<div dir="rtl">

# مستندات وب سرویس مخاطبین - ویرایش مخاطب (ContactEdit) 

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۹

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/ContactEdit](https://portal.amootsms.com/rest/ContactEdit)

## توضیحات

از طریق این متد می‌توانید با ارسال اطلاعات مخاطب، رکورد مربوطه را در دفترچه تلفن پنل کاربری **ویرایش و ذخیره** کنید. احراز هویت از طریق هدر <span dir="ltr">Authorization</span> انجام می‌شود و بدنهٔ درخواست به‌صورت فرم ارسال می‌گردد.

> نکته: در برخی سناریوها مقدار <span dir="ltr">ContactGroupID</span> استفاده نمی‌شود؛ در این حالت مقدار <span dir="ltr">0</span> ارسال کنید و برای دسته‌بندی از <span dir="ltr">Labels</span> بهره ببرید.

## پارامترهای ورودی

| نام                       | نوع          | الزامی | توضیحات                                                                                       |
| ------------------------- | ------------ | :----: | --------------------------------------------------------------------------------------------- |
| Token                     | String       |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود            |
| ContactID                 | Long         |   بله  | کد یکتای مخاطب جهت ویرایش                                                                     |
| ContactGroupID            | Long         |   بله  | کد گروه مخاطب (درصورت غیرفعال بودن، مقدار ۰)                                                  |
| Active                    | Bool         |   بله  | وضعیت فعال بودن مخاطب                                                                         |
| Mobile                    | String       |   بله  | شمارهٔ همراه                                                                                  |
| FName                     | String       |   بله  | نام                                                                                           |
| LName                     | String       |   بله  | نام‌خانوادگی                                                                                  |
| GenderType                | Bool         |   بله  | <span dir="ltr">False</span>=خانم، <span dir="ltr">True</span>=آقا                            |
| CompanyTitle              | String       |   بله  | عنوان شرکت                                                                                    |
| JobTitle                  | String       |   بله  | سمت/شغل                                                                                       |
| Email                     | String       |   بله  | ایمیل                                                                                         |
| CityName                  | String       |   بله  | نام شهر                                                                                       |
| AddressText               | String       |   بله  | آدرس                                                                                          |
| BornDate                  | DateTime     |   بله  | تاریخ تولد (فرمت: <span dir="ltr">yyyy-MM-dd</span>)                                          |
| AnniversaryDate           | DateTime     |   بله  | تاریخ سالگرد (فرمت: <span dir="ltr">yyyy-MM-dd</span>)                                        |
| CustomText1 … CustomText6 | String       |   بله  | فیلدهای متنی سفارشی ۱ تا ۶                                                                    |
| CustomDate1 … CustomDate3 | DateTime     |   بله  | تاریخ‌های سفارشی ۱ تا ۳ (فرمت: <span dir="ltr">yyyy-MM-dd</span>)                             |
| Labels                    | Array/String |   خیر  | لیست برچسب‌ها؛ در فرم می‌توان با کلیدهای تکرارشونده <span dir="ltr">Labels[]</span> ارسال کرد |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                 |
| ------ | ------ | :----: | ------------------------------------------------------- |
| Status | String |   خیر  | <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا |
| Data   | Object |   خیر  | شیء شامل نتیجهٔ عملیات و اطلاعات مخاطب ویرایش‌شده       |

## نمونه کدها

<div dir=ltr>

### C#

```csharp
string Token = "MyToken";
long ContactID = 0;
long ContactGroupID = 0;
bool Active = true;
string Mobile = "09120000000";
string FName = "";
string LName = "";
bool GenderType = true;
string CompanyTitle = "";
string JobTitle = "";
string Email = "";
string CityName = "";
string AddressText = "";
DateTime? BornDate = null;
DateTime? AnniversaryDate = null;
string CustomText1 = "";
string CustomText2 = "";
string CustomText3 = "";
string CustomText4 = "";
string CustomText5 = "";
string CustomText6 = "";
DateTime? CustomDate1 = null;
DateTime? CustomDate2 = null;
DateTime? CustomDate3 = null;
string[] Labels = new []{"گروه 1","گروه 2"};

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString() },
        { "ContactGroupID", ContactGroupID.ToString() },
        { "Active", Active.ToString() },
        { "Mobile", Mobile },
        { "FName", FName },
        { "LName", LName },
        { "GenderType", GenderType.ToString() },
        { "CompanyTitle", CompanyTitle },
        { "JobTitle", JobTitle },
        { "Email", Email },
        { "CityName", CityName },
        { "AddressText", AddressText },
        { "BornDate", BornDate?.ToString("yyyy-MM-dd") },
        { "AnniversaryDate", AnniversaryDate?.ToString("yyyy-MM-dd") },
        { "CustomText1", CustomText1 },
        { "CustomText2", CustomText2 },
        { "CustomText3", CustomText3 },
        { "CustomText4", CustomText4 },
        { "CustomText5", CustomText5 },
        { "CustomText6", CustomText6 },
        { "CustomDate1", CustomDate1?.ToString("yyyy-MM-dd") },
        { "CustomDate2", CustomDate2?.ToString("yyyy-MM-dd") },
        { "CustomDate3", CustomDate3?.ToString("yyyy-MM-dd") },
        { "Labels", string.Join(",", Labels) }
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactEdit", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests

url = "https://portal.amootsms.com/rest/ContactEdit"
headers = {"Authorization": "MyToken"}

data = {
    "ContactID": 0,
    "ContactGroupID": 0,
    "Active": True,
    "Mobile": "09120000000",
    "FName": "",
    "LName": "",
    "GenderType": True,
    "CompanyTitle": "",
    "JobTitle": "",
    "Email": "",
    "CityName": "",
    "AddressText": "",
    "BornDate": None,  # yyyy-MM-dd
    "AnniversaryDate": None,
    "CustomText1": "",
    "CustomText2": "",
    "CustomText3": "",
    "CustomText4": "",
    "CustomText5": "",
    "CustomText6": "",
    "CustomDate1": None,
    "CustomDate2": None,
    "CustomDate3": None,
    "Labels": ["گروه 1", "گروه 2"],
}

r = requests.post(url, headers=headers, data=data)
print(r.text)
```

### PHP

```php
$Token = "MyToken";
$Labels = ["گروه 1", "گروه 2"];

$data = [
  "ContactID" => 0,
  "ContactGroupID" => 0,
  "Active" => true,
  "Mobile" => "09120000000",
  "FName" => "",
  "LName" => "",
  "GenderType" => true,
  "CompanyTitle" => "",
  "JobTitle" => "",
  "Email" => "",
  "CityName" => "",
  "AddressText" => "",
  "BornDate" => null,
  "AnniversaryDate" => null,
  "CustomText1" => "",
  "CustomText2" => "",
  "CustomText3" => "",
  "CustomText4" => "",
  "CustomText5" => "",
  "CustomText6" => "",
  "CustomDate1" => null,
  "CustomDate2" => null,
  "CustomDate3" => null,
  "Labels" => implode(",", $Labels)
];

$ch = curl_init("https://portal.amootsms.com/rest/ContactEdit");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // JSON
```

### Node.js

```js
const axios = require('axios');
const token = 'MyToken';

const payload = {
  ContactID: 0,
  ContactGroupID: 0,
  Active: true,
  Mobile: '09120000000',
  FName: '',
  LName: '',
  GenderType: true,
  CompanyTitle: '',
  JobTitle: '',
  Email: '',
  CityName: '',
  AddressText: '',
  BornDate: null,
  AnniversaryDate: null,
  CustomText1: '',
  CustomText2: '',
  CustomText3: '',
  CustomText4: '',
  CustomText5: '',
  CustomText6: '',
  CustomDate1: null,
  CustomDate2: null,
  CustomDate3: null,
  Labels: ['گروه 1', 'گروه 2']
};

axios.post('https://portal.amootsms.com/rest/ContactEdit', payload, { headers: { Authorization: token }})
  .then(res => console.log(res.data))
  .catch(err => console.error(err.response ? err.response.data : err.message));
```

### Java

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;

public class ContactEdit {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    String urlString = "https://portal.amootsms.com/rest/ContactEdit";

    String data = String.join("&",
      "ContactID=0",
      "ContactGroupID=0",
      "Active=true",
      "Mobile=09120000000",
      "FName=",
      "LName=",
      "GenderType=true",
      "CompanyTitle=",
      "JobTitle=",
      "Email=",
      "CityName=",
      "AddressText=",
      "BornDate=",
      "AnniversaryDate=",
      "CustomText1=",
      "CustomText2=",
      "CustomText3=",
      "CustomText4=",
      "CustomText5=",
      "CustomText6=",
      "CustomDate1=",
      "CustomDate2=",
      "CustomDate3=",
      "Labels=گروه 1,گروه 2"
    );

    URL url = new URL(urlString);
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);

    try (OutputStream os = conn.getOutputStream()) {
      os.write(data.getBytes(StandardCharsets.UTF_8));
    }

    try (BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
      String line; StringBuilder sb = new StringBuilder();
      while ((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString());
    }
  }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactEdit" \
  -H "Authorization: MyToken" \
  -F "ContactID=0" \
  -F "ContactGroupID=0" \
  -F "Active=true" \
  -F "Mobile=09120000000" \
  -F "FName=" \
  -F "LName=" \
  -F "GenderType=true" \
  -F "CompanyTitle=" \
  -F "JobTitle=" \
  -F "Email=" \
  -F "CityName=" \
  -F "AddressText=" \
  -F "BornDate=" \
  -F "AnniversaryDate=" \
  -F "CustomText1=" \
  -F "CustomText2=" \
  -F "CustomText3=" \
  -F "CustomText4=" \
  -F "CustomText5=" \
  -F "CustomText6=" \
  -F "CustomDate1=" \
  -F "CustomDate2=" \
  -F "CustomDate3=" \
  -F "Labels[]=گروه 1" \
  -F "Labels[]=گروه 2"
```
</div>

### نمونه خروجی موفق

<div dir=ltr>

```json
{
  "Status": "Success",
  "Data": {
        "ID": 123456,
        "Active": true,
        "ContactGroupID": 0,
        "Mobile": 9120000000,
        "FName": "",
        "LName": null,
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
        "UpdateDateTime": "2015-10-21T11:02:41.96"
    }
}
```
</div>

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این الگو، متد در صورت بروز خطا با **HTTP 200 (OK)** پاسخ می‌دهد اما بدنهٔ پاسخ نشان‌دهندهٔ خطاست.

‌ بدنه یک شیء JSON است که در آن فیلد Status برابر **کد خطا** و فیلد Data معمولاً با مقادیر پیش‌فرض/خالی است؛ مثال:

<div dir="ltr">

```json
{
  "Status": "ContactID_Invalid",
  "Data": { /* ... */ }
}
```

</div>

> نتیجه: **HTTP 200** به‌تنهایی نشانهٔ موفقیت نیست؛ همیشه فیلد Status یا متن بدنه را بررسی کنید. اگر مقدار Status = Success باشد، یعنی عملیات به‌درستی انجام شده است و مقدار Data شامل شیء Contact ویرایش‌شده خواهد بود.

</div>