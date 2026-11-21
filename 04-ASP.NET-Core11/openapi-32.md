# OpenAPI 3.2

# OpenAPI 3.2 in ASP.NET Core 11

OpenAPI یک قرارداد machine-readable برای endpointها، request/response، error و security است. Swagger یک ابزار یا implementation برای نمایش و تست قراردادهاست.

## تنظیم

```csharp
builder.Services.AddOpenApi(options =>
{
    options.OpenApiVersion =
        Microsoft.OpenApi.OpenApiSpecVersion.OpenApi3_2;
});

var app = builder.Build();
app.MapOpenApi();
app.Run();
```

برای UI از Scalar یا Swagger UI استفاده کنید.

## Endpoint با streaming

```csharp
app.MapGet("/orders/{id}/events", (
    Guid id,
    CancellationToken cancellationToken) =>
{
    async IAsyncEnumerable<OrderEvent> StreamEvents(
        [EnumeratorCancellation] CancellationToken ct)
    {
        yield return new OrderEvent(id, "Pending");
        await Task.Delay(100, ct);
        yield return new OrderEvent(id, "Paid");
    }

    return Results.Stream(StreamEvents(cancellationToken),
        contentType: "application/x-ndjson");
});
```

`application/x-ndjson` و statusهای binary یا streaming را از طریق OpenAPI/TypedResult مستند کنید.

## Minimal API documentation

```csharp
app.MapPost("/orders", (CreateOrderRequest request) =>
{
    return TypedResults.Accepted(
        $"/orders/{Guid.NewGuid()}",
        new OrderAccepted(Guid.NewGuid()));
})
.WithName("CreateOrder")
.WithSummary("Registers an order")
.WithDescription("Creates an order and queues asynchronous domain processing")
.WithTags("Orders");
```

## مزایا

- قرارداد API برای UI، SDK و تست قابل استفاده است
- binary و streaming responseها مستندسازی دقیق می‌شوند
- breaking changeها در CI قابل detection است

## محدودیت‌ها

- OpenAPI هر رفتار runtime را پوشش نمی‌دهد
- مثالها، descriptions و security schemes باید نگهداری شوند
- Swagger UI و Scalar نیاز به endpointهای ساخته‌شده دارند

## منابع

- [ASP.NET Core 11 OpenAPI](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [OpenAPI in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/overview)

