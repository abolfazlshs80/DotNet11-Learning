# ترجمه تاریخ و زمان در EF Core 11

EF Core 11 ترجمه‌های بیشتری برای `DateTimeOffset` و دیگر typeهای زمان SQL Server اضافه می‌کند.

## قابلیت‌های پشتیبانی‌شده

- دسترسی مستقیم به `DateTime`
- تبدیل UTC و local
- `Offset.TotalMinutes`
- `ToOffset(...)`
- ساخت `DateTimeOffset` از `DateTime`
- `EF.Functions.DateTrunc(...)`

## DateTrunc

```csharp
var dailyEvents = await context.Events
    .Where(eventEntry =>
        EF.Functions.DateTrunc(
            "day",
            eventEntry.OccurredAt) ==
        EF.Functions.DateTrunc(
            "day",
            DateTimeOffset.UtcNow))
    .ToListAsync();
```

این function روی precisionهای مثل روز، ساعت و دقیقه قابل استفاده است.
