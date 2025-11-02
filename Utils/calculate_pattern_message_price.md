#  محاسبه هزینه ارسال با الگو — CalculatePatternMessagePrice


**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

**نسخه API:** <span dir="ltr">v1</span>

##  آدرس وب‌سرویس
>https://portal.amootsms.com/rest/CalculatePatternMessagePrice


---

##  توضیح متد
این متد هزینهٔ ارسال پیامک بر اساس یک الگوی ثبت‌شده در پنل را محاسبه می‌کند.  
با ارسال شناسه الگو، مقادیر متغیرها و شماره(ها) دریافت‌کننده، سرور مقدار هزینه را برمی‌گرداند (به واحد ریال یا مقدار تعیین‌شده در پنل).

> نکته: مقادیر `PatternValues` باید به همان ترتیبی که در الگو تعریف شده‌اند ارسال شوند تا محاسبه هزینه صحیح انجام شود.

---

##  پارامترهای ورودی
| نام | نوع | الزامی | توضیحات |
|---|---:|:---:|---|
| Token | String | بله | توکن احراز هویت پنل آموت |
| Mobile | String | بله | موبایل نمونه (برای تعیین اپراتور/تخمین هزینه) — می‌تواند تک شماره یا لیست باشد بسته به پیاده‌سازی |
| PatternCodeID | Int | بله | شناسه الگوی پیامک |
| PatternValues | String[] | بله | آرایه مقادیر متغیرهای داخل الگو (به ترتیب) |

---

##  مقدار خروجی
| نام | نوع | توضیحات |
|---|---:|---|
| ReturnValue | Int | هزینهٔ محاسبه‌شده (عدد صحیح) — مقدار هزینه نهایی برای ارسال با آن الگو |

### مثال JSON خروجی
```json
{
  "Status": 1,
  "ReturnValue": 1500
}
```

در مثال بالا مقدار 1500 نمایانگر هزینه نهایی (مثلاً به ریال) برای ارسال الگو است.
# نمونه کد

## C#

```CSHARP
string Token = "MyToken";
string Mobile = "09120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",", PatternValues) },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CalculatePatternMessagePrice", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی JSON
    Console.WriteLine(json);
}

```

## PHP 
```PHP
 $token = "MyToken";
$data = [
    "Mobile" => "09120000000",
    "PatternCodeID" => 1,
    "PatternValues" => "پارامتر 1,پارامتر 2"
];

$ch = curl_init("https://portal.amootsms.com/rest/CalculatePatternMessagePrice");
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Authorization: $token"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);
echo $response;

```

## NodeJs
```javascript
const axios = require("axios");
const token = "MyToken";

axios.post("https://portal.amootsms.com/rest/CalculatePatternMessagePrice", {
    Mobile: "09120000000",
    PatternCodeID: 1,
    PatternValues: ["پارامتر 1","پارامتر 2"].join(",")
}, {
    headers: { Authorization: token }
}).then(res => console.log(res.data))
  .catch(err => console.error(err.response ? err.response.data : err.message));
```

## CURL
```bash
curl -X POST "https://portal.amootsms.com/rest/CalculatePatternMessagePrice" \
  -H "Authorization: MyToken" \
  -d "Mobile=09120000000" \
  -d "PatternCodeID=1" \
  -d "PatternValues=پارامتر 1,پارامتر 2"
```

