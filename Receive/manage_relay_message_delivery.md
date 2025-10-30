<div dir="rtl">

# مستندات وب‌سرویس پیامک - مجموعه متدهای انتقال دلیوری پیام‌ها

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## فهرست

* [1) افزودن آدرس دریافت دلیوری — RelayMessageDeliveryCreate](#relaymessagedeliverycreate)
* [2) دریافت لیست آدرس‌ها — RelayMessageDeliveryList](#relaymessagedeliverylist)
* [3) فعال/غیرفعال کردن آدرس — RelayMessageDeliverySetActive](#relaymessagedeliverysetactive)

> این سند سه متد مرتبط با مدیریت آدرس‌های دریافت دلیوری را پوشش می‌دهد.

---

## <a name="relaymessagedeliverycreate"></a>1) افزودن آدرس دریافت دلیوری (RelayMessageDeliveryCreate)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayMessageDeliveryCreate](https://portal.amootsms.com/rest/RelayMessageDeliveryCreate)

### توضیحات

با این متد می‌توانید یک آدرس کامل (URL) را برای دریافت **گزارش تحویل پیامک‌ها** ثبت کنید. پس از ثبت، سامانه وضعیت تحویل هر پیام را با درخواست **POST** به آن URL ارسال می‌کند.

### پارامترهای ورودی

| نام        | نوع    | الزامی | توضیحات                                                     |
| ---------- | ------ | :----: | ----------------------------------------------------------- |
| Token      | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود |
| URLAddress | String |   بله  | آدرس کامل شامل اسکیم <span dir="ltr">http/https</span>      |

### مقدار خروجی (طبق سرویس واقعی)

آبجکت JSON با فیلدهای زیر:

| نام                    | نوع    | توضیحات                                                   |
| ---------------------- | ------ | --------------------------------------------------------- |
| Status                 | String | مقدار «Success» در صورت موفقیت یا کد خطای متناسب با ورودی |
| RelayMessageDeliveryID | Long   | شناسه رکورد ایجادشده؛ در خطا ممکن است 0 باشد              |

#### نمونه خروجی موفق

```json
{
  "Status": "Success",
  "RelayMessageDeliveryID": 99
}
```

#### نمونه خروجی ناموفق (تکراری بودن آدرس)

```json
{
  "Status": "URLAddress_Duplicate",
  "RelayMessageDeliveryID": 0
}
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
string URLAddress = "https://mysite.com/RelayMessageDelivery";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection() { { "URLAddress", URLAddress } };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliveryCreate", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json);
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
payload = {"URLAddress": "https://mysite.com/RelayMessageDelivery"}
r = requests.post("https://portal.amootsms.com/rest/RelayMessageDeliveryCreate", data=payload, headers=headers)
print(r.json())
```

#### PHP

```php
$token = 'MyToken';
$post = ['URLAddress' => 'https://mysite.com/RelayMessageDelivery'];
$ch = curl_init('https://portal.amootsms.com/rest/RelayMessageDeliveryCreate');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

#### Node.js

```js
const axios = require('axios');
const token = 'MyToken';
const url = 'https://portal.amootsms.com/rest/RelayMessageDeliveryCreate';
axios.post(url, new URLSearchParams({ URLAddress: 'https://mysite.com/RelayMessageDelivery' }).toString(), {
  headers: { Authorization: token, 'Content-Type': 'application/x-www-form-urlencoded' }
}).then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RelayCreateExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayMessageDeliveryCreate");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);
    String body = "URLAddress=https%3A%2F%2Fmysite.com%2FRelayMessageDelivery";
    try(OutputStream os = conn.getOutputStream()) { os.write(body.getBytes()); }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
      String line; StringBuilder sb = new StringBuilder();
      while((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString());
    }
  }
}
```

#### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/RelayMessageDeliveryCreate" \
  -H "Authorization: MyToken" \
  -d "URLAddress=https://mysite.com/RelayMessageDelivery"
```

---

## <a name="relaymessagedeliverylist"></a>2) دریافت لیست آدرس‌ها (RelayMessageDeliveryList)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayMessageDeliveryList](https://portal.amootsms.com/rest/RelayMessageDeliveryList)

### توضیح کلی

لیست تمام آدرس‌های ثبت‌شده برای دریافت دلیوری را برمی‌گرداند. هر آیتم شامل شناسه، آدرس، وضعیت فعال و زمان ثبت است.

### پارامترهای ورودی

| نام   | نوع    | الزامی | توضیحات                                                     |
| ----- | ------ | :----: | ----------------------------------------------------------- |
| Token | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود |

### مقدار خروجی (طبق سرویس واقعی)

آبجکت JSON با فیلدهای زیر:

| نام    | نوع    | توضیحات                                  |
| ------ | ------ | ---------------------------------------- |
| Status | String | مقدار «Success» در صورت موفقیت یا کد خطا |
| Data   | Array  | آرایه‌ای از آدرس‌ها                      |

#### ساختار هر آیتم از Data

