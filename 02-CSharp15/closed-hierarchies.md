# Closed Hierarchies

# C# 15 Closed Hierarchies

## تعریف

Closed Hierarchy یک base type است که فقط می‌تواند در assembly اعلام‌کننده مشتق شود. مجموعه فرزندان مستقیم در زمان کامپایل ثابت می‌شود.

```csharp
public closed record class OrderState;

public record class Pending : OrderState;
public record class Completed : OrderState;
public record class Cancelled : OrderState;
```

کلاس‌های دیگر خارج از assembly فعلی نمی‌توانند از `OrderState` مشتق شوند.

## تفاوت با `sealed`

| ویژگی | sealed | closed |
|---|---|---|
| مشتق‌سازی | کاملاً ممنوع | فقط در assembly اعلام‌کننده |
| فرزندان داخلی | ندارد | می‌تواند داشته باشد |
| Exhaustive switch | از نظر hierarchy کافی نیست | برای مجموعه کامل طراحی شده |

## نمونه فروشگاه

```csharp
public closed record class OrderState;
public sealed record Pending(DateTimeOffset CreatedAt) : OrderState;
public sealed record Paid(string PaymentId) : OrderState;
public sealed record Shipped(string TrackingCode) : OrderState;
public sealed record Cancelled(string Reason) : OrderState;

public sealed record Order(Guid Id, OrderState State);

public static string Describe(Order order) => order.State switch
{
    Pending pending => $"Created at {pending.CreatedAt}",
    Paid paid => $"Paid: {paid.PaymentId}",
    Shipped shipped => $"Tracking: {shipped.TrackingCode}",
    Cancelled cancelled => $"Cancelled: {cancelled.Reason}"
};
```

## Closed Hierarchy و Union

Closed hierarchy یک base hierarchy شیء‌محور است. Union یک نوع مجموعه‌ای از caseهاست که conversion ضمنی دارد و برای مدل‌سازی Outcomeها کاربردی‌تر است. Closed hierarchy زمانی مناسب است که انواع وضعیت هم‌نام بر روی base مشترک رفتار یا داده مشترک داشته باشند.

## نکات معماری

- Closed hierarchy برای Domain states مناسب است؛ روی pluginها یا extension APIs استفاده نکنید.
- اگر نیازی به باز بودن hierarchy دارید، closed modifier اشتباه است.
- `switch` باید همه descendantهای شناخته‌شده را پوشش دهد.
- Serialization و polymorphism را با JSON contracts و source generatorها بررسی کنید.

## منابع

- [Closed hierarchies in C# 15](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15#closed-hierarchies)
- [C# inheritance](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/inheritance)

