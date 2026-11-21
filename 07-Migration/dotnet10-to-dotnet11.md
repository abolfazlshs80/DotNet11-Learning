# مهاجرت از .NET 10 به .NET 11

# .NET 10 to .NET 11 Migration

## مناسب برای مهاجرت

- تیم روی SDK و CI سازگار است
- Providerهای database و packageها نسخه NET 11 را پشتیبانی می‌کنند
- ماشین‌آلات Production runtime 11 را ساپورت می‌کنند
- تستهای Integration و Performance اجرا شده‌اند

## بهتر روی .NET 10 باقی بمانند

- NuGetهای critical هنوز سازگار نیستند
- SQL provider از NET11 پشتیبانی ندارد
- نیازمند LTS و پشتیبانی قانونی کامل هستید و timeline version نامطمئن است
- Production infra فقط runtime قدیمی را ارائه می‌دهد

## مراحل مهاجرت

1. یک branch و target framework تغییر دهید.
2. packageها را به compatible .NET 11 version ارتقا دهید.
3. `dotnet restore` و build را اجرا کنید.
4. breaking changes را بررسی کنید.
5. unit و integration tests را اجرا کنید.
6. migrationهای EF را در staging تست کنید.
7. benchmarkهای p50/p95/p99 و startup را انجام دهید.
8. rollout و rollback را برنامه‌ریزی کنید.

## نمونه تغییر TFM

```xml
<TargetFramework>net11.0</TargetFramework>
```

همه پروژه‌ها را همزمان ارتقا دهید؛ یک Solution با Targetهای مختلف نیز ممکن است اما dependency risk دارد.

## تست

```powershell
dotnet test DotNet11.OrderManagement.sln --configuration Release
dotnet publish src/OrderManagement.Api -c Release
```

## Rollback

- سابقه image/container را نگه دارید
- database migrationها را reversible کنید
- config feature flags را حفظ کنید
- data migrationها را با backup/restore آزموده‌شده انجام دهید

## منابع

- [.NET 11 migration](https://learn.microsoft.com/en-us/dotnet/core/porting/)
- [Breaking changes .NET 11](https://learn.microsoft.com/en-us/dotnet/core/compatibility/11.0)

