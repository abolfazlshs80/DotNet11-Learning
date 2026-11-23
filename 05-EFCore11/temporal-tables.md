# Temporal Tables در EF Core 11

EF Core 11 می‌تواند ستون‌های `PeriodStart` و `PeriodEnd` را روی CLR propertyهای معمولی map کند.

```csharp
public sealed class Employee
{
    public Guid EmployeeId { get; set; }
    public required string Name { get; set; }
    public required string Position { get; set; }
    public DateTime PeriodStart { get; set; }
    public DateTime PeriodEnd { get; set; }
}

modelBuilder.Entity<Employee>()
    .ToTable(
        "Employees",
        table => table.IsTemporal(
            temporal =>
            {
                temporal.HasPeriodStart(employee => employee.PeriodStart);
                temporal.HasPeriodEnd(employee => employee.PeriodEnd);
            }));
```

این properties مستقیماً قابل استفاده در LINQ هستند:

```csharp
var history = context.Employees
    .TemporalAll()
    .OrderBy(employee => employee.PeriodStart)
    .Select(employee => new
    {
        employee.Name,
        employee.PeriodStart,
        employee.PeriodEnd
    })
    .ToList();
```

مقدار periodها توسط SQL Server تولید می‌شوند و در INSERT یا UPDATE به‌صورت معمولی ارسال نمی‌شوند.
