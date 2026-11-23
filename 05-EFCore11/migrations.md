# بهبود Migration در EF Core 11

## Exclude Foreign Key از Migration

رابطه در EF نگه داشته می‌شود اما database constraint ساخته نمی‌شود:

```csharp
modelBuilder.Entity<Blog>()
    .HasMany(blog => blog.Posts)
    .WithOne(post => post.Blog)
    .HasForeignKey(post => post.BlogId)
    .ExcludeForeignKeyFromMigrations();
```

## Latest Migration در Snapshot

Model snapshot شناسه آخرین migration را نگه می‌دارد تا divergence بین branchها از طریق merge conflict مشخص شود.

## Create و Apply در یک گام

```powershell
dotnet ef database update InitialCreate --add
```

این command migration را scaffold و apply می‌کند.

## Connection و Offline

```powershell
dotnet ef migrations remove --connection "Server=prod;Database=MyDb;"
dotnet ef migrations remove --offline
dotnet ef migrations remove --force
dotnet ef database drop --connection "Server=test;Database=MyDb;" --force
```

گزینه‌های `--offline` و `--force` با هم قابل استفاده نیستند.

## تنظیمات dotnet ef

فایل `.config/dotnet-ef.json` مقادیر پیش‌فرض را نگه می‌دارد:

```json
{
  "project": "src/App.Infrastructure",
  "startupProject": "src/App.Api",
  "framework": "net11.0",
  "configuration": "Debug",
  "context": "AppDbContext",
  "runtime": "win-x64",
  "verbose": true,
  "noColor": false,
  "prefixOutput": false
}
```

Optionهای command-line اولویت دارند.

## -NoBuild و Multiple Context

```powershell
Add-Migration MyMigration -NoBuild
Update-Database -NoBuild
dotnet ef migrations list --context "*"
```

فقط روی پروژه buildشده و به‌روز از `-NoBuild` استفاده کنید.
