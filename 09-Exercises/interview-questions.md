# سؤالات مصاحبه C# 15 و .NET 11

# Interview Questions

## 1. تفاوت C# و .NET چیست؟

C# زبان و کامپایلر، .NET runtime و library platform است.

## 2. تفاوت SDK و Runtime چیست؟

SDK ابزار build است؛ Runtime برنامه را اجرا می‌کند.

## 3. C# 15 چگونه با .NET 11 مرتبط است؟

C# 15 و runtime 11 همزمان منتشر می‌شوند اما قابلیت‌های language و runtime متفاوت‌اند.

## 4. Union Type چیست؟

نوعی که مقدار را به چند case محدود می‌کند و switch را exhaustively بررسی می‌کند.

```csharp
public union Result(Success, Failure);
```

## 5. Union در مقابل `object`

`object` caseها را از type system مخفی می‌کند؛ union آنها را شناخته‌شده نگه می‌دارد.

## 6. Union در مقابل Interface

Union مجموعه caseها را closed می‌کند؛ interface برای implementationهای خارجی باز است.

## 7. Closed hierarchy چیست؟

Hierarchyای که مشتق‌سازی بیرون assembly را ممنوع می‌کند.

## 8. Closed hierarchy و sealed چه تفاوتی دارند؟

sealed هیچ مشتقی ندارد؛ closed مشتقات داخلی دارد.

## 9. Closed hierarchy و union چه تفاوتی دارند؟

closed یک hierarchy شیء‌محور است؛ union مجموعه caseهای زبان است.

## 10. Collection expression arguments چیست؟

عنصر `with(...)` برای ارسال constructor/factory argumentها در collection expression.

## 11. Extension indexer چیست؟

قابلیت اضافه کردن دسترسی براکتی به receiver خارجی.

## 12. Labeled break چه زمانی مفید است؟

برای خروج صریح از حلقه بیرونی nested loop.

## 13. Labeled break و goto چیست؟

Labeled break محدود به حلقه است؛ goto کنترل عمومی دارد و خوانایی را کاهش می‌دهد.

## 14. Memory safety چگونه تعریف می‌شود؟

جلوگیری از unmanaged memory accessهای غیرمجاز و corruption با managed invariants.

## 15. Unsafe context چه زمانی لازم است؟

برای pointer operations، fixed managed buffer و native interop.

## 16. Runtime Async چیست؟

اجرای Task-returning async methodهایی که بخشی از runtime execution به جای compiler state machine صرف است.

## 17. آیا Runtime Async همیشه performance بهبود می‌دهد؟

خیر؛ workload، I/O، allocation، tiering و environment اثر می‌گذارند.

## 18. Native AOT مزیت و محدودیت دارد؟

startup و native deployment بهتر است؛ reflection و trimming compatibility چالش دارند.

## 19. تفاوت ASP.NET Core 11 و 10؟

11 شامل async validation، OpenAPI 3.2 و بهبودهای Minimal API و providerها است؛ نسخه فعلی را از release notes بررسی کنید.

## 20. AsyncValidationAttribute چیست؟

Feature validation وابسته به I/O که asynchronous rules را قبل از use case اجرا می‌کند.

## 21. وقتی async validation استفاده کنیم؟

وقتی قانون به database، cache یا external service دسترسی دارد.

## 22. `IAsyncValidatableObject` و attribute تفاوت دارند؟

Object-level cross-property و I/O ruleها برای IAsyncValidatableObject مناسب‌اند؛ attribute reusable و field-level است.

## 23. OpenAPI و Swagger چه تفاوتی دارند؟

OpenAPI specification است؛ Swagger ابزارهای implementation و UI هستند.

## 24. OpenAPI 3.2 چه کاربردی دارد؟

Contract endpointها، binary/streaming responses و tooling را استاندارد می‌کند.

## 25. Endpoint filter چیست؟

Middleware دقیق Endpoint-level برای Minimal API.

## 26. Middleware و Endpoint Filter تفاوت دارند؟

Middleware global request pipeline است؛ endpoint filter به binding و endpoint تعلق دارد.

## 27. EF Core FullJoin چیست؟

ترکیب دو مجموعه که unmatched رکوردهای هر دو طرف را نگه می‌دارد.

## 28. Vector embedding چیست؟

بردار عددی برای نمایش semantic content است.

## 29. Vector index و ANN چیست؟

Index vector و approximate nearest neighbor برای جستجوی نزدیک با trade-off recall هستند.

## 30. چک‌لیست مهاجرت چیست؟

SDK, package, provider, infrastructure, breaking changes, tests, performance, migration, rollback و support.

