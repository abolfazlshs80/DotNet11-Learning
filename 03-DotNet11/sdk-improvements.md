# SDK و Developer Productivity

# SDK Improvements

## MSBuild Server

MSBuild Server فرآیندهای build را برای کاهش startup و استفاده مجدد از state بهبود می‌دهد. فایل‌های solution و project را از طریق ساخت واقعی تست کنید.

```powershell
dotnet build DotNet11.OrderManagement.sln
dotnet msbuild -graphBuild DotNet11.OrderManagement.sln
```

برای پاک‌سازی server و debuggerهای build به documentation آن نسخه مراجعه کنید؛ خروجی پایدار را با Clean Build و Incremental Build جدا کنید.

## dotnet test

.NET 11 dotnet test improvements شامل:

- `--no-dependencies`
- `--use-current-runtime`
- `--test-modules`
- timeout و حداکثر تست‌های fail‌شده
- live display و JSON output

```powershell
dotnet test DotNet11.OrderManagement.sln --use-current-runtime --timeout 00:10:00
```

## Solution Filters

فایل `*.slnf` برای build و تست مجموعه کوچکی از پروژه‌ها مفید است.

```json
{
  "solution": {
    "path": "DotNet11.OrderManagement.sln",
    "projects": [
      "src/OrderManagement.Api/OrderManagement.Api.csproj",
      "src/OrderManagement.Application/OrderManagement.Application.csproj"
    ]
  }
}
```

فرمت دقیق را با template SDK خود بررسی کنید.

## File-based Apps

فایل‌های تک‌فایل برای scriptهای کوچک، sampleها و ابزارهای داخلی مفیدند؛ برای برنامه‌های production چندسایز، ساختار پروژه از خوانایی و dependency management بهتر بهره می‌برد.

```powershell
dotnet run app.cs
```

دستور پشتیبانی بسته به SDK final و preview feature متفاوت است؛ ابتدا `dotnet run --help` را بررسی کنید.

## Container Publish

```powershell
dotnet publish src/OrderManagement.Api -c Release -r linux-x64 --output publish
```

پشتیبانی runtime-native و reproducible digestها به Docker/Podman، OS host و SDK/version وابسته است.

## منابع

- [.NET 11 SDK](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/sdk)
- [dotnet test](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-test)
- [dotnet publish](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)

