# تمرین‌های C# 15

# C# 15 Exercises

## 1. Collection Expression Arguments

### مقدماتی

برای یک `HashSet<string>`، از `StringComparer.OrdinalIgnoreCase` و عناصر اولیه استفاده کنید.

خروجی مورد انتظار: مقدارهای `"A"` و `"a"` فقط یک بار باشند.

### متوسط

از capacity و spread استفاده کنید تا لیست کامنت‌های یک batch را بسازید.

خروجی مورد انتظار: ظرفیت حداقل برابر batch count باشد.

### پیشرفته

یک collection expression برای factory یک custom collection پیاده‌سازی کنید که configuration callback را می‌پذیرد.

خروجی مورد انتظار: callback فقط یک بار برای construction اجرا شود.

## 2. Union Types

### مقدماتی

union زیر را پیاده‌سازی کنید:

```text
FindUserResult = UserFound | UserNotFound
```

### متوسط

به union بالا مورد `DatabaseUnavailable` اضافه کنید و exhaustiveness switch را تکمیل کنید.

### پیشرفته

یک API `CreateSubscription` بسازید که برای paid/failed/not eligible responses union داشته باشد و آن را به ProblemDetails یا HTTP status تبدیل کند.

## 3. Closed Hierarchy

### مقدماتی

برای حالت‌های `Draft`, `Submitted`, `Approved` یک closed record hierarchy بسازید.

### متوسط

transitionهای ناممکن را در یک state machine ممنوع کنید.

### پیشرفته

یک `OrderState` با derivations Pending/Paid/Shipped/Cancelled بسازید و از assembly خارجی مشتق‌سازی را منع کنید.

## 4. Extension Indexers

### مقدماتی

برای `IReadOnlyDictionary<string, decimal>` یک indexer با key fallback به صفر بسازید.

### متوسط

یک extension indexer برای `ReadOnlySpan<T>` بسازید که index منفی را روی آخر Collection محاسبه کند.

### پیشرفته

با استفاده از named receiver، یک indexer برای `IEnumerable<T>` بسازید و مشاهده کنید ElementAt بر روی lazy sequence هزینه دارد.

## 5. Labeled break/continue

### مقدماتی

دو حلقه تو در تو بسازید و با labeled break وقتی مقدار منفی پیدا شود خارج شوید.

### متوسط

ماتریس ناخواسته را با labeled continue به batch بعدی هدایت کنید.

### پیشرفته

نسخه‌ای با flag و exception و labeled control بنویسید و complexity و خوانایی را مقایسه کنید.

## 6. Memory Safety

### مقدماتی

یک buffer کوچک را با `stackalloc` بسازید.

### متوسط

یک `Span<byte>` را به یک API managed ارسال کنید و copy را اندازه‌گیری کنید.

### پیشرفته

یک `SafeHandle` برای native resource بسازید و lifetime و release را تست کنید.

