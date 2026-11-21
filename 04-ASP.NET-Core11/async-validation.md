# Asynchronous Validation

# ASP.NET Core 11 Async Validation

## چرا async validation؟

بررسی range یا required field sync است؛ یکتایی Email، موجودی محصول و policyهای external به I/O نیاز دارند.

## روش‌های ASP.NET Core

با توجه به release ASP.NET Core 11:

- `AsyncValidationAttribute`
- `IAsyncValidatableObject`
- `Validator.ValidateObjectAsync`
- `AddValidation`

نوع دقیق APIها و DI registration در release notes را بررسی کنید؛ APIهای پیشنهادی را در runtime final verify کنید.

## مدل ثبت‌نام

```csharp
public sealed class RegisterUserRequest
{
    public required string Email { get; init; }
    public required string Password { get; init; }
}

public interface IUserStore
{
    Task<bool> EmailExistsAsync(string email, CancellationToken cancellationToken);
}

public sealed class RegisterUserValidator(IUserStore store)
    : IAsyncValidatableObject
{
    public async Task<IEnumerable<ValidationResult>> ValidateAsync(
        object value,
        ValidationContext context,
        CancellationToken cancellationToken)
    {
        if (value is not RegisterUserRequest request)
            yield break;

        if (await store.EmailExistsAsync(request.Email, cancellationToken))
        {
            yield return new ValidationResult(
                "Email is already registered.",
                [nameof(request.Email)]);
        }
    }
}
```

این کد ساختار مفهومی IAsyncValidatableObject را نشان می‌دهد؛ exact signature را در مستند ASP.NET Core 11 و کلاس‌های framework بررسی کنید.

## Minimal API

در endpointهای ثبت‌نام، validation را در schema یا قبل از use case صریح نگه دارید و cancellation را از `HttpContext.RequestAborted` انتقال دهید.

```csharp
app.MapPost("/users", async (
    RegisterUserRequest request,
    IUserStore store,
    CancellationToken cancellationToken) =>
{
    if (await store.EmailExistsAsync(request.Email, cancellationToken))
        return Results.ValidationProblem(new Dictionary<string, string[]>
        {
            ["Email"] = ["Email is already registered."]
        });

    return Results.Created("/users/new-id", request);
});
```

## نکات

- ایمیل را trim و normalize کنید اما domain rule را در model مشخص نگه دارید.
- HTTP cancellation را به database query پاس دهید.
- race condition بعد از validation ممکن است؛ database unique constraint را لازم نگه دارید.
- remote serviceها timeout، retry و circuit breaker می‌خواهند.

## منابع

- [ASP.NET Core 11 release notes](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [Data validation](https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation)

