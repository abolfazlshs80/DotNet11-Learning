# Collection Expression Arguments

# C# 15 Collection Expression Arguments

## مشکل اولیه

اگر می‌خواهید هم Collection Expression داشته باشید و هم Capacity یا Comparer را تعیین کنید، بین `new List<T>()` و collection literalها باید انتخاب می‌کردید. در C# 15، عنصر اول `with(...)` آرگومان‌های سازنده یا factory را می‌دهد.

```csharp
string[] values = ["one", "two", "three"];

List<string> names =
[
    with(capacity: values.Length * 2),
    .. values
];

HashSet<string> set =
[
    with(StringComparer.OrdinalIgnoreCase),
    "Hello",
    "HELLO",
    "hello"
];
```

اولین عنصر حتماً `with(...)` باشد. مقادیر spread بعد از آن با `..` وارد Collection می‌شوند.

## روش قدیمی

```csharp
var names = new List<string>(values.Length * 2);
names.AddRange(values);

var set = new HashSet<string>(StringComparer.OrdinalIgnoreCase);
set.UnionWith(["Hello", "HELLO", "hello"]);
```

روش قدیمی شفاف است اما boilerplate بیشتر دارد. `with(...)` نامرتبط نیست و فقط زمانی مفید است که Collection Expression مربوط به نوعی قابل ساخت با constructor یا factory باشد.

## کاربردهای Backend

```csharp
var allowedStates =
[
    with(StringComparer.OrdinalIgnoreCase),
    "Pending",
    "Paid",
    "Shipped"
];

var basePermissions = new[] { "read", "write" };
var expandedPermissions =
[
    with(capacity: basePermissions.Length + 2),
    .. basePermissions,
    "audit",
    "export"
];
```

در متدهایی که داده ورودی را normalize می‌کنند، Comparer را یک بار مشخص کنید.

## مزایا و محدودیت‌ها

- Capacity با تعداد عناصر یا spreadها کمک می‌کند allocation را کاهش دهد.
- Comparer برای setهای مثل کدهای وضعیت و ایمیل مفید است.
- اسپرد کردن Sequenceهای عظیم ممکن است copy یا enumerate انجام دهد.
- این Syntax جایگزین نحوه مدیریت concurrency یا immutable collections نیست.
- برای انواعی که سازنده مناسب ندارند ممکن است collection expression arguments قابل استفاده نباشند.

## تمرین

یک Cache key set بسازید که comparer آن `StringComparer.OrdinalIgnoreCase` باشد و ظرفیت آن از اندازه لیست ورودی گرفته شود.

## منابع

- [C# 15 - Collection expression arguments](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15#collection-expression-arguments)
- [Collection expressions language reference](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/collection-expressions)

