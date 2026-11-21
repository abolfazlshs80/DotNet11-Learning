# مقایسه C# 14 و C# 15 و .NET 10 و .NET 11

# C# 14 vs C# 15 and .NET 10 vs .NET 11

## جدول مقایسه C#

| قابلیت | C# 14 | C# 15 |
|---|---|---|
| Collection Expressions | پشتیبانی پایه | افزودن آرگومان سازنده/فکتوری با عنصر `with(...)` |
| مدل Union | معمولاً Discriminated resultهای نوع کاربر | Union Types زبان |
| Hierarchy | sealed و inheritance معمولی | Closed Hierarchies |
| Indexer روی انواع خارجی | Extension Method، نه indexer | Extension Indexer |
| Static interface members | static abstract/virtual در زمان‌های قبلی | سازگاری برای اعضای static غیرمجازی روی Runtimeهای قدیمی |

جدول فقط خلاصه است؛ جزئیات و رفتار نهایی هر قابلیت در مستندات فصل مربوطه آمده است.

## مقایسه پلتفرم

| بخش | .NET 10 | .NET 11 |
|---|---|---|
| Runtime Async | الگوی کامپایلری async رایج؛ برخی امکانات Preview در زمان‌های خاص | Runtime Async بخشی از اجرای نسخه هدف `net11.0` |
| SDK/Build | MSBuild و CLI نسخه قبلی | بهبودهای MSBuild Server، test، container و CLI |
| ASP.NET Core | تغییرات نسخه 10 | Async Validation، OpenAPI 3.2 و بهبود Endpointها |
| EF Core | امکانات پایه | بهبود LINQ و پشتیبانی‌های اضافه برای FullJoin و قابلیت‌های Vector در Providerهای مرتبط |
| AI | Microsoft.Extensions.AI و MCP در اکوسیستم موجود | استفاده گسترده‌تر و ادغام تکمیلی؛ نباید همۀ MCP را جدید .NET 11 دانست |

## روش قدیمی و روش جدید

قبل از C# 15، مدل‌های Union معمولاً به abstract record، base hierarchy یا کتابخانه‌های شخص ثالث وابسته بودند. اینها نوعاً مسائل exhaustiveness و متنوع بودن تبدیلها را کامل حل نمی‌کردند:

```csharp
public abstract record CreateUserResult;
public sealed record UserCreated(Guid Id) : CreateUserResult;
public sealed record DuplicateEmail : CreateUserResult;
public sealed record InvalidEmail : CreateUserResult;
```

با Union Type، خود زبان مجموعه caseها را مدل می‌کند؛ اما محل قرارگیری و assembly visibilityها را نیز بررسی می‌کند.

## چه چیزی نباید مهاجرت‌پذیر دانسته شود

- وجود یک API در مستند انتشار، به معنی سازگاری کامل با همه Providerها نیست.
- داشتن نسخه .NET در نام پروژه، به معنی فعال بودن همه امکانات زبان نیست.
- ارائه Benchmarkهای عمومی، جایگزین Benchmark محلی برنامه شما نیست.

## جمع‌بندی

ارتقا صرفاً افزایش شماره نیست؛ C# 15، Runtime و Framework هرکدام تغییرات جداگانه دارند. مهاجرت را با یک compatibility matrix بین SDK، NuGet، Provider و Infrastructure انجام دهید.

## منابع

- [C# 15 release overview](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15)
- [.NET 11 release overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/overview)
- [ASP.NET Core 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [EF Core 11](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
