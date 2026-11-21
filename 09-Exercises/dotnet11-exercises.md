# تمرین‌های .NET 11

# .NET 11 Exercises

## Runtime Async

هدف: تفاوت async compiler و runtime را اندازه‌گیری کنید.

1. یک endpoint به API .NET 10 و یک پروژه .NET 11 بسازید.
2. همان data size و server را استفاده کنید.
3. startup, p95, allocation و stack trace را مقایسه کنید.

خروجی مورد انتظار: گزارش benchmark با تعداد iterations، machine و تفاوت‌های قابل تکرار.

## Zstandard Compression

هدف: انتخاب algorithm compression.

ده فایل با محتوای تکراری و تصادفی ایجاد کنید و GZip، Brotli و Zstandard را مقایسه کنید.

خروجی مورد انتظار: اندازه خروجی، زمان compression/decompression و memory.

## Process APIs

هدف: اجرای process امن.

- `dotnet --version` را اجرا کنید
- stdout و stderr را جدا capture کنید
- exit status را بررسی کنید
- handle و process lifetime را آزاد کنید

## System.Text.Json

هدف: سازگار کردن JSON contract.

- یک union result را serialize کنید
- JSONL را تولید و parse کنید
- polymorphic document را تست کنید
- numeric converter را روی decimalهای بزرگ تست کنید

خروجی مورد انتظار: JSON سازگار و round-trip data.

## Async Validation

هدف: یکتایی Email.

یک `POST /users` بسازید که:

- email را normalize کند
- database را async بررسی کند
- CancellationToken را بفرستد
- unique constraint را در database داشته باشد
- race condition را بدون exception تبدیل کند

## OpenAPI 3.2

یک endpoint با binary، streaming و ProblemDetails بسازید و:

- OpenAPI 3.2 را تولید کنید
- media typeها را بررسی کنید
- Swagger UI یا Scalar را تست کنید

## EF Core FullJoin

دو `IQueryable` برای محصول و موجودی بسازید و unmatched records را حفظ کنید.

خروجی مورد انتظار: SQL با FULL OUTER JOIN روی provider ساپورت‌شده.

## SQL Server Vector Search

در صورت پشتیبانی:

- database، EF provider و SQL Server edition را بررسی کنید
- embedding را ذخیره کنید
- nearest neighbor را query کنید
- ANN را بدون تضمین exactness مستند کنید

## Native AOT

یک API کوچک را publish کنید.

- startup را مقایسه کنید
- reflection و JSON source generation را بررسی کنید
- native dependencies را تست کنید
- binary size و memory را گزارش کنید

## Performance Benchmarking

برای یک hot path، BenchmarkDotNet اجرا کنید:

- allocation
- p95/p99
- GC count
- cold start
- warm start

هر نتیجه با `.NET 10` و `.NET 11` مقایسه شود.

