# پروژه عملی DotNet11.OrderManagement

# Order Management Backend

## هدف

پروژه‌ای چندلایه برای مدیریت محصول، سفارش، موجودی، وضعیت و پرداخت می‌سازیم:

```text
src/
  OrderManagement.Api/
  OrderManagement.Application/
  OrderManagement.Domain/
  OrderManagement.Infrastructure/
tests/
  OrderManagement.UnitTests/
  OrderManagement.IntegrationTests/
```

## ساخت Solution

```powershell
dotnet new sln -n DotNet11.OrderManagement
dotnet new webapi -n OrderManagement.Api -o src/OrderManagement.Api --framework net11.0
dotnet new classlib -n OrderManagement.Application -o src/OrderManagement.Application --framework net11.0
dotnet new classlib -n OrderManagement.Domain -o src/OrderManagement.Domain --framework net11.0
dotnet new classlib -n OrderManagement.Infrastructure -o src/OrderManagement.Infrastructure --framework net11.0

dotnet sln DotNet11.OrderManagement.sln add src/*/*.csproj
```

## Domain: Union Result

```csharp
namespace OrderManagement.Domain;

public record class OrderCreated(Guid OrderId);
public record class InsufficientStock(string Sku);
public record class InvalidOrder(string Reason);

public union CreateOrderResult(OrderCreated, InsufficientStock, InvalidOrder);

public sealed record OrderLine(string Sku, int Quantity);
public sealed record CreateOrderRequest(Guid CustomerId, IReadOnlyList<OrderLine> Lines);
```

## Domain: Closed Status

```csharp
public closed record class OrderStatus;

public sealed record Pending : OrderStatus;
public sealed record Paid(string PaymentId) : OrderStatus;
public sealed record Shipped(string TrackingCode) : OrderStatus;
public sealed record Cancelled(string Reason) : OrderStatus;
```

## Collection Expression Arguments

```csharp
string[] allowedStatuses = ["Pending", "Paid", "Shipped"];

var normalizedStatuses =
[
    with(StringComparer.OrdinalIgnoreCase),
    .. allowedStatuses
];
```

## API

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddOpenApi(options =>
{
    options.OpenApiVersion =
        Microsoft.OpenApi.OpenApiSpecVersion.OpenApi3_2;
});

var app = builder.Build();
app.MapOpenApi();

app.MapPost("/api/orders", async (
    CreateOrderRequest request,
    ICreateOrderHandler handler,
    CancellationToken cancellationToken) =>
{
    var result = await handler.HandleAsync(request, cancellationToken);

    return result switch
    {
        OrderCreated created => Results.Created(
            $"/api/orders/{created.OrderId}", created),
        InsufficientStock stock => Results.Conflict(
            new { stock.Sku }),
        InvalidOrder invalid => Results.UnprocessableEntity(
            new { invalid.Reason })
    };
});

app.Run();
```

## EF Core Infrastructure

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="11.0.*" />
```

`DbContext` شامل Aggregateهای Product و Order باشد. Stock check و Order creation را در transaction انجام دهید تا race condition موجودی ایجاد نشود.

## تست‌ها

```powershell
dotnet new xunit -n OrderManagement.UnitTests -o tests/OrderManagement.UnitTests
dotnet new xunit -n OrderManagement.IntegrationTests -o tests/OrderManagement.IntegrationTests
```

Unit testها Domain rules را آزمایش می‌کنند؛ Integration testها SQL Server، EF، API و OpenAPI را پوشش می‌دهند.

## قابلیت‌های واقعی

- Async stock check
- Unique constraints روی SKU و Payment ID
- Domain event transaction
- problem details
- structured logging
- OpenTelemetry metrics
- cancellation

## منابع

- [.NET 11 overview](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-11/overview)
- [ASP.NET Core 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [EF Core 11](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-11.0/whatsnew)

