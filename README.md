# آموزش جامع C# 15 و .NET 11 برای توسعه‌دهندگان Backend

این ریپوزیتوری یک مسیر یادگیری فارسی، پروژه‌محور و مستقل برای **C# 15 و .NET 11** است. مطالب برای توسعه‌دهندگانی نوشته شده‌اند که با مبانی C# و .NET آشنا هستند و می‌خواهند قابلیت‌های جدید را در پروژه‌های واقعی Backend و ASP.NET Core به کار بگیرند.

> تاریخ بازبینی محتوای این پروژه: ۲۰ نوامبر ۲۰۲۶. برای هر قابلیت، مستندات رسمی Microsoft را مرجع نهایی در نظر بگیرید و قبل از استفاده Production، وضعیت Release و فهرست Breaking Changes نسخه هدف را دوباره بررسی کنید.

## نقشه یادگیری


| بخش | موضوع | فایل‌ها |
|---|---|---|
| مقدمه | نسخه‌ها، نصب و ساخت محیط | [01-Introduction/](01-Introduction/) |
| زبان C# 15 | Union، Closed Hierarchy و سایر قابلیت‌ها | [02-CSharp15/](02-CSharp15/) |
| .NET 11 | Runtime، SDK و کتابخانه‌ها | [03-DotNet11/](03-DotNet11/) |
| ASP.NET Core 11 | Minimal API، Validation و OpenAPI | [04-ASP.NET-Core11/](04-ASP.NET-Core11/) |
| EF Core 11 | Complex Types، LINQ، Vector، Full-Text، Migrations و Cosmos | [05-EFCore11/](05-EFCore11/) |
| هوش مصنوعی | Microsoft.Extensions.AI و MCP | [06-AI-DotNet/](06-AI-DotNet/) |
| مهاجرت | تصمیم‌گیری و چک‌لیست | [07-Migration/](07-Migration/) |
| پروژه عملی | مدیریت سفارش | [08-Project/](08-Project/) |
| تمرین‌ها | تمرین، مصاحبه و ارزیابی | [09-Exercises/](09-Exercises/) |

## پیش‌نیازها

- تجربه پایه با C# و برنامه‌نویسی شیءگرا
- آشنایی اولیه با `async`/`await`
- آشنایی مقدماتی با ASP.NET Core و Entity Framework Core
- VS Code یا Visual Studio 2026 به‌روزرسانی‌شده
- SQL Server یا SQL Server Container برای پروژه عملی

## شروع سریع

```powershell
dotnet --info
dotnet --list-sdks
dotnet --list-runtimes

dotnet new console -n CSharp15Learning --framework net11.0
cd CSharp15Learning
dotnet run
```

اگر `.NET 11` در خروجی نصب‌شده نبود، پروژه را روی SDKهای Preview یا Nightly اجرا نکنید تا پیش از افزودن آن‌ها با production channel تفاوت ندارند. ابتدا از صفحه رسمی دانلود .NET 11 و نصب‌کننده پلتفرم خود استفاده کنید.

## مبدأهای اصلی

- [What's new in C# 15](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15)
- [What's new in .NET 11](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/overview)
- [What's new in ASP.NET Core for .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [What's new in EF Core for .NET 11](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [What's New in .NET 11? A Practical Guide for Backend Developers](https://jamili.pro/whats-new-in-net-11-a-practical-guide-for-backend-developers/)

## قالب مطالعه هر فصل

هر درس شامل معرفی مفهوم، مسئله پیش از قابلیت، مقایسه روش قدیمی و جدید، کدهای قابل اجرا، کاربرد Backend، مزایا و محدودیت‌ها، تمرین و منابع رسمی است.

## فصل‌ها

1. [شناخت C# و .NET](01-Introduction/what-is-csharp15.md)
2. [نصب و راه‌اندازی](01-Introduction/installation-and-setup.md)
3. [C# 14 در مقابل C# 15](01-Introduction/what-is-dotnet11.md)
4. [Collection Expression Arguments](02-CSharp15/collection-expression-arguments.md)
5. [Union Types](02-CSharp15/union-types.md)
6. [Closed Hierarchies](02-CSharp15/closed-hierarchies.md)
7. [Extension Indexers](02-CSharp15/extension-indexers.md)
8. [Labeled break و continue](02-CSharp15/labeled-break-continue.md)
9. [Static Interface Members](02-CSharp15/static-interface-members.md)
10. [Memory Safety](02-CSharp15/memory-safety.md)
11. [Runtime Async](03-DotNet11/runtime-async.md)
12. [Performance](03-DotNet11/performance.md)
13. [SDK Improvements](03-DotNet11/sdk-improvements.md)
14. [Libraries](03-DotNet11/libraries.md)
15. [ASP.NET Core 11](04-ASP.NET-Core11/overview.md)
16. [Async Validation](04-ASP.NET-Core11/async-validation.md)
17. [OpenAPI 3.2](04-ASP.NET-Core11/openapi-32.md)
18. [Endpoint Filters](04-ASP.NET-Core11/endpoint-filters.md)
19. [EF Core 11](05-EFCore11/overview.md)
20. [Complex Types](05-EFCore11/complex-types.md)
21. [بهبود LINQ و SQL Translation](05-EFCore11/linq-improvements.md)
22. [FullJoin](05-EFCore11/full-join.md)
23. [JSON در SQL Server](05-EFCore11/json-sql-server.md)
24. [Vector Search](05-EFCore11/vector-search.md)
25. [Full-Text و Hybrid Search](05-EFCore11/full-text-search.md)
26. [Temporal Tables](05-EFCore11/temporal-tables.md)
27. [ترجمه تاریخ و زمان](05-EFCore11/date-time-translations.md)
28. [بهبود SQLite](05-EFCore11/sqlite.md)
29. [Azure Cosmos DB](05-EFCore11/cosmos-db.md)
30. [بهبود Migrations](05-EFCore11/migrations.md)
31. [راهنمای Migration به EF Core 11](05-EFCore11/migration-guide.md)
32. [AI Development](06-AI-DotNet/ai-development.md)
33. [Migration از .NET 10](07-Migration/dotnet10-to-dotnet11.md)
34. [Breaking Changes](07-Migration/breaking-changes.md)
35. [پروژه مدیریت سفارش](08-Project/order-management.md)
36. [تمرین‌های C# 15](09-Exercises/csharp15-exercises.md)
37. [تمرین‌های .NET 11](09-Exercises/dotnet11-exercises.md)
38. [سؤالات مصاحبه](09-Exercises/interview-questions.md)
39. [منابع](Resources/references.md)

