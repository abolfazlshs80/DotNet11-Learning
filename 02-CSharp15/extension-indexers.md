# Extension Indexers

# C# 15 Extension Indexers

## مفهوم

Indexer دسترسی به عضو با براکتهاست. Extension Member افزوده‌ای برای نوعی دیگر است. Extension Indexer indexer را به extension block اضافه می‌کند.

```csharp
public static class SequenceIndexer
{
    extension(IEnumerable<int> sequence)
    {
        public int this[int index] => sequence.ElementAt(index);
    }
}
```

استفاده:

```csharp
IEnumerable<int> numbers = [10, 20, 30];
Console.WriteLine(numbers[1]);
```

برای typeهایی مثل `IEnumerable<T>` که indexer استاندارد ندارند، دسترسی تسکین‌محور ارائه می‌شود؛ اما `ElementAt` ممکن است enumerate کند و بنابراین نباید برای لیستهای بزرگ یا sourceهای streaming استفاده شود.

## Extension block و named receiver

Receiver نمونه‌ای از نوع هدف است که در داخل block قابل دسترسی است:

```csharp
public static class CalendarExtensions
{
    extension(IReadOnlyDictionary<DateOnly, decimal> totals)
    {
        public decimal this[string isoDate]
            => DateOnly.TryParse(isoDate, out var date) && totals.TryGetValue(date, out var value)
                ? value
                : 0m;
    }
}
```

این مثال خوانایی بهتر دارد ولی نام متد یا lookup method به‌طور صریح‌تر خطاها را کنترل می‌کند.

## کاربرد Collectionها

```csharp
public static class ReadOnlySpanIndexers
{
    extension(ReadOnlySpan<char> text)
    {
        public char this[Index index] => text[index];
    }
}
```

## مزایا و محدودیت‌ها

- قابلیت خوانایی را به نوع‌های external اضافه می‌کند.
- برای زنجیره‌ها و APIهای عمومی مناسب است اما باید پیچیدگی lookup شفاف باشد.
- extension indexers ثابت یا overloading پیچیده را بدتر می‌کنند.
- indexing با مقدار ناموجود باید policy مشخص داشته باشد: exception، sentinel یا fallback.
- هرزمان که فقط یک عملیات خواندنی ساده است، method یا property معمولاً واضح‌تر است.

## منابع

- [C# 15 extension indexers](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15#extension-indexers)
- [Extension members](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)

