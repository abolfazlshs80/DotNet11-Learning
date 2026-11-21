# Entity Framework Core 11

# EF Core 11 Overview

## بهبودهای عمده

مستندهای EF Core 11 مواردی مثل improved LINQ translation، reference navigation، GroupBy، FullJoin و قابلیت‌های SQL Server Vector را معرفی می‌کنند.

## شرایط واقعی Provider

هر بهبود ممکن است فقط برای provider خاص یا نسخه server خاص باشد. برای SQL Server، نسخه Engine و EF provider را جداگانه کنترل کنید.

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="11.0.*" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="11.0.*" />
```

هر نسخه package را با TFM و SDK نهایی pin کنید؛ wildcard نسخه در CI برای release استفاده نشود.

## Reference Navigation و LINQ

EF Core نباید lazy loading یا client-side translation را پنهان کند. تمام queryها را روی دیتابیس تحلیل کنید و از `ToQueryString()` برای SQL بررسی استفاده کنید.

```csharp
var query = db.Orders
    .Include(x => x.Lines)
    .Where(x => x.Status == OrderStatus.Pending);

Console.WriteLine(query.ToQueryString());
```

## Migration

قبل از migration:

```powershell
dotnet ef migrations add AddOrderSchema
dotnet ef migrations script
```

SQL migration را در staging اجرا کنید، deploy strategy را تست کنید و rollback plan داشته باشید.

## منابع

- [EF Core 11 what's new](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [EF Core LINQ translations](https://learn.microsoft.com/en-us/ef/core/querying/sql)

