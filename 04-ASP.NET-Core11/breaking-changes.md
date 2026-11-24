# Breaking Changes در ASP.NET Core 11

## OpenAPI dependency

پشتیبانی OpenAPI 3.2 با ارتقای dependency به Microsoft.OpenApi 3.3.1 همراه است و ممکن است breaking changeهای کتابخانه زیرین را وارد پروژه کند. هنگام ارتقا، transformerها و کدهای سفارشی OpenAPI را build و تست کنید.

## Blazor

- template جدید Blazor Web App دیگر `UseAntiforgery()` را صریحاً اضافه نمی‌کند؛ antiforgery middleware به‌صورت خودکار تزریق می‌شود. قبل از حذف فراخوانی دستی از مسیرهای سفارشی middleware اطمینان حاصل کنید.
- QuickGrid با URL-based navigation می‌تواند header و paginator را به‌صورت `<a>` render کند؛ CSSهای سفارشی که فقط `button` را هدف می‌گیرند باید بازبینی شوند.
- sort URL بر اساس `Title` ستون است؛ تغییر title ممکن است لینک‌های ذخیره‌شده را از کار بیندازد.
- در QuickGridهای متعدد، query parameterها را یکتا کنید و برای هر grid `PaginationState` جدا داشته باشید.
- حذف `inspectUri` در launch profileهای standalone WebAssembly که از Blazor Gateway استفاده می‌کنند را طبق راهنمای مهاجرت انجام دهید.

## Packageهای Deprecated و Experimental

- `Microsoft.AspNetCore.Grpc.Swagger` deprecated شده است؛ مسیر جایگزین را پیش از حذف بررسی کنید.
- Device Bound Session Credentials و برخی APIهای QuickGrid/Blazor در متن Experimental اعلام شده‌اند.

## روش ارتقا

1. راهنمای رسمی Breaking Changes نسخه هدف را بخوانید.
2. packageهای ASP.NET Core و Microsoft.OpenApi را هم‌نسخه و سازگار کنید.
3. CSS و URLهای QuickGrid و launch profileهای Blazor را بازبینی کنید.
4. OpenAPI generation، antiforgery، احراز هویت و endpointهای gRPC را در تست‌ها بررسی کنید.
5. قابلیت‌های Experimental را از وابستگی‌های production جدا نگه دارید.

## منابع

- [Breaking changes in ASP.NET Core for .NET 11](https://learn.microsoft.com/en-us/aspnet/core/breaking-changes/11)
- [Breaking changes in .NET](https://learn.microsoft.com/en-us/dotnet/core/compatibility/)
