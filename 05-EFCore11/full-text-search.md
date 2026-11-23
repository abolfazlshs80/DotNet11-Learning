# Full-Text و Hybrid Search در EF Core 11

SQL Server full-text search می‌تواند در EF Core migration و LINQ تنظیم شود.

## Full-Text Catalog و Index

```csharp
modelBuilder.HasFullTextCatalog("ftCatalog");

modelBuilder.Entity<Blog>()
    .HasFullTextIndex(blog => blog.FullName)
    .UseKeyIndex("PK_Blogs")
    .UseCatalog("ftCatalog");
```

این model در migration SQL زیر را تولید می‌کند:

```sql
CREATE FULLTEXT CATALOG [ftCatalog];

CREATE FULLTEXT INDEX ON [Blogs]([FullName])
KEY INDEX [PK_Blogs] ON [ftCatalog];
```

## FreeTextTable

جدول‌محور full-text علاوه بر entity، ranking را برمی‌گرداند:

```csharp
var results = await context.Blogs
    .FreeTextTable("John", blog => blog.FullName)
    .Select(result => new
    {
        Blog = result.Value,
        result.Rank
    })
    .OrderByDescending(result => result.Rank)
    .ToListAsync();
```

`ContainsTable()` نیز پشتیبانی می‌شود و هر دو تابع خروجی
`FullTextSearchResult<TEntity>` دارند.

## Hybrid Search

Full-text تطابق لغوی و vector search شباهت معنایی را پیدا می‌کنند. با `FullJoin` می‌توان هر دو مجموعه نتیجه را ترکیب کرد:

```csharp
var results = await context.Articles
    .FreeTextTable<Article, int>(textQuery, topN: 20)
    .Join(
        context.Articles,
        fullText => fullText.Key,
        article => article.Id,
        (fullText, article) => new
        {
            Article = article,
            fullText.Rank
        })
    .FullJoin(
        context.Articles
            .VectorSearch(
                article => article.Embedding,
                queryEmbedding,
                "cosine")
            .Take(20)
            .WithApproximate(),
        fullText => fullText.Article.Id,
        vector => vector.Value.Id,
        (fullText, vector) => new
        {
            Article = fullText != null
                ? fullText.Article
                : vector.Value,
            FullTextRank = fullText == null
                ? null
                : (int?)fullText.Rank,
            VectorDistance = vector == null
                ? null
                : (double?)vector.Distance
        })
    .ToListAsync();
```

در production rankها، normalization، reranking و fallback را روی داده واقعی تست کنید.
