# تغییرات Blazor در ASP.NET Core 11

این فایل نکته‌های Blazor از متن What's New را به تفکیک قابلیت مرور می‌کند. APIها و packageهایی که هنوز Preview یا Experimental هستند را پیش از استفاده در production بررسی کنید.

## نمایش نام فیلدها

کامپوننت `DisplayName<TValue>` نام فیلد را از metadata می‌خواند. `[Display]` برای localization و metadata بیشتر مناسب است و در صورت وجود هم‌زمان با `[DisplayName]` اولویت دارد.

```razor
<label>
    <DisplayName For="@(() => Model!.ProductionDate)" />
    <InputDate @bind-Value="Model!.ProductionDate" />
</label>
```

## شروع برنامه و مسیریابی

- Blazor Server و WebAssembly از قالب جدید options برای `Blazor.start()` پشتیبانی می‌کنند.
- `<BasePath />` تگ `<base href>` را از base path برنامه تولید می‌کند.
- `RelativeToCurrentUri` در `NavigateTo` و `NavLink` برای ناوبری نسبت به مسیر جاری است.
- `NavigationManager.GetUriWithFragment(...)` ساخت URI همراه hash fragment را ساده می‌کند.
- قالب Blazor Web App از inline JavaScript در NavMenu فاصله گرفته و از JS module استفاده می‌کند که با CSP سازگارتر است.

```csharp
Navigation.NavigateTo("/configuration", new NavigationOptions
{
    RelativeToCurrentUri = true
});
```

## State در static SSR

برای نگهداری موقت داده میان requestها، static SSR از `TempData` پشتیبانی می‌کند. برای session state ماندگارتر از `AddSession`، `UseSession` و `[SupplyParameterFromSession]` استفاده کنید.

هنگام استفاده از session-backed state در streaming SSR، cookie مربوط به session ممکن است پیش از شروع stream صادر شود. در Blazor Web Appهای جدید، antiforgery middleware به‌صورت خودکار اضافه می‌شود و فراخوانی صریح `UseAntiforgery()` معمولاً لازم نیست.

## Web Worker و Blazor WebAssembly

- نام template به `blazorwebworker` تغییر کرده است؛ پروژه‌های موجود همچنان معتبرند.
- Web Worker client از `InvokeVoidAsync`، `CancellationToken` و timeout پشتیبانی می‌کند.
- template جداگانه‌ای برای service defaults با Aspire در دسترس است.
- `IHostedService` می‌تواند background work را در WebAssembly اجرا کند.
- WebAssembly configuration می‌تواند environment variableهایی مانند endpointها و feature flagها را بخواند.
- گزینه‌های browser startup با `WithBrowserOptions` از سمت سرور نیز قابل تنظیم‌اند.
- `EnvironmentView` محتوای صفحه را براساس hosting environment نمایش می‌دهد.
- client-side prerendering می‌تواند culture سرور را حفظ کند؛ در صورت نیاز می‌توان آن را غیرفعال کرد.
- MathML در interactive rendering با namespace صحیح ساخته می‌شود.
- برخی trimmingها خروجی publish را برای برنامه‌هایی که از OTEL یا Hot Reload استفاده نمی‌کنند کوچک‌تر می‌کنند.
- قالب Blazor Web App از گزینه container support در Visual Studio پشتیبانی می‌کند.

## Virtualize

`Virtualize<TItem>` اکنون اندازه واقعی itemها را در زمان اجرا در نظر می‌گیرد و برای تغییر ارتفاع آیتم‌ها scroll anchoring مناسب‌تری دارد. مقدار پیش‌فرض `OverscanCount` از 3 به 15 تغییر کرده است.

`AnchorMode` رفتار viewport را هنگام اضافه‌شدن آیتم‌ها مشخص می‌کند:

| مقدار | رفتار |
|---|---|
| `None` | جای viewport ثابت می‌ماند |
| `Start` | ابتدای فهرست را نگه می‌دارد |
| `End` | انتهای فهرست را نگه می‌دارد |

برای پیمایش مستقیم:

```razor
<Virtualize TItem="Product"
            Items="products"
            InitialItemIndex="500"
            @ref="list">
    <div>@context.Name</div>
</Virtualize>
```

پس از اولین render تعاملی، `ScrollToItemAsync(index)` نیز قابل استفاده است. آخرین فراخوانی هم‌زمان برنده می‌شود و index خارج از محدوده به محدوده معتبر clamp می‌شود.

برای سازگاری با CSP سخت‌گیرانه، ارتفاع spacerها به‌جای inline style در data attributeها render می‌شود.

## QuickGrid

- صفحه، sort و direction می‌توانند در query string ذخیره شوند؛ در نتیجه لینک قابل اشتراک و browser back/forward کار می‌کنند و static SSR نیز می‌تواند state را از URL بخواند.
- `QueryParameterNameOptions` نام پارامترها را تغییر می‌دهد. برای چند grid روی یک صفحه، نام‌های یکتا و `PaginationState` جداگانه در نظر بگیرید.
- ستون sort با `Title` شناسایی می‌شود؛ تغییر title می‌تواند URLهای bookmarkشده را بشکند.
- وقتی URL navigation فعال است، لینک‌های paginator و header به‌صورت anchor render می‌شوند؛ CSS سفارشی را با این markup هماهنگ کنید.
- `OnRowClick` برای واکنش به انتخاب ردیف اضافه شده است.
- grid مجازی‌شده APIهایی برای `InitialItemIndex` و `ScrollToItemAsync` دارد.
- برخی APIهای QuickGrid، از جمله `AnchorMode` و `ItemComparer`، ممکن است experimental باشند.
- می‌توان URL-based navigation را با AppContext switch خاموش کرد؛ این کار لزوماً URL query state داخلی را حذف نمی‌کند.

## اعتبارسنجی فرم و نمایش خروجی

- static SSR فرم‌ها می‌توانند با `DataAnnotationsValidator` اعتبارسنجی client-side ارائه دهند.
- Blazor forms از async validator برای بررسی‌هایی مثل دسترسی نام کاربری یا اطلاعات remote پشتیبانی می‌کنند؛ لغو validation قبلی و وضعیت pending نیز مدیریت می‌شود.
- `CacheView` خروجی subtreeهای ثابت را در static SSR cache می‌کند و در cache hit، lifecycle کامپوننت‌های فرزند اجرا نمی‌شود.
- `DisplayName<TValue>` و `EnvironmentView` به ترتیب برای metadata و نمایش شرطی مفیدند.
- ویژگی authentication تازه‌شده می‌تواند در circuit فعال اعمال شود، بدون reconnect.
- validation attributes از package `Microsoft.Extensions.Validation` که پیش‌تر experimental بودند، دیگر experimental علامت‌گذاری نمی‌شوند.

## Blazor AI

پکیج `Microsoft.AspNetCore.Components.AI` مجموعه‌ای اولیه از اجزای UI برای chat streaming، rich content، اجرای function، تأیید کاربر و وضعیت UI ارائه می‌کند. این قابلیت در متن به‌عنوان نوپا/Experimental معرفی شده است؛ وضعیت prerelease، API و نیازهای امنیتی را حتماً بررسی کنید.

## منبع

- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
- [Blazor](https://learn.microsoft.com/en-us/aspnet/core/blazor/)
