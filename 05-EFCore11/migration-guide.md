# راهنمای Migration به EF Core 11

EF Core 11 نیازمند .NET 11 SDK و runtime است و روی نسخه‌های قدیمی‌تر .NET اجرا نمی‌شود.

## قبل از ارتقا

1. همه packageهای EF Core را هم‌نسخه و سازگار کنید.
2. SQL Server edition و compatibility level را بررسی کنید.
3. model snapshot و migrationهای pending را پاک نگه دارید.
4. تست‌های query، save، migration و Cosmos را روی runtime 11 اجرا کنید.
5. breaking changes Cosmos، JSON، temporal و migration tooling را بررسی کنید.
6. SQL تولیدی و execution plan را روی workload واقعی تحلیل کنید.

## چک‌لیست Migration

```powershell
dotnet --info
dotnet ef migrations add UpgradeToEf11
dotnet ef migrations script
```

SQL migration را روی staging اجرا کنید، rollback plan داشته باشید و قبل از deploy توانایی بازیابی را تست کنید.

## منابع

- [EF Core 11 what's new](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [EF Core migrations](https://learn.microsoft.com/en-us/ef/core/managing-schemas/migrations/)
