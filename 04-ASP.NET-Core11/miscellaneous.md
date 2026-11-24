# سایر تغییرات ASP.NET Core 11

## Output Cache

`IOutputCachePolicyProvider` امکان پیاده‌سازی provider سفارشی برای انتخاب policy پایه و resolve کردن policy نام‌دار را فراهم می‌کند؛ برای مثال policyهای tenant-specific یا بارگذاری‌شده از منبع پیکربندی.

## Development Certificate در WSL

در WSL اجرای زیر می‌تواند certificate توسعه را در WSL و Windows مورد اعتماد قرار دهد:

```bash
dotnet dev-certs https --trust
```

## OpenTelemetry

ASP.NET Core به activityهای HTTP server، semantic conventionهای OpenTelemetry اضافه می‌کند. برای دریافتشان به source داخلی subscribe کنید:

```csharp
builder.Services.AddOpenTelemetry()
    .WithTracing(tracing =>
        tracing.AddSource("Microsoft.AspNetCore"));
```

## Performance و Compression

- parser مربوط به HTTP/1.1 برای درخواست نامعتبر از مسیر non-throwing استفاده می‌کند.
- HTTP logging می‌تواند با pool کردن buffer streamها allocation کمتری داشته باشد.
- Zstandard برای response compression و request decompression پشتیبانی می‌شود.
- response compression همیشه `Vary: Accept-Encoding` اضافه می‌کند.
- HTTP/3 می‌تواند پردازش request را زودتر آغاز کند.
- بخش‌هایی از ASP.NET Core shared framework برای `net11.0+` با runtime-async ساخته می‌شوند.
- rate limiter مقدار دقیق‌تری برای `Retry-After` ارائه می‌کند.

نمونه تنظیم Zstandard:

```csharp
builder.Services.AddResponseCompression();
builder.Services.AddRequestDecompression();
builder.Services.Configure<ZstandardCompressionProviderOptions>(options =>
{
    options.CompressionOptions = new ZstandardCompressionOptions
    {
        Quality = 6
    };
});
```

## MCP Server Template

template ساخت MCP Server همراه SDK .NET ارائه می‌شود:

```powershell
dotnet new mcpserver -o MyMcpServer
```

## Kestrel، TLS و timeout

- اطلاعات exception مربوط به TLS handshake در `ITlsHandshakeFeature` قابل دسترسی است.
- برای مشاهده ClientHello از `UseTlsClientHelloListener` استفاده می‌شود؛ این middleware باید پیش از `UseHttps()` ثبت شود.
- `RequestHeadersTimeout` روی trailerهای fragmented در HTTP/2 و HTTP/3 نیز اعمال می‌شود.
- `ITlsConnectionFeature` می‌تواند TLS channel-binding token را ارائه دهد.
- تنظیم HTTP.sys برای Extended Protection و channel-binding token با `HttpAuthenticationHardeningLevel` کنترل می‌شود؛ `Strict` در صورت نبود token درخواست authenticated را رد می‌کند.

## منابع

- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [Kestrel web server](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel)
- [Rate limiting middleware](https://learn.microsoft.com/en-us/aspnet/core/performance/rate-limit)
