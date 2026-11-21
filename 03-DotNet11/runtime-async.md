# Runtime Async

# .NET 11 Runtime Async

## Async در مدل قبلی

در مدل compiler-based، کامپایلر یک async method را به state machine تبدیل می‌کند. این state machine استمرار (continuation)، return type و exception handling را مدیریت می‌کند.

```csharp
public static async Task<int> LoadAsync(HttpClient client)
{
    using var response = await client.GetAsync("/api/orders");
    return (int)response.StatusCode;
}
```

## Runtime Async

در .NET 11، Runtime Async اجرای Task-returning methods را در لایه Runtime توسعه می‌دهد. مستندات رسمی می‌گویند Runtime Async برای پروژه‌های `net11.0` دیگر نیاز به `EnablePreviewFeatures=true` ندارد و Runtime libraryها با runtime-async=on ساخته شده‌اند.

بهبودهای رسمی شامل مواردی است مثل:

- continuationهای باقیمانده بدون capture اضافی اگر ExecutionContext محیطی در حالت غیرضروری باشد
- tiered compilation و intrinsicهای Task/ValueTask
- کاهش warm-up allocation و tail-merged suspension points
- stack traceهای قابل خواندن‌تر

این بهبودها برای همه workloadها تضمین نتیجه یکسان نمی‌کنند.

## اندازه‌گیری

Benchmark را فقط روی برنامه واقعی اجرا کنید:

```csharp
using BenchmarkDotNet.Attributes;

[MemoryDiagnoser]
public class HttpAsyncBenchmarks
{
    [Benchmark]
    public Task<int> GetStatusAsync()
    {
        return Task.FromResult(200);
    }
}
```

برای بار واقعی، از Local HTTP server و درخواستهای مشابه قبل و بعد از ارتقا استفاده کنید؛ GC, allocation و latency را جداگانه گزارش کنید.

## محدودیت‌ها

- Runtime async یک جایگزین برای async patterns نامناسب نیست.
- .NET 11 را با .NET 10 مقایسه کنید و فقط تغییر target framework را در benchmark کنترل کنید.
- عوامل CPU، I/O، `SynchronizationContext` و environment را ثابت نگه دارید.
- stack traceها ممکن است تغییر کنند؛ tooling یا parserهای پارس‌شده را بررسی کنید.

## جمع‌بندی

Runtime Async قابلیت اجرایی است نه فقط Syntax C#. مزایای آن وابسته به workload است و بهبودهای واقعی باید با benchmark قابل تکرار سنجیده شوند.

## تمرین

برای یک endpoint ثبت سفارش، قبل و بعد از ارتقا:

- درخواستهای p50/p95/p99 را ثبت کنید
- allocationهای GC را مقایسه کنید
- تعداد context switchها را بدون ابزار host بررسی نکنید؛ از ابزارهای profiler استفاده کنید

## منابع

- [What's new in .NET 11 runtime](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/runtime)
- [.NET 11 overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/overview)

