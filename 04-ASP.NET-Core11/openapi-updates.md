# بهبودهای OpenAPI در ASP.NET Core 11

برای تنظیم اولیه OpenAPI 3.2 و endpointها، [openapi-32.md](openapi-32.md) را ببینید. این فایل تغییرات دیگر توصیف OpenAPI را مرور می‌کند.

## پاسخ فایل و stream

- `FileContentResult`، `FileStreamResult`، `FileContentHttpResult` و `FileStreamHttpResult` می‌توانند به‌صورت `string` با `format: binary` مستند شوند.
- برای تولید schema دقیق، نوع و content type پاسخ را با APIهای `Produces<T>` معرفی کنید.
- Server-Sent Events در OpenAPI 3.2 از `itemSchema` برای توصیف نوع هر event پشتیبانی می‌کنند.
- برای SSE از `TypedResults.ServerSentEvents(...)` استفاده کنید؛ برگرداندن مستقیم `IAsyncEnumerable<SseItem<T>>` می‌تواند به‌جای SSE به‌صورت JSON serialize شود.

## HTTP QUERY

تولید OpenAPI، HTTP `QUERY` را می‌شناسد. این verb برای queryهایی مناسب است که body دارند و در URL جای نمی‌گیرند. نمایش inline این operation به OpenAPI 3.2 وابسته است؛ در نسخه‌های قبلی، سند از extension استفاده می‌کند.

## Schemaها و پاسخ‌های چندگانه

- مقادیر enum در query/route/header/form با قواعد binding خودشان توصیف می‌شوند و الزاماً از JSON naming policy پیروی نمی‌کنند.
- نام component برای آرایه‌ها معتبرتر تولید می‌شود.
- چند `Produces` برای یک status code حفظ می‌شوند؛ محتوای متفاوت با media type جدا و در صورت لزوم schema ترکیبی توصیف می‌شود.
- attribute `[Obsolete]` به `deprecated: true` برای operation، schema و property تبدیل می‌شود.
- transformerهای OpenAPI می‌توانند مقدار تولیدی را برای API مشخص تغییر دهند.

## OpenAPI 3.2 پیش‌فرض

مستند تولیدی به‌صورت پیش‌فرض OpenAPI 3.2 است. اگر ابزار مصرف‌کننده نسخه قدیمی‌تری می‌خواهد، نسخه را صریحاً تنظیم کنید:

```csharp
builder.Services.AddOpenApi(options =>
{
    options.OpenApiVersion =
        Microsoft.OpenApi.OpenApiSpecVersion.OpenApi3_1;
});
```

این ارتقا، dependency مربوط به Microsoft.OpenApi را نیز تغییر می‌دهد و ممکن است breaking change ایجاد کند.

## SSE

برای اینکه OpenAPI schema هر event را درست تولید کند، endpoint باید از `TypedResults.ServerSentEvents(...)` استفاده کند:

```csharp
app.MapGet("/events", (CancellationToken cancellationToken) =>
    TypedResults.ServerSentEvents(
        GetEventsAsync(cancellationToken)));
```

## تولید build-time و محیط اجرا

`OpenApiGenerationEnvironment` در MSBuild محیط host هنگام تولید build-time سند را تعیین می‌کند؛ تنظیمات و transformerهای محیط‌محور در خروجی لحاظ می‌شوند.

## منابع

- [OpenAPI in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/overview)
- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