| نام         | نوع    | توضیحات            |
| ----------- | ------ | ------------------ |
| ID          | Long   | شناسه رکورد        |
| EncryptedID | String | شناسه رمزنگاری‌شده |
| Active      | Bool   | وضعیت فعال بودن    |
| RegDateTime | String | زمان ثبت (ISO)     |
| URLAddress  | String | آدرس دریافت دلیوری |

#### نمونه خروجی

```json
{
  "Status": "Success",
  "Data": [
    {
      "ID": 99,
      "EncryptedID": "",
      "Active": true,
      "RegDateTime": "2015-10-21T14:53:30.56",
      "URLAddress": "https://mysite.com/relaymessagedelivery"
    }
  ]
}
```

#### نمونه خروجی ناموفق 

```json
{
    "Status": "Token_Invalid",
    "Data": []
}
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliveryList", new System.Collections.Specialized.NameValueCollection());
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json);
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
r = requests.post("https://portal.amootsms.com/rest/RelayMessageDeliveryList", headers=headers)
print(r.json())
```

#### PHP

```php
$token = 'MyToken';
$ch = curl_init('https://portal.amootsms.com/rest/RelayMessageDeliveryList');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

#### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/RelayMessageDeliveryList', null, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RelayListExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayMessageDeliveryList");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);
    try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
      String line; StringBuilder sb = new StringBuilder();
      while((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString());
    }
  }
}
```

#### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/RelayMessageDeliveryList" \
  -H "Authorization: MyToken"
```

---

## <a name="relaymessagedeliverysetactive"></a>3) فعال/غیرفعال کردن آدرس (RelayMessageDeliverySetActive)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayMessageDeliverySetActive](https://portal.amootsms.com/rest/RelayMessageDeliverySetActive)

### توضیح کلی

وضعیت فعال یا غیرفعال بودن یک آدرس ثبت‌شده را تغییر می‌دهد؛ زمانی مفید است که بخواهید موقتاً ارسال دلیوری به آدرس خاصی متوقف/فعال شود.

### پارامترهای ورودی

| نام                    | نوع    | الزامی | توضیحات                                                                    |
| ---------------------- | ------ | :----: | -------------------------------------------------------------------------- |
| Token                  | String |   بله  | توکن ثبت‌شده در سامانه آموت؛ در هدر Authorization ارسال شود                |
| RelayMessageDeliveryID | Long   |   بله  | شناسه آدرس (از لیست دریافت‌شده)                                            |
| Active                 | Bool   |   بله  | <span dir="ltr">true</span> = فعال، <span dir="ltr">false</span> = غیرفعال |

### مقدار خروجی

**رشتهٔ JSON (String)** با یکی از دو مقدار زیر:

* `"Success"`
* `"Failed"`

#### نمونه‌ها

```json
"Success"
```

```json
"Failed"
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
long RelayMessageDeliveryID = 42; // شناسه معتبر
bool Active = true;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection() {
        { "RelayMessageDeliveryID", RelayMessageDeliveryID.ToString() },
        { "Active", Active.ToString().ToLower() }
    };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliverySetActive", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json); // "Success" یا "Failed"
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
form = {"RelayMessageDeliveryID": 42, "Active": "true"}
r = requests.post("https://portal.amootsms.com/rest/RelayMessageDeliverySetActive", data=form, headers=headers)
print(r.text)  # "Success" یا "Failed"
```

#### PHP

```php
$token = 'MyToken';
$post = ['RelayMessageDeliveryID' => 42, 'Active' => 'true'];
$ch = curl_init('https://portal.amootsms.com/rest/RelayMessageDeliverySetActive');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // "Success" یا "Failed"
```

#### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/RelayMessageDeliverySetActive', new URLSearchParams({
  RelayMessageDeliveryID: '42', Active: 'true'
}).toString(), { headers: { Authorization: 'MyToken', 'Content-Type': 'application/x-www-form-urlencoded' } })
  .then(r => console.log(r.data)) // "Success" یا "Failed"
  .catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RelaySetActiveExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayMessageDeliverySetActive");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);
    String body = "RelayMessageDeliveryID=42&Active=true";
    try(OutputStream os = conn.getOutputStream()) { os.write(body.getBytes()); }
    try(BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
      String line; StringBuilder sb = new StringBuilder();
      while((line = br.readLine()) != null) sb.append(line);
      System.out.println(sb.toString()); // "Success" یا "Failed"
    }
  }
}
```

#### cURL

```bash
curl -X POST "https://portal.amootsms.com/rest/RelayMessageDeliverySetActive" \
  -H "Authorization: MyToken" \
  -d "RelayMessageDeliveryID=42" \
  -d "Active=true"
```

---

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا

در همهٔ این متدها، کُد HTTP در صورت خطا نیز **200 (OK)** است و تشخیص نتیجه صرفاً از محتوای بدنه انجام می‌شود. نمونه‌ها:

```json
{ "Status": "URLAddress_Duplicate", "RelayMessageDeliveryID": 0 }
```

```json
"Failed"
```

</div>
