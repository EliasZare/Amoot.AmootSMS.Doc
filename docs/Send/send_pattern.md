# SendWithPattern - ارسال از طریق الگو (پترن)

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

## آدرس وب سرویس

>https://portal.amootsms.com/rest/SendWithPattern

## توضیح متد

با استفاده از این متد می‌توانید الگوی پیامک ثبت شده در پنل کاربری را به شماره انتخابی ارسال کنید. قبل از استفاده، باید الگوی پیامک خود را در پنل کاربری ثبت و پس از تایید اپراتور، فعال نمایید.

## پارامترهای ورودی

| نام           | نوع      | توضیحات                                    |
| ------------- | -------- | ------------------------------------------ |
| Token         | String   | الزامی، توکن ثبت شده در سامانه پیامک آموت  |
| Mobile        | String   | الزامی، شماره موبایل دریافت‌کننده پیامک    |
| PatternCodeID | Int      | الزامی، کد الگوی پیامک                     |
| PatternValues | String[] | الزامی، مقادیر متغیرهای داخل الگو به ترتیب |

## مقدار خروجی

`ReturnValue` - Object شامل اطلاعات ارسال پیامک

## نمونه کد

### C#

```csharp
string Token = "MyToken";
string Mobile = "9120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",",PatternValues) },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendWithPattern", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی
}
```

### PHP

```php
$token = 'MyToken';
$mobile = '9120000000';
$patternCodeID = 1;
$patternValues = ['پارامتر 1', 'پارامتر 2'];

$ch = curl_init('https://portal.amootsms.com/rest/SendWithPattern');
curl_setopt($ch, CURLOPT_HTTPHEADER, ['Authorization: '.$token]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query([
    'Mobile' => $mobile,
    'PatternCodeID' => $patternCodeID,
    'PatternValues' => implode(',', $patternValues)
]));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

### NodeJS

```javascript
const axios = require('axios');

const token = 'MyToken';
const mobile = '9120000000';
const patternCodeID = 1;
const patternValues = ['پارامتر 1', 'پارامتر 2'];

axios.post('https://portal.amootsms.com/rest/SendWithPattern', {
    Mobile: mobile,
    PatternCodeID: patternCodeID,
    PatternValues: patternValues.join(',')
}, {
    headers: { Authorization: token }
}).then(res => console.log(res.data));
```

### CURL

```bash
curl -X POST https://portal.amootsms.com/rest/SendWithPattern \
-H "Authorization: MyToken" \
-d "Mobile=9120000000" \
-d "PatternCodeID=1" \
-d "PatternValues=پارامتر 1,پارامتر 2"
```
