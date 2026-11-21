# Endpoint Filters

# Endpoint Filters in ASP.NET Core 11

Endpoint Filter روی Minimal API endpointها اعمال می‌شود و می‌تواند:

- parameter binding را ببینید
- response را تغییر دهد
- validation و logging انجام دهد
- exception را به response تبدیل کند

Middleware روی کل request pipeline اعمال می‌شود و برای global concerns مناسب است.

## پیاده‌سازی

```csharp
app.MapPost("/orders", (CreateOrderRequest request) =>
{
    return Results.Created("/orders/new-id", request);
})
.AddEndpointFilter(async (context, next) =>
{
    var request = context.Arguments
        .OfType<CreateOrderRequest>()
        .FirstOrDefault();

    if (request is null)
        return Results.BadRequest("Invalid order request.");

    var logger = context.HttpContext.RequestServices
        .GetRequiredService<ILoggerFactory>()
        .CreateLogger("EndpointFilter");

    logger.LogInformation("Processing order for {CustomerId}",
        request.CustomerId);

    var result = await next(context);

    logger.LogInformation("Endpoint completed");
    return result;
});
```

## Binding error

Microsoft .NET 11 در مستند ASP.NET Core به improved parameter binding errors اشاره می‌کند. خطا را به response عمومی، schema problem و logهای معتبر تبدیل کنید.

```csharp
return Results.ValidationProblem(new Dictionary<string, string[]>
{
    ["request"] = ["Request body or route parameter is invalid."]
});
```

## اعتبار سنجی security

- input را در filter trust نکنید
- authorization policy را در middleware یا endpoint metadata نگه دارید
- PII را در log نکنید
- exceptionهای داده حساس را به `ProblemDetails` عمومی تبدیل کنید

## منابع

- [Endpoint filters](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/min-api-filters)
- [ASP.NET Core 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
