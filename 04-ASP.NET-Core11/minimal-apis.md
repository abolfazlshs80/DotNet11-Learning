# تغییرات Minimal API در ASP.NET Core 11

## Endpoint Filter و خطای Binding

اگر endpoint دارای filter باشد، pipeline فیلترها در صورت شکست parameter binding نیز اجرا می‌شود. در نتیجه filter می‌تواند status code برابر 400 را ببیند و پاسخ خطا را سفارشی کند.

در محیط Development، برای اینکه خطا به‌جای پرتاب `BadHttpRequestException` به filter برسد، `RouteHandlerOptions.ThrowOnBadRequest` را خاموش کنید.

برای درس فیلترها و نمونه‌های بیشتر، [endpoint-filters.md](endpoint-filters.md) را ببینید.

## C# Union Types

Union types در JSON bodyهای Minimal API و MVC قابل استفاده‌اند و schema مربوط به آن‌ها در OpenAPI به شکل `anyOf` توصیف می‌شود.

```csharp
public union ValueResult(int, string);

app.MapGet("/value", () => new ValueResult(42));
```

Union type برای binding مستقیم از route، query string، header یا form نیست. در JSON serialization چند protocol یا case هم‌شکل، محدودیت‌های framework را بررسی کنید.

## Asynchronous Validation

Minimal API validation از `AsyncValidationAttribute` و `IAsyncValidatableObject` پشتیبانی می‌کند. `AddValidation()` سرویس‌های validation را ثبت می‌کند. این امکان برای database lookup یا remote API مناسب است؛ `CancellationToken` را به عملیات I/O منتقل کنید.

```csharp
builder.Services.AddValidation();

app.MapPost("/reservations", (ReservationRequest request) =>
    Results.Ok(request));
```

همچنین async validation می‌تواند در Blazor فرم‌ها استفاده شود. برای مفهوم و توصیه‌های validation به [async-validation.md](async-validation.md) مراجعه کنید.

## Short-circuit Endpoint

ویژگی `[ShortCircuit]` endpoint را بلافاصله پس از routing اجرا می‌کند و بقیه middleware pipeline را رد می‌کند:

```csharp
app.MapGet("/health", [ShortCircuit] () => "Healthy");
```

از آن فقط برای endpointهایی استفاده کنید که واقعاً نیازی به middlewareهای ردشده ندارند. برای مثال، مطمئن شوید که با ردکردن authentication، authorization، CORS یا middlewareهای امنیتی سیاست مورد انتظار نقض نمی‌شود.

## Localization در Validation

`Microsoft.Extensions.Validation` می‌تواند پیام‌های validation و display nameها را با localization یکپارچه کند:

```csharp
builder.Services.AddLocalization();
builder.Services.AddValidation();
```

Resource keyها می‌توانند از metadata و attributeها استخراج شوند. همین قواعد بین Minimal API و Blazor مشترک‌اند؛ بنابراین یک مدل می‌تواند پیام یکسان و ترجمه‌شده‌ای در هر دو سطح ارائه دهد.

## منابع

- [Minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis)
- [Validation in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation)
- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
