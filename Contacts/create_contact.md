<div dir="rtl">

# مستندات وب سرویس مخاطبین - ایجاد مخاطب (ContactCreate)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

---

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/ContactCreate](https://portal.amootsms.com/rest/ContactCreate)

---

## توضیحات

این متد برای ایجاد مخاطب جدید در دفترچه تلفن پنل کاربری استفاده می‌شود.

> نکته: پارامتر `ContactGroupID` غیرفعال است. مقدار `0` ارسال شود و برای گروه‌بندی از پارامتر `Labels` استفاده کنید.

---

## پارامترهای ورودی

| نام             | نوع      | الزامی | توضیحات                                                                                  |
| --------------- | -------- | :----: | ---------------------------------------------------------------------------------------- |
| Token           | String   |    بله   | توکن ثبت‌شده در سامانه پیامک آموت                                                        |
| ContactGroupID  | Long     |    بله   | کد گروه مخاطب (غیرفعال است، ۰ قرار دهید). برای برچسب‌ها از پارامتر `Labels` استفاده کنید |
| Active          | Bool     |    بله   | فعال بودن مخاطب                                                                          |
| Mobile          | String   |    بله   | شماره همراه مخاطب                                                                        |
| FName           | String   |    بله   | نام                                                                                      |
| LName           | String   |    بله   | نام خانوادگی                                                                             |
| GenderType      | Bool     |    بله   | `False` = خانم، `True` = آقا                                                             |
| CompanyTitle    | String   |    بله   | عنوان شرکت                                                                               |
| JobTitle        | String   |    بله   | عنوان شغل                                                                                |
| Email           | String   |    بله   | ایمیل مخاطب                                                                              |
| CityName        | String   |    بله   | نام شهر                                                                                  |
| AddressText     | String   |    بله   | آدرس                                                                                     |
| BornDate        | DateTime |    بله   | تاریخ تولد (فرمت: yyyy-MM-dd)                                                            |
| AnniversaryDate | DateTime |    بله   | تاریخ سالگرد (فرمت: yyyy-MM-dd)                                                          |
| CustomText1-6   | String   |    بله   | فیلدهای متنی سفارشی ۱ تا ۶                                                               |
| CustomDate1-3   | DateTime |    بله   | تاریخ‌های سفارشی ۱ تا ۳                                                                  |
| Labels          | String[] |    بله   | لیست برچسب‌ها                                                                            |

---

## مقدار خروجی

| نام         | نوع    | الزامی | توضیحات                                 |
| ----------- | ------ | :----: | --------------------------------------- |
| ReturnValue | Object |    خیر   | شامل اطلاعات مخاطب ایجادشده یا پیام خطا |

---

## نمونه کدها
<div dir=ltr>

###  C#

```csharp
string Token = "MyToken";
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
string[] CustomText = {"", "", "", "", "", ""};
DateTime?[] CustomDate = {null, null, null};
string[] Labels = {"گروه 1", "گروه 2"};

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "FName", FName },
        { "LName", LName },
        { "GenderType", GenderType.ToString() },
        { "CompanyTitle", CompanyTitle },
        { "JobTitle", JobTitle },
        { "Email", Email },
        { "CityName", CityName },
        { "AddressText", AddressText },
        { "ContactGroupID", ContactGroupID.ToString() },
        { "BornDate", BornDate?.ToString("yyyy-MM-dd") },
        { "AnniversaryDate", AnniversaryDate?.ToString("yyyy-MM-dd") },
        { "CustomText1", CustomText[0] },
        { "CustomText2", CustomText[1] },
        { "CustomText3", CustomText[2] },
        { "CustomText4", CustomText[3] },
        { "CustomText5", CustomText[4] },
        { "CustomText6", CustomText[5] },
        { "CustomDate1", CustomDate[0]?.ToString("yyyy-MM-dd") },
        { "CustomDate2", CustomDate[1]?.ToString("yyyy-MM-dd") },
        { "CustomDate3", CustomDate[2]?.ToString("yyyy-MM-dd") },
        { "Labels", string.Join(",", Labels) },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactCreate", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
}
```

---

###  Python

```python
import requests

url = "https://portal.amootsms.com/rest/ContactCreate"
headers = {"Authorization": "MyToken"}
data = {
    "Mobile": "09120000000",
    "FName": "",
    "LName": "",
    "ContactGroupID": 0,
    "GenderType": True,
    "CompanyTitle": "",
    "JobTitle": "",
    "Email": "",
    "CityName": "",
    "AddressText": "",
    "BornDate": None,
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
    "Labels": ["گروه 1", "گروه 2"]
}

response = requests.post(url, headers=headers, data=data)
print(response.text)
```

---

###  PHP

```php
$token = "MyToken";
$data = [
    "Mobile" => "09120000000",
    "FName" => "",
    "LName" => "",
    "ContactGroupID" => 0,
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
    "Labels" => ["گروه 1", "گروه 2"]
];

$ch = curl_init("https://portal.amootsms.com/rest/ContactCreate");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

---

###  Node.js

```js
const axios = require('axios');
const token = 'MyToken';

axios.post('https://portal.amootsms.com/rest/ContactCreate', {
    Mobile: '09120000000',
    FName: '',
    LName: '',
    ContactGroupID: 0,
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
}, {
    headers: { Authorization: token }
}).then(res => console.log(res.data))
  .catch(err => console.error(err));
```

---

###  cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactCreate" \
  -H "Authorization: MyToken" \
  -F "Mobile=09120000000" \
  -F "FName=" \
  -F "LName=" \
  -F "ContactGroupID=0" \
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
  -F "Labels[]=گروه 2" \
  -F "Active=true"
```
</div>
---


## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در این الگو، متدهای وب‌سرویس در صورت بروز خطا **همیشه با کد وضعیت HTTP = 200 (OK)** پاسخ می‌دهند، اما بدنهٔ پاسخ نشان‌دهندهٔ خطاست. 

‌ بدنه یک شیء JSON است که در آن فیلد Status برابر **کد خطا** و فیلد Data معمولاً با مقادیر پیش‌فرض/خالی است؛ مثال:


<div dir="ltr">

```json
{
  "Status": "ContactID_Invalid",
  "Data": { /* ... */ }
}
```

</div>

> نتیجه‌گیری: **HTTP 200** الزاماً به معنی موفقیت نیست. همواره مقدار <span dir="ltr">Status</span> یا متن بدنه را بررسی کنید.


</div>
