# تغییرات SignalR در ASP.NET Core 11

## تازه‌سازی Authentication

SignalR می‌تواند access token را بدون قطع اتصال تازه کند. سرور این قابلیت را برای هر hub فعال می‌کند و می‌تواند identity قبلی و جدید را با callback مقایسه یا رد کند.

```csharp
app.MapHub<ChatHub>("/chat", options =>
{
    options.EnableAuthenticationRefresh = true;
    options.CloseOnAuthenticationExpiration = true;

    options.OnAuthenticationRefresh = context =>
    {
        var oldSubject = context.PreviousUser.FindFirst("sub")?.Value;
        var newSubject = context.NewUser.FindFirst("sub")?.Value;

        return Task.FromResult(
            oldSubject is not null &&
            string.Equals(oldSubject, newSubject, StringComparison.Ordinal));
    };
});
```

Hub می‌تواند پس از اعمال identity جدید به `OnAuthenticationRefreshedAsync` واکنش نشان دهد. کلاینت .NET و TypeScript APIهایی برای تنظیم refresh خودکار، دریافت رویداد موفقیت یا شکست و درخواست refresh فوری دارند. کلاینت .NET به‌طور پیش‌فرض refresh خودکار را فعال می‌کند.

## لغو فراخوانی Hub

کلاینت می‌تواند هنگام فراخوانی non-streaming یک `CancellationToken` ارسال کند. لغو token پیام cancellation می‌فرستد و token متناظر در متد hub نیز لغو می‌شود.

```csharp
public class WorkHub : Hub
{
    public Task LongRunningWork(CancellationToken cancellationToken) =>
        Task.Delay(TimeSpan.FromMinutes(5), cancellationToken);
}
```

## پیکربندی اتصال Interactive Server

برای Blazor Interactive Server می‌توان گزینه‌های `HttpConnectionDispatcherOptions` را هنگام ثبت render mode تنظیم کرد:

```csharp
app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode(options =>
    {
        options.ConfigureConnection = connection =>
        {
            connection.CloseOnAuthenticationExpiration = true;
            connection.AllowStatefulReconnects = true;
        };
    });
```

## Union types

SignalR از C# union types فقط با JSON hub protocol پشتیبانی می‌کند؛ MessagePack و Newtonsoft.Json protocol این نوع را پشتیبانی نمی‌کنند.

## منابع

- [SignalR authentication and authorization](https://learn.microsoft.com/en-us/aspnet/core/signalr/authn-and-authz)
- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
