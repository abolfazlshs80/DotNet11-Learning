# SQL Server Vector Search در EF Core 11

# Vector Search

## مفاهیم

- **Vector**: بردار عددی که embedding را نمایش می‌دهد.
- **Embedding**: تبدیل متن، تصویر یا entity به اعداد برای semantic similarity.
- **Vector Search**: یافتن نزدیک‌ترین vectorها در فضای embedding.
- **Vector Index**: ساختار index برای near-neighbor search.
- **ANN**: Approximate Nearest Neighbor؛ سرعت بالا با احتمال بازگرداندن مورد دقیق‌تر.

## وضعیت پشتیبانی

ویژگی‌های `VECTOR_SEARCH()` و vector index در SQL Server و EF Core experimental هستند و ممکن است API یا رفتار آن‌ها تغییر کند. SQL Server edition، compatibility level و EF provider را قبل از استفاده Production بررسی کنید.

## تعریف Vector

```csharp
public sealed class KnowledgeDocument
{
    public Guid Id { get; init; }
    public required string Text { get; init; }
    public required float[] Embedding { get; init; }
}
```

نوع mapping vector به EF provider و SQL Server بستگی دارد. از یک embedding model ثابت و نسخه‌گذاری‌شده برای index و query استفاده کنید.

## Vector Index

```csharp
modelBuilder.Entity<Blog>()
    .HasVectorIndex(blog => blog.Embedding, "cosine");
```

## جستجوی Exact و Approximate

```csharp
var exact = await context.Blogs
    .VectorSearch(
        blog => blog.Embedding,
        queryEmbedding,
        "cosine")
    .OrderBy(result => result.Distance)
    .Take(5)
    .ToListAsync();

var approximate = await context.Blogs
    .VectorSearch(
        blog => blog.Embedding,
        queryEmbedding,
        "cosine")
    .OrderBy(result => result.Distance)
    .Take(5)
    .WithApproximate()
    .ToListAsync();
```

بدون `WithApproximate()` جستجوی exact kNN اجرا می‌شود. با `WithApproximate()` SQL Server از vector index و ANN استفاده می‌کند و ممکن است exact nearest را تضمین نکند.

`VectorSearch()` خروجی `VectorSearchResult<TEntity>` دارد که هم entity و هم distance را ارائه می‌کند.

## بارگذاری Vector

EF Core 11 مقدار `SqlVector<T>` را به‌طور پیش‌فرض همراه entity انتخاب نمی‌کند، چون انتقال هزاران عددی float می‌تواند هزینه‌زا باشد.

```csharp
// Vector در این projection بارگذاری نمی‌شود
var blogs = await context.Blogs
    .OrderBy(blog => blog.Name)
    .ToListAsync();

// Vector به‌طور صریح خوانده می‌شود
var embeddings = await context.Blogs
    .Select(blog => new
    {
        blog.Id,
        blog.Embedding
    })
    .ToListAsync();
```

Vector همچنان در WHERE و ORDER BY قابل استفاده است.

## سناریو RAG

1. متن query را به embedding تبدیل کنید.
2. documentهای نزدیک را از SQL Server پیدا کنید.
3. نتایج را به context مدل اضافه کنید.
4. پاسخ را همراه citation نگه دارید.
5. reranking و hybrid search را در صورت نیاز انجام دهید.

## محدودیت‌ها

- Embedding modelهای متفاوت قابل مقایسه نیستند.
- Vector index برای همه workloadها بهینه نیست.
- ANN exact nearest را تضمین نمی‌کند.
- Tuning و migration روی databaseهای بزرگ باید برنامه داشته باشد.

## منابع

- [EF Core 11 what's new](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [SQL Server vector search documentation](https://learn.microsoft.com/en-us/sql/relational-databases/vectors/vectors)
