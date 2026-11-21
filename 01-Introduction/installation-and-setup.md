# نصب و راه‌اندازی .NET 11

# Installation and Setup

## ایجاد پروژه

```powershell
dotnet new console -n CSharp15Learning --framework net11.0
cd CSharp15Learning
dotnet run
```

اگر `net11.0` توسط SDK انتخاب‌شده پشتیبانی نمی‌شود، `.NET 11` روی ماشین نصب نشده یا SDK فعال نیست.

## کنترل پروژه با global.json

برای تعیین SDK دقیق در یک تیم یا CI:

```json
{
  "sdk": {
    "version": "11.0.100",
    "rollForward": "latestFeature"
  }
}
```

قبل از قطعی کردن شماره نسخه، از خروجی `dotnet --list-sdks` استفاده کنید. در CI، تصویرهای رسمی SDK را به جای نصب دستی گسترده ترجیح دهید.

## تنظیم `TargetFramework`

```xml
<TargetFramework>net11.0</TargetFramework>
```

این مقدار APIهای موجود در کتابخانه‌های .NET 11 را مشخص می‌کند و نباید با نسخه زبان اشتباه گرفته شود.

## تنظیم `LangVersion`

```xml
<LangVersion>15</LangVersion>
```

در پروژه‌های `.NET 11` معمولاً نسخه زبان پشتیبان‌گیری‌شده توسط SDK فعال است. `preview` را فقط برای آزمایش و با اعلام وضعیت تغییرات استفاده کنید.

## بررسی ناشران و ویژگی Preview

مستندات Microsoft برای هر ویژگی سطح پشتیبانی را مشخص می‌کند. در این ریپوزیتوری یک ویژگی را Stable در نظر نگیرید مگر مستند رسمی آن را با وضعیت قطعی معرفی کرده باشد.

```xml
<EnablePreviewFeatures>true</EnablePreviewFeatures>
```

این Property برای یک ویژگی خاص توضیح‌شده استفاده شود؛ روشن کردن سراسری Preview نمی‌تواند غیرقابل‌تثبیت بودن یک پروژه را پنهان کند.

## ساخت کدهای قابل اجرا

```csharp
var sdk = Environment.Version;
Console.WriteLine($"Runtime: {sdk}");
Console.WriteLine($".NET: {Environment.Version}");
```

نسخه دقیق Runtime با استفاده از assemblyها و `RuntimeInformation.FrameworkDescription` مناسب‌تر است:

```csharp
using System.Runtime.InteropServices;

Console.WriteLine(RuntimeInformation.FrameworkDescription);
```

## جمع‌بندی

نصب SDK، انتخاب TFM و انتخاب زبان سه لایه مستقل‌اند. اگر کامپایل موفق نبود، ابتدا خطا را از یکی از لایه‌های SDK، TargetFramework یا LangVersion تشخیص دهید.

## تمرین

یک ASP.NET Core 11 بسازید:

```powershell
dotnet new webapi -n Api11 --framework net11.0
dotnet run --project Api11
```

مسیر OpenAPI را پس از اجرای پروژه بررسی کنید و پاسخ خروجی را یادداشت نمایید.

## منابع

- [Install .NET on Windows](https://learn.microsoft.com/en-us/dotnet/core/install/windows)
- [global.json overview](https://learn.microsoft.com/en-us/dotnet/core/tools/global-json)
- [Target frameworks](https://learn.microsoft.com/en-us/dotnet/standard/frameworks)

