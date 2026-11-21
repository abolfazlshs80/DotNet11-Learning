# Labeled break و continue

# Labeled Break and Continue

## مشکل حلقه‌های تو در تو

وقتی حلقه داخلی شرطی را پیدا می‌کند و باید از یک حلقه بیرونی نیز خارج شود، روش قدیمی معمولا از یک flag، exception یا متد کمک استفاده می‌کند.

```csharp
foreach (var batch in batches)
{
    foreach (var item in batch)
    {
        if (item.Status == "Stop")
        {
            break;
        }
    }
}
```

در بالا `break` فقط از حلقه داخلی خارج می‌شود.

## Labeled break

```csharp
outer:
foreach (var batch in batches)
{
    foreach (var item in batch)
    {
        if (item.Status == "Stop")
            break outer;
    }
}
```

label مشخص می‌کند حلقه مقصد کدام است.

## Labeled continue

```csharp
outer:
foreach (var batch in batches)
{
    foreach (var item in batch)
    {
        if (item.Skip)
            continue outer;

        Process(item);
    }

    Console.WriteLine("Next batch");
}
```

در این صورت لغزش به iteration بعدی حلقه بیرونی انجام می‌شود.

## مقایسه با goto

Labeled break/continue از goto عمومی محدودتر است و فقط هدف کنترل حلقه را دارد؛ goto به هر statement اجازه می‌دهد، خوانایی را پایین می‌آورد و در بلوکهای scope نیز محدودیت دارد.

## نکات

- labelها را کوتاه و معنایی نگه دارید.
- از label زمانی استفاده کنید که nested loop نیاز به termination صریح دارد.
- اگر کد طولانی و چند خطی شد، extraction به یک متد یا LINQ قدیمی‌تر و خواناتر می‌تواند بهتر باشد.
- labeled continue را برای کنترل flowهای دامنه‌ای خطرناک استفاده نکنید؛ گاهی state machine یا method result واضح‌تر است.

## تمرین

یک ماتریس سه‌روشن پیدا کنید که در آن جمع سطرها از حد مجاز عبور کرده باشد. با labeled break فوراً از هر دو حلقه خارج شوید.

## منابع

- [C# 15 labeled break and continue](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15#labeled-break-and-continue)
- [Iteration statements](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/iteration-statements)

