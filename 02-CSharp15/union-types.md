# Union Types

# C# 15 Union Types

## معرفی مفهوم

Union Type مقدار را محدود به چند type یا case می‌کند. برخلاف `object`، کامپایلر همه caseها را می‌شناسد و در `switch` از exhaustiveness مطلع است.

```csharp
public record class Cat(string Name);
public record class Dog(string Name);
public record class Bird(string Name);

public union Pet(Cat, Dog, Bird);

Pet pet = new Dog("Rex");

string name = pet switch
{
    Dog dog => dog.Name,
    Cat cat => cat.Name,
    Bird bird => bird.Name
};
```

## Union، Interface و Abstract Class

| روش | مزیت | محدودیت |
|---|---|---|
| `object` | آزاد و قابل قبول همه نوع‌ها | تحلیل caseها توسط کامپایلر وجود ندارد |
| Interface | امتدادپذیر و باز | هر implementation خارجی ممکن است اضافه شود |
| Abstract class | اشتراک رفتارهای غنی | hierarchy و construction پیچیده |
| Union | مجموعه caseهای شناخته‌شده و implicit conversion | مدل محدود ولی مناسب برای دامنه‌های بسته |

## مدل نتیجه عملیات

```csharp
public record class OrderCreated(Guid OrderId);
public record class InsufficientStock(string Sku);
public record class InvalidOrder(string Reason);

public union CreateOrderResult(OrderCreated, InsufficientStock, InvalidOrder);

public static CreateOrderResult CreateOrder(IReadOnlyList<(string Sku, int Quantity)> lines)
{
    foreach (var line in lines)
    {
        if (line.Quantity <= 0)
            return new InvalidOrder($"Invalid quantity for {line.Sku}");
    }

    return new OrderCreated(Guid.NewGuid());
}

var result = CreateOrder([("A1", 2)]);

int statusCode = result switch
{
    OrderCreated => 201,
    InsufficientStock => 409,
    InvalidOrder => 422
};
```

این روش برای success/failureهای کوچک که هیچ نمونه دیگری نباید به آن اضافه کند مناسب است.

## مقایسه روش قدیمی

```csharp
public abstract record CreateOrderOutcome;
public sealed record Created(Guid OrderId) : CreateOrderOutcome;
public sealed record Failed(string Error) : CreateOrderOutcome;
```

Abstract record نوعی از closed modeling است اما فرزندان دیگر می‌توانند در assemblyهای مجاز اضافه شوند و switchها ممکن است از همه نوع‌ها مطلع نباشند؛ Union برای caseهای صریح و بسته شده زبان کمکی دارد.

## مدیریت خطاهای API

Union نباید جایگزین طبقه‌بندی عمومی Exception شود؛ برای نتیجه‌ی قابل انتظار دامنه مناسب است. Exception را برای شکست‌های غیرقابل‌پیش‌بینی، I/O و زیرساخت نگه دارید.

## محدودیت‌ها

- Union برای اکثریت APIها نباید با نوع‌های داده خارجی یا قراردادهای JSON مرتبط شود.
- serialization، JSON و OpenAPI به پشتیبانی خاص نیاز دارند؛ نتیجه union را ابتدا به DTO تبدیل کنید.
- تبدیل ضمنی برای هر case را روشن نگه دارید؛ در collisionهای ambiguous از constructorهای صریح استفاده کنید.

## جمع‌بندی

Union برای Domain Modeling و خروجی‌های use case مفید است چون واقعاً مجموعه caseها را نوع می‌کند و در switch باید کامل پوشش داده شود.

## تمرین

یک union برای نتیجه پرداخت طراحی کنید:

- `PaymentSucceeded`
- `PaymentDeclined`
- `PaymentProviderUnavailable`

یک `switch` بنویسید که HTTP status مناسب را برگرداند.

## منابع

- [C# 15 union types](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15#union-types)
- [Union types language reference](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/union)

