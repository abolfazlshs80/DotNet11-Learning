# بهبود LINQ و SQL Translation در EF Core 11

EF Core 11 چند بهینه‌سازی برای حذف joinها، castها و محاسبات ناخواسته روی SQL تولیدی اضافه می‌کند.

## Joinهای to-one

در includeهای collection با split query، EF Core joinهای reference navigation اضافه را حذف می‌کند.

```csharp
var blogs = context.Blogs
    .Include(blog => blog.BlogType)
    .Include(blog => blog.Posts)
    .AsSplitQuery()
    .ToList();
```

EF همچنین کلید اضافه reference navigation را از `ORDER BY` حذف می‌کند، چون کلید والد از foreign key قابل تعیین است.

## GroupBy

EF Core 11 GroupBy را در این سناریوها بهتر translate می‌کند:

- navigation و join روی top result هر گروه
- aggregate روی reference navigation با join به‌جای correlated subquery
- anonymous type و DTO روی left join

## No-op CASTها

EF Core castهای بی‌اثر را روی SQL حذف می‌کند:

```sql
-- قبل
WHERE CAST([u].[Name] AS nvarchar(max)) LIKE N'Name%'

-- بعد از EF Core 11
WHERE [u].[Name] LIKE N'Name%'
```

این کار مقدار SQL و امکان استفاده از index را بهبود می‌دهد.

## MaxBy و MinBy

EF Core روش‌های sync و async این عملگرها را translate می‌کند:

```csharp
var blogWithMostPosts = await context.Blogs
    .MaxByAsync(blog => blog.Posts.Count());

var oldestPost = await context.Posts
    .MinByAsync(post => post.PublishedAt);
```

## بررسی خروجی SQL

برای اعتبارسنجی، SQL را روی provider واقعی بررسی کنید:

```csharp
string sql = query.ToQueryString();
Console.WriteLine(sql);
```
