<div dir="rtl">

# مستندات وب‌سرویس مخاطبین - دریافت تغییرات مخاطبین (ContactListUpdates)

**آخرین بروزرسانی:** ۱۴۰۴/۰۸/۰۴

**نسخه API:** <span dir="ltr">v1</span>

---

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/ContactListUpdates](https://portal.amootsms.com/rest/ContactListUpdates)

---

## توضیح کلی

از طریق این متد می‌توانید فهرستی از **مخاطبین به‌روزرسانی‌شده** را دریافت کنید. هر رکورد شامل شناسهٔ مخاطب، تاریخ آخرین به‌روزرسانی و وضعیت حذف است. این متد معمولاً برای **همگام‌سازی (Sync)** مخاطبین سمت کلاینت استفاده می‌شود.

> نکته: در نسخهٔ فعلی، پارامتر <span dir="ltr">ContactGroupID</span> صرفاً برای اعتبارسنجی بررسی می‌شود (نباید منفی باشد) و در فیلتر خروجی اعمال نمی‌گردد.

---

## پارامترهای ورودی

| نام            | نوع    | الزامی | توضیحات                                                                               |
| -------------- | ------ | :----: | ------------------------------------------------------------------------------------- |
| Token          | String |   بله  | توکن ثبت‌شده در سامانه پیامک آموت (ارسال در هدر <span dir="ltr">Authorization</span>) |
| ContactGroupID | Long   |   بله  | شناسهٔ گروه مخاطبین؛ مقدار منفی نامعتبر است.                                          |

---

## مقدار خروجی

| نام    | نوع    | الزامی | توضیحات                                                   |
| ------ | ------ | :----: | --------------------------------------------------------- |
| Status | String |   خیر  | وضعیت پاسخ (`Success` در صورت موفقیت یا یکی از کدهای خطا) |
| Data   | Array  |   خیر  | آرایه‌ای از اشیای شامل اطلاعات به‌روزرسانی‌شده            |

### ساختار هر آیتم در <span dir="ltr">Data</span>

| فیلد           | نوع      | توضیح                                                                               |
| -------------- | -------- | ----------------------------------------------------------------------------------- |
| ID             | Long     | شناسه یکتای مخاطب                                                                   |
| UpdateDateTime | DateTime | زمان آخرین به‌روزرسانی رکورد (فرمت: <span dir="ltr">yyyy-MM-ddTHH:mm:ss.fff</span>) |
| Deleted        | Bool     | وضعیت حذف (در این نسخه همیشه `false`)                                               |

---

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این نوع، پاسخ HTTP همواره `200 (OK)` است ولی نتیجهٔ واقعی از طریق فیلد <span dir="ltr">Status</span> مشخص می‌شود. در صورت خطا، مقدار `Data` خالی برگردانده می‌شود.

---

## نمونه خروجی موفق

<div dir="ltr">

```json
{
    "Status": "Success",
    "Data": [
        {
            "ID": 12345678,
            "UpdateDateTime": "2025-10-20T13:11:03.35",
            "Deleted": false
        },
        {
            "ID": 12345679,
            "UpdateDateTime": "2025-10-21T11:03:41.96",
            "Deleted": false
        }
    ]
}
```

</div>

---

## نمونه خروجی ناموفق

<div dir="ltr">

```json
{
    "Status": "ContactGroupID_Invalid",
    "Data": []
}
```

</div>

---

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
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactListUpdates", data);
    string json = System.Text.Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json);
}
```

### Python

```python
import requests
url = "https://portal.amootsms.com/rest/ContactListUpdates"
headers = {"Authorization": "MyToken"}
data = {"ContactGroupID": 0}
response = requests.post(url, headers=headers, data=data)
print(response.text)
```

### PHP

```php
$Token = "MyToken";
$ContactGroupID = 0;
$ch = curl_init('https://portal.amootsms.com/rest/ContactListUpdates');
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $Token"]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query(['ContactGroupID' => $ContactGroupID]));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

### Node.js

```javascript
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/ContactListUpdates', { ContactGroupID: 0 }, {
  headers: { Authorization: 'MyToken' }
})
.then(res => console.log(res.data))
.catch(err => console.error(err));
```

### Java

```java
import java.io.*; import java.net.*; import java.nio.charset.StandardCharsets;
String token = "MyToken";
String data = "ContactGroupID=0";
URL url = new URL("https://portal.amootsms.com/rest/ContactListUpdates");
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
curl -X POST "https://portal.amootsms.com/rest/ContactListUpdates" \
  -H "Authorization: MyToken" \
  -d "ContactGroupID=0"
```

</div>
