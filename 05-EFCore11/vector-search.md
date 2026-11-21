# SQL Server Vector Search در EF Core 11

# Vector Search

## مفاهیم

- **Vector**: بردار عددی که embedding را نمایش می‌دهد.
- **Embedding**: تبدیل متن، تصویر یا entity به اعداد برای semantic similarity.
- **Vector Search**: یافتن نزدیک‌ترین vectors در فضای embedding.
- **Vector Index**: ساختار index برای near-neighbor search.
- **ANN**: Approximate Nearest Neighbor؛ سرعت بالا با احتمال از دست دادن exact nearest.

## تفاوت با search متنی

جستجوی متنی معمولاً روی tokenها و full-text rules کار می‌کند. semantic search معنی نزدیک را پیدا می‌کند اما embedding و similarity threshold را می‌خواهد.

## وضعیت پشتیبانی

پشتیبانی SQL Server Vector و EF Core Provider را فقط در مستند رسمی و release notes بررسی کنید؛ این قابلیت ممکن است Experimental باشد یا به سرویس SQL Server خاص نیاز داشته باشد.

## مثال مفهومی

```csharp
public sealed class KnowledgeDocument
{
    public Guid Id { get; init; }
    public required string Text { get; init; }
    public required float[] Embedding { get; init; }
}
```

استفاده از `float[]` به provider یا SQL Server column mapping خاص بستگی دارد. پیش از اضافه کردن schema، SQL Server edition و EF package را verify کنید.

## سناریو RAG

1. متن query را به embedding تبدیل کنید.
2. vectorها را در SQL Server جستجو کنید.
3. documentهای نزدیک را به context مدل اضافه کنید.
4. پاسخ را همراه با citation نگه دارید.
5. reranking و hybrid search را در صورت نیاز انجام دهید.

## محدودیت‌ها

- Embedding model version را در metadata نگه دارید؛ نسخه‌های مختلف embeddingها قابل مقایسه نیستند.
- Vector index برای همه workloadها بهتر نیست.
- ANN ممکن است exact nearest را تضمین نکند.
- index tuning و upgrade روی databaseهای بزرگ باید migration plan داشته باشد.

## منابع

- [EF Core 11 what's new](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [SQL Server vector search documentation](https://learn.microsoft.com/en-us/sql/relational-databases/vectors/vectors)

