# متد وب سرویس آمار پیامک‌های ارسالی (Statistics)

با استفاده از این متد می‌توانید آمار و گزارش پیامک‌های ارسالی خود را دریافت نمایید.

<div style="border-left:6px solid #dc2626; background:#fff1f2; padding:14px 16px; border-radius:10px; box-shadow:0 1px 4px rgba(220,38,38,0.1);">
<div style="display:flex; gap:10px; align-items:center;">
<div>
<strong style="font-size:16px; color:#7f1d1d;">نکته</strong>
<div style="color:#991b1b; margin-top:4px;">این متد از دسترس خارج شده است</div>
</div>
</div>
</div>

---

> **آخرین بروزرسانی:** ۱۴۰۴/۰۷/۲۸

## آدرس وب‌سرویس

> [https://portal.amootsms.com/rest/Statistics](https://portal.amootsms.com/rest/Statistics)

---

## پارامترهای ورودی

| نام        | نوع    | الزامی | توضیحات                           |
| ---------- | ------ | ------ | --------------------------------- |
| `Token`    | String | بله      | توکن ثبت‌شده در سامانه پیامک آموت |
| `FromDate` | Object | بله      | تاریخ شروع بازه آماری             |
| `ToDate`   | Object | بله      | تاریخ پایان بازه آماری            |

---

## مقدار خروجی

| نام           | نوع    | توضیحات                                               |
| ------------- | ------ | ----------------------------------------------------- |
| `ReturnValue` | Object | شامل آمار و گزارش پیامک‌های ارسالی در بازه زمانی مشخص |

---

## نمونه کد (C#)

```csharp
string Token = "MyToken";
DateTime FromDate = DateTime.Now.AddMonths(-1);
DateTime ToDate = DateTime.Now;

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "FromDate", FromDate.ToString() },
        { "ToDate", ToDate.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/Statistics", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes); // خروجی
}
```

## نمونه کد (PHP)

```php
$Token = "MyToken";
$FromDate = date('Y-m-d', strtotime('-1 month'));
$ToDate = date('Y-m-d');

$data = array(
    'FromDate' => $FromDate,
    'ToDate' => $ToDate
);

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, "https://portal.amootsms.com/rest/Statistics");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    "Authorization: $Token"
));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
curl_close($ch);

$json = $response; // خروجی
```

