# Complex Types در EF Core 11

Complex type داده‌های nested و structured را با value semantics نگه می‌دارد. این قابلیت برای inheritanceهای TPT و TPC و Azure Cosmos DB نیز تکمیل شده است.

## Entity با Complex Type

```csharp
public abstract class Animal
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public required AnimalDetails Details { get; set; }
}

public class Dog : Animal
{
    public required string Breed { get; set; }
}

[ComplexType]
public sealed class AnimalDetails
{
    public DateTime BirthDate { get; init; }
    public string? Veterinarian { get; init; }
}
```

## TPT و TPC

EF Core 11 complex type و JSON column را روی entityهای TPT و TPC پشتیبانی می‌کند:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Animal>().UseTptMappingStrategy();

    modelBuilder.Entity<Animal>()
        .ComplexProperty(animal => animal.Details, b => b.ToJson());
}
```

## پیکربندی Propertyهای Nested

در EF Core 11 propertyهای nested را مستقیماً از lambda تنظیم کنید:

```csharp
modelBuilder.Entity<MyEntity>()
    .Property(entity => entity.Details.Description)
    .HasMaxLength(500);
```

قبل از EF Core 11 معمولاً ابتدا باید `ComplexProperty` فراخوانی می‌شد.

## Key و Index روی Nested Property

```csharp
modelBuilder.Entity<Customer>()
    .HasKey(customer => customer.CustomerId.Value);

modelBuilder.Entity<Customer>()
    .HasIndex(customer => customer.Address.PostalCode);

modelBuilder.Entity<Customer>()
    .HasIndex("Address.PostalCode");
```

برای complex collection mapped به JSON می‌توان از مسیرهای array استفاده کرد:

```csharp
modelBuilder.Entity<Order>()
    .ComplexCollection(order => order.Items, b => b.ToJson());

modelBuilder.Entity<Order>()
    .HasIndex("Items[].Sku");
```

## ویژگی‌های یکسان

- Complex type identity ندارد.
- به‌جای owned entity برای value object مناسب‌تر است.
- Nested objectهای JSON و value collection در Cosmos پشتیبانی می‌شوند.
- تغییرات در value nested باید با conventionهای tracking و ExecuteUpdate بررسی شود.
