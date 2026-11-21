# Memory Safety در C# 15

# Memory Safety in C# 15

## مفهوم

Managed memory توسط GC مدیریت می‌شود؛ unmanaged memory توسط native APIs یا pointers مدیریت می‌شود. Pointerهای معمولی باعث escape از invariants runtime می‌شوند و use-after-free، buffer overflow و corruption را ممکن می‌کنند.

## Safe و Unsafe

کد managed عادی در safe context اجرا می‌شود. `unsafe` برای pointer operations و native interop است.

```csharp
int number = 42;

unsafe
{
    int* pointer = &number;
    *pointer = 100;
}
```

برای کامپایل:

```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
```

هر کد unsafe را محدود و قابل بازبینی نگه دارید.

## تغییرات Memory Safety

C# 15 تغییرات `updated memory safety rules` و keyword جدیدی به نام `safe` برای مدیریت context معرفی می‌کند؛ ویژگی‌های دقیق در مستندات رسمی و feature specification قرار دارند. نام‌ها یا sampleها را فقط در SDK که این قابلیت را پشتیبانی می‌کند استفاده کنید.

به جای فرض اینکه همه pointer operations اکنون safe شده‌اند، قوانین زیر را حفظ کنید:

- Pointer declaration معمولاً نیاز به unsafe context دارد.
- Pointer dereference در unsafe context انجام می‌شود.
- `fixed` برای pin کردن managed object و interop استفاده می‌شود.
- `stackalloc` حافظه stack را اختصاص می‌دهد و lifetime آن کوتاه است.
- تست و تحلیل را با `/p:AllowUnsafeBlocks` یا ویژگی پروژه کنار نگه دارید.

## Stackalloc

```csharp
Span<int> values = stackalloc int[8];
values[0] = 42;
```

این مثال کد safe است و به heap GC وابسته نیست؛ برای اندازه‌های بزرگ یا lifetimهای غیرموقتی مناسب نیست.

## Fixed

```csharp
unsafe
{
    byte[] buffer = new byte[256];
    fixed (byte* p = buffer)
    {
        NativeApi(p, buffer.Length);
    }
}
```

بعد از پایان block، GC می‌تواند object را جابه‌جا کند.

## مزایا و خطرات

مزایا شامل interop کنترل‌شده و الگوریتم‌های special-purpose است؛ خطرات شامل crash، memory corruption، data race، lifetime violation و سخت‌تر بودن audit است.

## بهترین روش‌ها

1. ابتدا `Span<T>` و `Memory<T>` را امتحان کنید.
2. K own و pin را به مرزهای بسیار کوچک محدود کنید.
3. native buffer length را پیش از فراخوانی verify کنید.
4. unmanaged handleها را با SafeHandle و lifetime type مدیریت کنید.

## تمرین

یک پکیج `byte[]` را با `fixed` به `Marshal.Copy` یا تابع native ارسال کنید و تفاوت lifetime را مستند کنید.

## منابع

- [C# 15 memory safety](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-15)
- [Unsafe code](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/unsafe-code)
- [Pointers](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/unsafe-code#pointers)
