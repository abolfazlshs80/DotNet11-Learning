# Runtime و Performance Improvements

# Runtime and Performance

## JIT و Optimization

JIT کدهای IL را به machine code تبدیل می‌کند و tiered compilation را برای warm start و hot methods اعمال می‌کند. Native AOT از زمان اجرا JIT حذف می‌کند اما با محدودیت‌های reflection و dynamic loading همراه است.

## زمینه‌های بهبود در .NET 11

مستندات رسمی مواردی مانند Runtime Async، interface dispatch، startup، code size و JIT را ذکر می‌کند. بهبود هر workload را قبل از معرفی در مستند محصول یا release benchmark بررسی کنید.

## BenchmarkDotNet

```xml
<PackageReference Include="BenchmarkDotNet" Version="*" />
```

جایگزین `*` را در پروژه واقعی خودکار نکنید؛ version سازگار را pin کنید.

```csharp
using BenchmarkDotNet.Attributes;

[MemoryDiagnoser]
public class SerializationBenchmark
{
    [Benchmark(Baseline = true)]
    public int Baseline() => 42;

    [Benchmark]
    public int NewPath() => 42;
}
```

اعداد واقعی نتیجه اجرای ماشین شما هستند.

## Native AOT

```xml
<PublishAot>true</PublishAot>
```

مزایا: startup پایین، native executable، کوچک‌تر بودن runtime dependency. محدودیت‌ها: reflection trimming، platform-specific publish، اندازه binary و dependency compatibility.

## GC و allocation

- از allocationهای غیرضروری در hot path جلوگیری کنید.
- برای I/O از buffer reuse و `Memory<T>` استفاده کنید.
- `Span<T>` را به call stack و synchronous paths محدود کنید مگر lifecycle آن مشخص باشد.
- GC را بدون داده production tune نکنید.

## مقایسه قبل و بعد ارتقا

```text
Scenario .NET 10 -> .NET 11
Machine: fixed
SDK: fixed for each run
Data size: fixed
Warm-up: fixed
Iterations: fixed
Latency: p50/p95/p99
Allocation: bytes/request
Startup: process start to first request
```

نتیجه را فقط پس از چند iteration، monitoring host و نتایج قابل تکرار اعلام کنید.

## منابع

- [.NET 11 runtime overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/runtime)
- [BenchmarkDotNet](https://benchmarkdotnet.org/)
- [Native AOT](https://learn.microsoft.com/en-us/dotnet/core/deploying/native-aot/)

