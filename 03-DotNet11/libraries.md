# کتابخانه‌های جدید .NET 11

# .NET 11 Libraries

این فصل APIهای منتشرشده یا بهبود یافته در مستند رسمی را دسته‌بندی می‌کند.

## Zstandard Compression

Zstandard برای payloadهای تکراری و compression ratio بهتر مفید است؛ GZip برای سازگاری گسترده و Brotli برای HTTP responseها سناریوهای رایج دارند.

```csharp
using System.IO.Compression;

byte[] original = [1, 1, 1, 2, 2, 3];
await using var output = new MemoryStream();
await using (var zstd = new ZLibStream(output, CompressionLevel.SmallestSize, leaveOpen: true))
{
    await zstd.WriteAsync(original);
}
```

اسم class Zstandard بستگی به API رسمی N11 دارد؛ قبل از کد Production مستند namespace و enum .NET 11 را بررسی کنید.

## Process APIs

APIهای جدید Process شامل run/capture، lifecycle handleها، suspended start، safe lookup، signal و exit status است. برای workerها capture و handle lifecycle را صریح کنید.

```csharp
var startInfo = new ProcessStartInfo("dotnet")
{
    ArgumentList = { "--version" },
    RedirectStandardOutput = true
};

using var process = Process.Start(startInfo)
    ?? throw new InvalidOperationException();

string output = await process.StandardOutput.ReadToEndAsync();
await process.WaitForExitAsync();
Console.WriteLine(output);
```

## System.Text.Json

بهبودهای .NET 11 ممکن است شامل union, JSONL, polymorphism و numeric converterها باشند. اینها در همه targetها و packageها یکسان نیستند.

```csharp
var json = JsonSerializer.Serialize(new
{
    OrderId = Guid.NewGuid(),
    Status = "Pending"
});
```

برای JSONL streamها، چند JSON object را در خطوط جدا بنویسید؛ استفاده از trailing comma یا یک آرایه JSON اشتباه است.

## Numeric APIs

.NET 11 numeric types شامل Decimal32/64/128 و generic APIs دقیق‌تر می‌شوند. از اینها برای scientific/financial modelها فقط پس از بررسی precision، round mode و provider استفاده کنید. تغییر از decimal/long معمولاً هزینه serialization و interop دارد.

## Memory APIs

- `Memory<T>` و `ReadOnlyMemory<T>` برای viewهای managed/unmanaged بدون copy
- `ReadOnlySequence<T>` برای segmentهای چندبخشی
- ReadOnlyMemoryStream و WritableMemoryStream برای wrap کردن bufferها
- StringStream برای داده رشته‌ای

APIهای نامبرده در مقاله .NET 11 را با مستند رسمی .NET API verify کنید؛ nameها یا release status ممکن است تغییر کند.

## سایر APIها

ZIP, CRC32, Deflate/GZip/ZLib reset, Base64, Tar, DNS و TLS بهبودهای lifecycle, validation و performance دارند:

```csharp
var crc = new System.IO.Hashing.Crc32();
crc.Append("hello"u8);
Console.WriteLine(Convert.ToHexString(crc.GetCurrentHash()));
```

پکیج `System.IO.Hashing` قبلاً جدا بوده است؛ فقط برای نسخه‌هایی که namespace عضو .NET 11 نیست، package را اضافه کنید.

## منابع

- [.NET 11 libraries overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/libraries)
- [System.IO.Compression](https://learn.microsoft.com/en-us/dotnet/api/system.io.compression)
- [System.Diagnostics.Process](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.process)

