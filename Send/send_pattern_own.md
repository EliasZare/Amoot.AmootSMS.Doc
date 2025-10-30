#  متد ارسال پیامک از طریق الگو (پترن) با خط اختصاصی — SendWithPatternOWN

**آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸ 

---

##  آدرس وب‌سرویس  
>https://portal.amootsms.com/rest/SendWithPatternOWN


---

##  توضیح متد  
این متد برای ارسال پیامک بر اساس **الگوی تایید شده (Pattern)** از طریق **خط اختصاصی شما** استفاده می‌شود.  
برای استفاده از این متد باید ابتدا الگوی پیامک خود را در پنل کاربری ثبت و توسط اپراتور تأیید نمایید.  
پس از فعال شدن، می‌توانید آن را برای شماره‌های دلخواه ارسال کنید.

>  نکته:  
> مسیر ثبت الگو در پنل کاربری:  
> **الگوها → امکانات**

---

## ⚙️ پارامترهای ورودی  

| نام | نوع | توضیحات |
| --- | --- | --- |
| Token | String | الزامی — توکن ثبت شده در سامانه پیامک آموت |
| LineNumber | String | الزامی — شماره خط اختصاصی ارسال‌کننده |
| Mobile | String | الزامی — شماره موبایل دریافت‌کننده پیامک |
| PatternCodeID | Int | الزامی — شناسه الگوی پیامک |
| PatternValues | String[] | الزامی — آرایه‌ای از مقادیر برای جای‌گذاری در متن الگو (به همان ترتیب تعریف در پنل) |

---

##  مقدار خروجی  

**ReturnValue** — شامل یک شیء از اطلاعات پاسخ سرور:  

```json
{
  "Status": true,
  "MessageID": "9823749237",
  "Mobile": "09120000000",
  "LineNumber": "public",
  "PatternCodeID": 1,
  "PatternValues": ["پارامتر 1", "پارامتر 2"]
}
```

# نمونه کدها
## C#
```CSHARP
string Token = "MyToken";
string Mobile = "9120000000";
string LineNumber = "public";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "LineNumber", LineNumber },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",", PatternValues) },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendWithPatternOWN", data);
    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی JSON
}

```

## PHP
```PHP 
<?php
$Token = "MyToken";
$Mobile = "9120000000";
$LineNumber = "public";
$PatternCodeID = 1;
$PatternValues = array("پارامتر 1", "پارامتر 2");

$url = "https://portal.amootsms.com/rest/SendWithPatternOWN";

$data = array(
  "Mobile" => $Mobile,
  "LineNumber" => $LineNumber,
  "PatternCodeID" => $PatternCodeID,
  "PatternValues" => implode(",", $PatternValues)
);

$options = array(
  "http" => array(
    "header"  => "Authorization: $Token\r\n",
    "method"  => "POST",
    "content" => http_build_query($data)
  )
);

$context  = stream_context_create($options);
$result = file_get_contents($url, false, $context);
echo $result;
?>
```

## NodeJS

```JAVASCRIPT
const axios = require("axios");

const data = {
  Mobile: "9120000000",
  LineNumber: "public",
  PatternCodeID: 1,
  PatternValues: ["پارامتر 1", "پارامتر 2"].join(","),
};

axios.post("https://portal.amootsms.com/rest/SendWithPatternOWN", data, {
  headers: { Authorization: "MyToken" },
})
.then((res) => console.log(res.data))
.catch((err) => console.error(err.response.data));
```

##  CURL
```bash
curl -X POST "https://portal.amootsms.com/rest/SendWithPatternOWN" \
     -H "Authorization: MyToken" \
     -d "Mobile=9120000000" \
     -d "LineNumber=public" \
     -d "PatternCodeID=1" \
     -d "PatternValues=پارامتر 1,پارامتر 2"
```