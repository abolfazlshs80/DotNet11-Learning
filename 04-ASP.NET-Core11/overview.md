# تغییرات ASP.NET Core 11

# ASP.NET Core 11 Overview

ASP.NET Core 11 بخش‌هایی از چارچوب وب را در Minimal API، OpenAPI، validation، model binding و SignalR بهبود می‌دهد.

## Asynchronous Validation

Validationهای وابسته به database یا remote service با `async` اجرا می‌شوند تا قبل از ثبت entity موجودیت تکراری بررسی شود.

## OpenAPI 3.2

نسخه پشتیبانی‌شده OpenAPI را صریح تنظیم کنید:

```csharp
builder.Services.AddOpenApi(options =>
{
    options.OpenApiVersion =
        Microsoft.OpenApi.OpenApiSpecVersion.OpenApi3_2;
});
```

این API/article را با package Microsoft.AspNetCore.OpenApi نصب‌شده نسخه 11 تست کنید.

## Endpoint Filters

Endpoint filters بعد از route matching و parameter binding روی endpointهای Minimal API اعمال می‌شوند؛ Middleware برای کل pipeline و قبل/بعد از routing مناسب‌تر است.

```csharp
app.MapPost("/orders", async (CreateOrderRequest request) =>
{
    return Results.Created($"/orders/{Guid.NewGuid()}",
        new { request.CustomerId });
})
.AddEndpointFilter(async (context, next) =>
{
    var logger = context.HttpContext.RequestServices
        .GetRequiredService<ILoggerFactory>()
        .CreateLogger("EndpointFilter");

    logger.LogInformation("Invoking endpoint");
    return await next(context);
});
```

## Blazor و SignalR

تغییرات دقیق Blazor و SignalR را از release notes ASP.NET Core 11 بخوانید. یک قابلیت Web را به‌طور قطعی در .NET 11 ندانید مگر مستند `aspnetcore-11` آن را تأیید کند.

## خلاصه تصمیم‌گیری

| قابلیت | استفاده مناسب |
|---|---|
| Async validation | Database uniqueness و remote policy |
| OpenAPI 3.2 | مستندسازی قراردادهای streaming/binary |
| Endpoint filter | validation، logging و response transformation یک endpoint |
| Middleware | security، tracing و policyهای global |

## منابع

- [ASP.NET Core 11 what's new](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis)

