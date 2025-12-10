<div dir="rtl">

# مستندات وب‌سرویس پیامک - مجموعه متدهای انتقال پیام‌های دریافتی (RelayRecieveMessage)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## فهرست

* [1) افزودن آدرس — RelayRecieveMessageCreate](#relay-recieve-message-create)
* [2) دریافت لیست آدرس‌ها — RelayRecieveMessageList](#relay-recieve-message-list)
* [3) فعال/غیرفعال کردن آدرس — RelayRecieveMessageSetActive](#relay-recieve-message-set-active)

---

## <a id="relay-recieve-message-create"></a>1) افزودن آدرس انتقال پیام‌های دریافتی (RelayRecieveMessageCreate)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayRecieveMessageCreate](https://portal.amootsms.com/rest/RelayRecieveMessageCreate)

### توضیح کلی

یک URL جدید برای انتقال **پیام‌های دریافتی** ثبت می‌کند. پس از ثبت، سامانه پیام‌های دریافتی را به این URL با **POST** ارسال می‌کند.

### پارامترهای ورودی

| نام        | نوع    | الزامی | توضیحات                                                  |
| ---------- | ------ | :----: | -------------------------------------------------------- |
| Token      | String |   بله  | توکن ثبت‌شده در هدر <span dir="ltr">Authorization</span> |
| URLAddress | String |   بله  | آدرس کامل با <span dir="ltr">http/https</span>           |

### مقدار خروجی (طبق سرویس واقعی)

| نام                   | نوع    | توضیحات                                      |
| --------------------- | ------ | -------------------------------------------- |
| Status                | String | مقدار «Success» یا کد خطا                    |
| RelayRecieveMessageID | Long   | شناسه رکورد ایجادشده؛ در خطا ممکن است 0 باشد |

#### نمونه خروجی موفق

```json
{
  "Status": "Success",
  "RelayRecieveMessageID": 999
}
```

#### نمونه خروجی ناموفق (تکراری بودن آدرس)

```json
{
  "Status": "URLAddress_Duplicate",
  "RelayRecieveMessageID": 0
}
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
string URLAddress = "http://Mysite.com/RelayRecieveMessage";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection() { { "URLAddress", URLAddress } };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageCreate", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json); // Status + RelayRecieveMessageID
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
payload = {"URLAddress": "http://Mysite.com/RelayRecieveMessage"}
r = requests.post("https://portal.amootsms.com/rest/RelayRecieveMessageCreate", data=payload, headers=headers)
print(r.json())
```

#### PHP

```php
$token = 'MyToken';
$post = ['URLAddress' => 'http://Mysite.com/RelayRecieveMessage'];
$ch = curl_init('https://portal.amootsms.com/rest/RelayRecieveMessageCreate');
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
const url = 'https://portal.amootsms.com/rest/RelayRecieveMessageCreate';
axios.post(url, new URLSearchParams({ URLAddress: 'http://Mysite.com/RelayRecieveMessage' }).toString(), {
  headers: { Authorization: token, 'Content-Type': 'application/x-www-form-urlencoded' }
}).then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RecvCreateExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayRecieveMessageCreate");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);
    String body = "URLAddress=http%3A%2F%2FMysite.com%2FRelayRecieveMessage";
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
curl -X POST "https://portal.amootsms.com/rest/RelayRecieveMessageCreate" \
  -H "Authorization: MyToken" \
  -d "URLAddress=http://Mysite.com/RelayRecieveMessage"
```

---

## <a id="relay-recieve-message-list"></a>2) دریافت لیست آدرس‌ها (RelayRecieveMessageList)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayRecieveMessageList](https://portal.amootsms.com/rest/RelayRecieveMessageList)

### توضیح کلی

لیست تمام آدرس‌های ثبت‌شده برای انتقال پیام‌های دریافتی را برمی‌گرداند. هر آیتم شامل شناسه، آدرس، وضعیت فعال و زمان ثبت است.

### پارامترهای ورودی

| نام   | نوع    | الزامی | توضیحات                           |
| ----- | ------ | :----: | --------------------------------- |
| Token | String |   بله  | توکن ثبت‌شده در هدر Authorization |

### مقدار خروجی (طبق سرویس واقعی)

| نام    | نوع    | توضیحات                   |
| ------ | ------ | ------------------------- |
| Status | String | مقدار «Success» یا کد خطا |
| Data   | Array  | آرایه‌ای از آدرس‌ها       |

#### ساختار هر آیتم از Data

| نام         | نوع    | توضیحات            |
| ----------- | ------ | ------------------ |
| ID          | Long   | شناسه رکورد        |
| EncryptedID | String | شناسه رمزنگاری‌شده |
| Active      | Bool   | وضعیت فعال         |
| RegDateTime | String | تاریخ/زمان ثبت     |
| URLAddress  | String | آدرس دریافت پیام   |

#### نمونه خروجی ناموفق (توکن ناموجود)

```json
{
  "Status": "Token_NotExists",
  "Data": []
}
```

#### نمونه خروجی موفق

```json
{
  "Status": "Success",
  "Data": [
    {
      "ID": 999,
      "EncryptedID": "",
      "Active": true,
      "RegDateTime": "2015-10-21T14:50:13.72",
      "URLAddress": "http://mysite.com/relayrecievemessage"
    },
    {
      "ID": 999,
      "EncryptedID": "",
      "Active": true,
      "RegDateTime": "2015-10-22T12:50:03.38",
      "URLAddress": "http://my1site.com/relayrecievemessage"
    }
  ]
}
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageList", new System.Collections.Specialized.NameValueCollection());
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(json);
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
r = requests.post("https://portal.amootsms.com/rest/RelayRecieveMessageList", headers=headers)
print(r.json())
```

#### PHP

```php
$token = 'MyToken';
$ch = curl_init('https://portal.amootsms.com/rest/RelayRecieveMessageList');
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
axios.post('https://portal.amootsms.com/rest/RelayRecieveMessageList', null, { headers: { Authorization: 'MyToken' } })
  .then(r => console.log(r.data)).catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RecvListExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayRecieveMessageList");
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
curl -X POST "https://portal.amootsms.com/rest/RelayRecieveMessageList" \
  -H "Authorization: MyToken"
```

---

## <a id="relay-recieve-message-set-active"></a>3) فعال/غیرفعال کردن آدرس (RelayRecieveMessageSetActive)

### آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/RelayRecieveMessageSetActive](https://portal.amootsms.com/rest/RelayRecieveMessageSetActive)

### توضیح کلی

وضعیت فعال/غیرفعال یک آدرس ثبت‌شده را تغییر می‌دهد.

### پارامترهای ورودی

| نام                   | نوع    | الزامی | توضیحات                                                     |
| --------------------- | ------ | :----: | ----------------------------------------------------------- |
| Token                 | String |   بله  | توکن در هدر Authorization                                   |
| RelayRecieveMessageID | Long   |   بله  | شناسه آدرس                                                  |
| Active                | Bool   |   بله  | <span dir="ltr">true</span> یا <span dir="ltr">false</span> |

### مقدار خروجی (طبق سرویس واقعی)

**رشتهٔ JSON (String)** با مقادیر زیر بسته به وضعیت:

* `"Success"`
* `"Token_NotExists"`

#### نمونه‌ها

```json
"Token_NotExists"
```

```json
"Success"
```

### نمونه کدها

#### C#

```csharp
string Token = "MyToken";
long RelayRecieveMessageID = 101;
bool Active = true;
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    var data = new System.Collections.Specialized.NameValueCollection() {
        { "RelayRecieveMessageID", RelayRecieveMessageID.ToString() },
        { "Active", Active.ToString().ToLower() }
    };
    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageSetActive", data);
    string body = System.Text.UTF8Encoding.UTF8.GetString(bytes);
    Console.WriteLine(body); // "Success" یا "Token_NotExists"
}
```

#### Python

```python
import requests

headers = {"Authorization": "MyToken"}
form = {"RelayRecieveMessageID": 101, "Active": "true"}
r = requests.post("https://portal.amootsms.com/rest/RelayRecieveMessageSetActive", data=form, headers=headers)
print(r.text)
```

#### PHP

```php
$token = 'MyToken';
$post = ['RelayRecieveMessageID' => 101, 'Active' => 'true'];
$ch = curl_init('https://portal.amootsms.com/rest/RelayRecieveMessageSetActive');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
$response = curl_exec($ch);
curl_close($ch);
echo $response; // "Success" یا "Token_NotExists"
```

#### Node.js

```js
const axios = require('axios');
axios.post('https://portal.amootsms.com/rest/RelayRecieveMessageSetActive', new URLSearchParams({
  RelayRecieveMessageID: '101', Active: 'true'
}).toString(), { headers: { Authorization: 'MyToken', 'Content-Type': 'application/x-www-form-urlencoded' } })
  .then(r => console.log(r.data))
  .catch(e => console.error(e.response?.data || e.message));
```

#### Java

```java
import java.io.*;
import java.net.*;

public class RecvSetActiveExample {
  public static void main(String[] args) throws Exception {
    String token = "MyToken";
    URL url = new URL("https://portal.amootsms.com/rest/RelayRecieveMessageSetActive");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("POST");
    conn.setRequestProperty("Authorization", token);
    conn.setDoOutput(true);
    String body = "RelayRecieveMessageID=101&Active=true";
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
curl -X POST "https://portal.amootsms.com/rest/RelayRecieveMessageSetActive" \
  -H "Authorization: MyToken" \
  -d "RelayRecieveMessageID=101" \
  -d "Active=true"
```

---

## نحوه پاسخ‌دهی وب‌سرویس در حالت خطا 

در این مجموعه نیز پاسخ HTTP در خطا **200 (OK)** است و نتیجه از بدنهٔ پاسخ مشخص می‌شود.

نمونه‌ها:

```json
{ "Status": "Token_NotExists", "Data": [] }
```

```json
"Token_NotExists"
```

</div>
