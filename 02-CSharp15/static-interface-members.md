# Static Interface Members در Runtimeهای قدیمی

# Non-Virtual Static Interface Members on Older Runtimes

## Static Members در Interface

Interfaceها می‌توانند:

- Instance members داشته باشند.
- Static members implementation فراهم کنند.
- Static abstract/virtual برای generic constraints داشته باشند.

```csharp
public interface IParseable<T>
{
    static abstract T Parse(string value);
}
```

`static abstract` به generic code اجازه می‌دهد عملگر یا factory را از type parameter فراخوانی کند.

## Non-Virtual Static Member

Members static implementation می‌توانند از نوع interface استفاده شوند و به override وابسته نباشند:

```csharp
public interface ILoggerDefaults
{
    static string DefaultCategory => "Application";
}

Console.WriteLine(ILoggerDefaults.DefaultCategory);
```

این نوع member با virtual dispatch ترکیب نمی‌شود.

## سازگاری Runtimeهای قدیمی

قابلیت‌ها ممکن است در کتابخانه‌ای پیشرفته‌تر روی Runtime قدیمی اجرا شوند، با این شرط که IL و member نوعی برای Runtime هدف سازگار باشد. همه static abstract/virtual features لزوماً روی runtimeهای قدیمی قابل emulate یا پشتیبانی نیستند.

مرجع نهایی در مستند رسمی C# 15 و runtime compatibility است؛ استفاده نکنید که فقط یک feature با نام مشابه وجود دارد، رفتار کامل همان را تضمین می‌کند.

## محدودیت‌ها

- Static member با implementation روی interface جایگزین service یا DI نیست.
- برای hookهای polymorphic از static abstract/virtual استفاده کنید.
- برای ثابت‌ها یا utilityهای بدون dispatch، static member روی interface یا کلاس utility را انتخاب کنید.
- Libraryهای چند-Target Framework باید compatibility matrix اجرا کنند.

## نمونه Library

```csharp
public interface IIdentifierRules
{
    static bool IsEmpty(string? value) =>
        string.IsNullOrWhiteSpace(value);
}

Console.WriteLine(IIdentifierRules.IsEmpty(" "));
```

## منابع

- [C# 15 static interface compatibility](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15)
- [Static abstract interface members](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-11.0/static-abstracts-in-interfaces)

