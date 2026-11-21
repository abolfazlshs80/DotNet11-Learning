# شناخت C# 15 و .NET 11

# C# 15 / .NET 11 — Introduction

## فهرست مطالب

- C# و .NET چیست؟
- رابطه Compiler و Runtime
- تفاوت SDK و Runtime
- C# 15 و .NET 11
- C# 14 در مقابل C# 15
- LTS و STS

## C# چیست؟

C# یک زبان برنامه‌نویسی strongly typed و managed است که توسط کامپایلر C# به IL تبدیل می‌شود. Compiler فقط نقش ترجمه ندارد؛ برای مواردی مثل overload resolution، type checking، exhaustiveness checking و تولید state machine در کدهای async نیز مسئول است.

## .NET چیست؟

.NET یک پلتفرم اجرای managed شامل CoreCLR، کتابخانه‌های پایه و ابزارهای SDK است. Runtime حافظه، thread، garbage collection، JIT و loading assemblyها را مدیریت می‌کند.

## رابطه C# Compiler با .NET Runtime

```text
Source C# -> C# Compiler -> IL -> JIT/AOT -> Machine Code
                         -> Metadata -> Runtime
```

کامپایلر Syntax و نوع‌ها را کنترل می‌کند و Runtime آن IL را اجرا می‌کند. یک قابلیت C# ممکن است به IL معمولی تبدیل شود؛ یک قابلیت Runtime مثل Runtime Async مستقیماً به اجرای کد در Runtime مربوط است.

## SDK و Runtime

| جزء | کاربرد |
|---|---|
| .NET SDK | کامپایلر، MSBuild، CLI و قالب پروژه |
| .NET Runtime | اجرای برنامه‌های کامپایل‌شده |
| ASP.NET Core Shared Framework | Middleware، Routing، Kestrel و خدمات وب |
| EF Core | کتابخانه NuGet مستقل از Runtime |

یک سرور ممکن است Runtime نصب کند و پروژه را از SDK دیگری بسازد. برای سازگاری Deployment از خودکشیدن یا متعادل‌سازی خودکار صرفاً به نام نسخه اکتفا نکنید؛ `dotnet --info` را روی هر محیط بررسی کنید.

## C# 15 چیست؟

C# 15 مجموعه‌ای از گسترش‌های زبان است که در مستندات رسمی، از جمله Collection Expression Arguments، Union Types، Closed Hierarchies، Extension Indexers و بهبودهای Memory Safety معرفی شده‌اند. این قابلیتها در Compiler و ویژگی‌های سمولتیک C# هستند؛ همهٔ آن‌ها به معنای تغییر رفتار Core Runtime نیستند.

## .NET 11 چیست؟

.NET 11 نسخه پلتفرم .NET شامل Runtime، SDK، کتابخانه‌های پایه، ASP.NET Core و EF Core است. برخلاف تصور رایج، C# 15 صرفاً درون .NET 11 قرار نمی‌گیرد؛ C# در هر ورژن SDK قابل تنظیم و استفاده با Target Frameworkهای مختلف است.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net11.0</TargetFramework>
    <LangVersion>15</LangVersion>
  </PropertyGroup>
</Project>
```

یک پروژه `.NET 11` معمولاً باید با SDK 11 و C# 15 ساخته شود؛ اما تنظیم `LangVersion` را بدون ضرورت بالا نکنید مگر می‌خواهید Syntaxهای نسخه جدید را بررسی کنید.

## چرخه پشتیبانی

- **LTS**: پشتیبانی بلندمدت
- **STS**: پشتیبانی کوتاه‌مدت

اگر سازمان‌تان به چرخه پشتیبانی پایدار، rollback و نگهداری چندساله نیاز دارد، ابتدا تاریخ انتشار، LTS یا STS بودن و End-of-Support را از صفحه رسمی lifecycle بررسی کنید؛ این اطلاعات ثابت نیست و نباید از حافظه نقل شود.

## دستورهای بررسی

```powershell
dotnet --info
dotnet --list-sdks
dotnet --list-runtimes
dotnet --version
```

برای نصب .NET 11 از [download page](https://dotnet.microsoft.com/download/dotnet/11.0) استفاده کنید. برای نمونه فعلی محیط این پروژه با SDK 10 کار می‌کند؛ بنابراین مثال‌ها با APIهای قطعی مستندات رسمـی نوشته شده‌اند و اجرای آن‌ها نیازمند SDK/.NET 11 است.

## جمع‌بندی

C# زبان است، .NET پلتفرم اجرا و کتابخانه است و SDK ابزار ساخت. C# 15 و .NET 11 هم‌زمان رشد می‌کنند اما باید قابلیت‌ها را از نظر Compiler، Runtime و Framework تفکیک کرد.

## تمرین

یک گزارش `environment.txt` بسازید و خروجی‌های زیر را ذخیره کنید:

```powershell
dotnet --version
dotnet --list-sdks
dotnet --list-runtimes
```

توضیح دهید کدام خروجی نسخه SDK و کدام‌ها نسخه Runtime را نشان می‌دهند.

## منابع

- [What’s new in C# 15](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15)
- [.NET SDK and Runtime](https://learn.microsoft.com/en-us/dotnet/core/install/)
- [.NET and .NET Core Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

