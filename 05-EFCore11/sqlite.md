# بهبود SQLite در EF Core 11

## Ordering در String Aggregation

EF Core می‌تواند `string.Join` و `string.Concat` را روی grouping به `group_concat` ترجمه کند.

با SQLite 3.44 یا جدیدتر می‌توان ترتیب داخل aggregate را تعیین کرد:

```csharp
var blogs = await context.Blogs
    .Select(blog => new
    {
        blog.Name,
        Posts = string.Join(
            ", ",
            blog.Posts
                .OrderByDescending(post => post.Id)
                .Select(post => post.Title))
    })
    .ToListAsync();
```

SQL نتیجه از `group_concat` همراه `ORDER BY` استفاده می‌کند.

## UInt128

Microsoft.Data.Sqlite می‌تواند مقدار `UInt128` را به‌عنوان متن zero-padded 39 رقمی bind کند تا order و comparison حفظ شود.

خواندن `UInt128` از data reader در نسخه فعلی پشتیبانی نشده است.
