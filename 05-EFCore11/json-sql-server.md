# JSON در SQL Server با EF Core 11

## JsonPathExists

`EF.Functions.JsonPathExists()` وجود یک JSON path را بررسی می‌کند:

```csharp
var blogs = await context.Blogs
    .Where(blog =>
        EF.Functions.JsonPathExists(blog.JsonData, "$.OptionalInt"))
    .ToListAsync();
```

روی SQL Server 2022 به این SQL ترجمه می‌شود:

```sql
WHERE JSON_PATH_EXISTS([b].[JsonData], N'$.OptionalInt') = 1
```

این API روی scalar string، complex type و owned entity mapped به JSON قابل استفاده است.

## JsonContains

SQL Server 2025 روی LINQ Contains برای primitive JSON collection می‌تواند از `JSON_CONTAINS` استفاده کند:

```csharp
protected override void OnConfiguring(
    DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder.UseSqlServer(
        "<CONNECTION STRING>",
        sql => sql.UseCompatibilityLevel(170));

var blogs = await context.Blogs
    .Where(blog => blog.Tags.Contains("ef-core"))
    .ToListAsync();
```

برای SQL Server قدیمی‌تر یا compatibility level پایین‌تر، EF ممکن است از `OPENJSON` استفاده کند.

## فراخوانی مستقیم JsonContains

```csharp
var blogs = await context.Blogs
    .Where(blog =>
        EF.Functions.JsonContains(
            blog.JsonData,
            8,
            "$.Rating") == 1)
    .ToListAsync();
```

## JSON Index

EF Core 11 می‌تواند JSON index روی مسیرهای complex type بسازد:

```csharp
modelBuilder.Entity<Customer>()
    .ComplexProperty(
        customer => customer.Contact,
        builder => builder.ToJson().HasColumnType("json"));

modelBuilder.Entity<Customer>()
    .HasIndex("Contact.Address.City");
```
