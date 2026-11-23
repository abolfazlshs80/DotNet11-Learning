# EF Core 11 و Azure Cosmos DB

## Complex Types

EF Core 11 complex types را در Cosmos به nested JSON object یا array نگاشت می‌کند:

```csharp
modelBuilder.Entity<Order>()
    .ComplexProperty(order => order.ShippingAddress);
```

Complex type نسبت به owned type بهتر value semantics دارد چون identity ندارد.

## Indexing

می‌توان indexing policy را تنظیم کرد، property و composite index اضافه کرد و automatic indexing را کنترل کرد:

```csharp
modelBuilder.Entity<Order>(builder =>
{
    builder.HasAutomaticIndexing()
        .Except("/InternalNotes/?");

    builder.HasIndex(order => new
    {
        order.CustomerId,
        order.OrderDate
    });
});
```

## Transactional Batches

روپ operations با container و partition گروه‌بندی می‌شوند. رفتار از طریق `AutoTransactionBehavior` تنظیم می‌شود:

| مقدار | رفتار |
|---|---|
| `Auto` | پیش‌فرض؛ عملیات را به batchهای transactional تقسیم می‌کند |
| `Never` | عملیات را فردی و sequential اجرا می‌کند |
| `Always` | همه عملیات را در یک batch قرار می‌دهد و در صورت شکست خطا می‌دهد |

## Bulk Execution

```csharp
protected override void OnConfiguring(
    DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder.UseCosmos(
        "<CONNECTION STRING>",
        "OrdersDB",
        cosmos => cosmos.BulkExecutionEnabled());
```

## Session Token

برای read-your-writes در محیط چند instance از session token استفاده کنید:

```csharp
var sessionToken = context.Database.GetSessionToken();

context.Database.UseSessionToken(sessionToken);
var document = await context.Documents.FindAsync(id);
```

Manual management با `SessionTokenManagementMode.SemiAutomatic` فعال می‌شود.

## JSON Serializer

EF Core 11 از `System.Text.Json` به‌جای Newtonsoft.Json استفاده می‌کند.

`JObject` shadow property حذف شده و unmapped JSON properties روی round-trip حفظ نمی‌شوند. این تغییر یک breaking change است.

## Undefined در Projection

EF Core 11 هنگام undefined شدن بخشی از projection یک `InvalidOperationException` می‌دهد.

```csharp
var results = await context.Entities
    .Where(entity =>
        EF.Functions.IsDefined(
            entity.Associate!.NestedAssociate!.Id))
    .Select(entity => new
    {
        entity.Associate!.NestedAssociate!.Id
    })
    .ToListAsync();
```
