# FullJoin در EF Core 11

# LINQ FullJoin

Full Join همه رکوردهای دو مجموعه را نگه می‌دارد. اگر در هر طرف match باشد، ستون‌ها ترکیب می‌شوند؛ رکورد unmatched هم با طرف مقابل null نمایش داده می‌شود.

| Join | رکوردهای طرف چپ | رکوردهای طرف راست |
|---|---|---|
| Inner | فقط match | فقط match |
| Left | همه | matchها و null |
| Right | matchها و null | همه |
| Full | همه | همه |

## مثال LINQ

```csharp
var inventory = new[]
{
    new { Sku = "A1", Stock = 10 },
    new { Sku = "B2", Stock = 4 }
};

var incoming = new[]
{
    new { Sku = "A1", Requested = 3 },
    new { Sku = "C3", Requested = 8 }
};

var result = inventory.FullJoin(
    incoming,
    left => left.Sku,
    right => right.Sku,
    (left, right) => new
    {
        left?.Sku ?? right!.Sku,
        left?.Stock,
        right?.Requested
    });
```

در EF، دو مجموعه باید از IQueryable باشند و provider باید translation را ساپورت کند.

```csharp
var result = await db.Products
    .FullJoin(
        db.StockReservations,
        product => product.Sku,
        reservation => reservation.Sku,
        (product, reservation) => new
        {
            Sku = product!.Sku ?? reservation!.Sku,
            product!.Name,
            reservation!.Quantity
        })
    .ToListAsync();
```

این نمونه مفهومی است و signatures دقیق provider را بررسی کنید.

## SQL تولیدی

provider ممکن است از `FULL OUTER JOIN` استفاده کند:

```sql
SELECT ...
FROM Products AS p
FULL OUTER JOIN StockReservations AS r
    ON p.Sku = r.Sku;
```

از ToQueryString برای verify استفاده کنید. SQL Server، PostgreSQL و SQLite ترجمه‌ها و fallbackها را متفاوت پشتیبانی می‌کنند.

## منابع

- [EF Core 11 FullJoin](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)
- [EF Core SQL Server provider](https://learn.microsoft.com/en-us/ef/core/providers/sql-server/)

