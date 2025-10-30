<div dir="rtl">

# مستندات وب‌سرویس مخاطبین - دریافت اطلاعات 

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۹

**نسخه API:** <span dir="ltr">v1</span>


## الگوی خطادهی 

* در حالت خطا پاسخ **HTTP 200 (OK)** است ولی بدنهٔ پاسخ نشان‌دهندهٔ خطاست. همیشه مقدار <span dir="ltr">Status</span> را بررسی کنید.
* **موفق:** <span dir="ltr">Status = "Success"</span> و <span dir="ltr">Data</span> شامل خروجی معتبر است.
* **ناموفق:** <span dir="ltr">Status</span> هر مقدار دیگری باشد (یا در برخی متدها متن سادهٔ کد خطا برگردد).

---

# 1) دریافت اطلاعات مخاطب (ContactGet)

**آدرس وب‌سرویس**

> [https://portal.amootsms.com/rest/ContactGet](https://portal.amootsms.com/rest/ContactGet)

## توضیح کلی

از طریق این متد می‌توانید اطلاعات یک مخاطب را با شناسهٔ آن دریافت کنید.

## پارامترهای ورودی

| نام       | نوع    | الزامی | توضیحات                                                                            |
| --------- | ------ | :----: | ---------------------------------------------------------------------------------- |
| Token     | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر <span dir="ltr">Authorization</span> ارسال شود |
| ContactID | Long   |   بله  | شناسهٔ یکتای مخاطب                                                                 |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                 |
| ------ | ------ | :----: | ------------------------------------------------------- |
| Status | String |   خیر  | <span dir="ltr">Success</span> در صورت موفقیت یا کد خطا |
| Data   | Object |   خیر  | در حالت موفق، شامل شیء <span dir="ltr">Contact</span>   |


## نمونه کدها

### C#

```csharp
string Token = "MyToken";
long ContactID = 0;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString() }
    };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactGet", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/ContactGet"
headers = {"Authorization": "MyToken"}
res = requests.post(url, headers=headers, data={"ContactID": 0})
print(res.text)
```

### PHP

```php
$Token = "MyToken";
$ContactID = 0;
$ch = curl_init("https://portal.amootsms.com/rest/ContactGet");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(["ContactID" => $ContactID]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```js
const axios = require('axios');
const token = 'MyToken';
axios.post('https://portal.amootsms.com/rest/ContactGet', { ContactID: 0 }, { headers: { Authorization: token } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
public class ContactGet {
  public static void main(String[] args) throws Exception {
    URL url = new URL("https://portal.amootsms.com/rest/ContactGet");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", "MyToken");
    conn.setDoOutput(true);
    try(OutputStream os = conn.getOutputStream()){
      os.write("ContactID=0".getBytes(StandardCharsets.UTF_8));
    }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))){
      String line; StringBuilder sb = new StringBuilder();
      while((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString());
    }
  }
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactGet" \
  -H "Authorization: MyToken" \
  -F "ContactID=0"
```

### نمونه خروجی موفق

<div dir="ltr">

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
        "UpdateDateTime": "2025-10-21T11:03:41.96"
  }
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{
  "Status": "Failed",
  "Data": {}
}
```

</div>

---

# 2) دریافت لیست گروه‌های مخاطبین (ContactGroupList)

**آدرس وب‌سرویس**

> [https://portal.amootsms.com/rest/ContactGroupList](https://portal.amootsms.com/rest/ContactGroupList)

## توضیح کلی

لیست گروه‌ها/برچسب‌های موجود در دفترچه تلفن را برمی‌گرداند.

## پارامترهای ورودی

| نام   | نوع    | الزامی | توضیحات                                                   |
| ----- | ------ | :----: | --------------------------------------------------------- |
| Token | String |   بله  | توکن ثبت‌شده؛ در هدر <span dir="ltr">Authorization</span> |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                                       |
| ------ | ------ | :----: | ----------------------------------------------------------------------------- |
| Status | String |   خیر  | <span dir="ltr">Success</span> یا کد خطا                                      |
| Data   | Object |   خیر  | در موفقیت، آرایهٔ <span dir="ltr">Groups</span>/<span dir="ltr">Labels</span> |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": [
        {
            "ContactGroupID": 123456,
            "Title": "عنوان گروه",
            "ContactsCount": 0,
            "ContactsActiveCount": 0
        },
        {
            "ContactGroupID": 123456,
            "Title": "عنوان گروه",
            "ContactsCount": 0,
            "ContactsActiveCount": 0
        }
    ]
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{ "Status": "Token_Invalid", "Data": [] }
```

</div>

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection();
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactGroupList", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
r = requests.post("https://portal.amootsms.com/rest/ContactGroupList", headers={"Authorization":"MyToken"})
print(r.text)
```

### PHP

```php
$Token = "MyToken";
$ch = curl_init("https://portal.amootsms.com/rest/ContactGroupList");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
echo curl_exec($ch);
curl_close($ch);
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/ContactGroupList', {}, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
URL url = new URL("https://portal.amootsms.com/rest/ContactGroupList");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setRequestProperty("Authorization", "MyToken");
conn.setDoOutput(true);
try(OutputStream os = conn.getOutputStream()) { os.write(new byte[0]); }
try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))) {
  String line; StringBuilder sb = new StringBuilder();
  while((line = br.readLine()) != null) sb.append(line);
  System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactGroupList" \
  -H "Authorization: MyToken"
```

---

# 3) دریافت لیست مخاطبین گروه (ContactList)

**آدرس وب‌سرویس**

> [https://portal.amootsms.com/rest/ContactList](https://portal.amootsms.com/rest/ContactList)

## توضیح کلی

مخاطبین موجود در یک گروه مشخص (برچسب) را برمی‌گرداند.

## پارامترهای ورودی

| نام            | نوع    | الزامی | توضیحات                                                   |
| -------------- | ------ | :----: | --------------------------------------------------------- |
| Token          | String |   بله  | توکن ثبت‌شده؛ در هدر <span dir="ltr">Authorization</span> |
| ContactGroupID | Long   |   بله  | کد گروه (۰ = بدون فیلتر، -۱ = بدون برچسب)                 |

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                           |
| ------ | ------ | :----: | ------------------------------------------------- |
| Status | String |   خیر  | <span dir="ltr">Success</span> یا کد خطا          |
| Data   | Object |   خیر  | در موفقیت، آرایهٔ <span dir="ltr">Contacts</span> |

### نمونه خروجی موفق

<div dir="ltr">

```json
{
  "Status": "Success",
  "Data": [
        {
            "ID": 123456,
            "Active": true,
            "ContactGroupID": 1234,
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
            "UpdateDateTime": "2025-10-21T11:03:41.96"
        }
    ]
}
```

</div>

### نمونه خروجی ناموفق

<div dir="ltr">

```json
{ "Status": "Token_Invalid", "Data": [] }
```

</div>

## نمونه کدها

### C#

```csharp
string Token = "MyToken";
long ContactGroupID = 0;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactGroupID", ContactGroupID.ToString() }
    };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactList", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    System.Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/ContactList"
headers = {"Authorization": "MyToken"}
res = requests.post(url, headers=headers, data={"ContactGroupID": 0})
print(res.text)
```

### PHP

```php
$Token = "MyToken"; $ContactGroupID = 0;
$ch = curl_init("https://portal.amootsms.com/rest/ContactList");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(["ContactGroupID" => $ContactGroupID]));
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
echo curl_exec($ch);
curl_close($ch);
```

### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/ContactList', { ContactGroupID: 0 }, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response ? e.response.data : e.message));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
URL url = new URL("https://portal.amootsms.com/rest/ContactList");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
conn.setRequestMethod("POST");
conn.setRequestProperty("Authorization", "MyToken");
conn.setDoOutput(true);
try(OutputStream os = conn.getOutputStream()){
  os.write("ContactGroupID=0".getBytes(StandardCharsets.UTF_8));
}
try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream(), StandardCharsets.UTF_8))){
  String line; StringBuilder sb = new StringBuilder();
  while((line = br.readLine()) != null) sb.append(line);
  System.out.println(sb.toString());
}
```

### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/ContactList" \
  -H "Authorization: MyToken" \
  -F "ContactGroupID=0"
```

</div>
