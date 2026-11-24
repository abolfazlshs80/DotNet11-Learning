# تغییرات Authentication و Authorization در ASP.NET Core 11

## TimeProvider در Identity

ASP.NET Core Identity از `TimeProvider` برای عملیات وابسته به زمان استفاده می‌کند. در تست می‌توان clock جعلی تزریق کرد تا expiration، lockout و security stamp به‌صورت قابل تکرار آزموده شوند.

## نام قابل‌نمایش Passkey

Identity می‌تواند برای authenticatorهای passkey شناخته‌شده نام دوستانه تعیین کند. برای authenticatorهای ناشناخته کاربر به جریان تغییر نام هدایت می‌شود و mappingهای اضافی قابل تعریف‌اند.

## JWT برای برنامه تک‌فایلی

`dotnet user-jwts` برای برنامه‌های file-based نیز کار می‌کند:

```powershell
dotnet user-jwts create --file app.cs
```

این ابزار برای توکن‌های development است، نه جایگزین identity provider در production.

## سیاست‌های Authorization یکپارچه

Authorization metadata از نوع `IAuthorizeData`، `AuthorizationPolicy` و `IAuthorizationRequirementData` هماهنگ‌تر روی MVC، SignalR و Blazor اعمال می‌شود.

## Negotiate و TLS Channel Binding

Negotiate authentication روی Kestrel می‌تواند channel-binding token اتصال HTTPS را وارد جریان Kerberos/NTLM کند. در اتصال غیر HTTPS یا وقتی token در دسترس نیست، رفتار پیشین حفظ می‌شود.

## Device Bound Session Credentials

پکیج `Microsoft.AspNetCore.Authentication.DeviceBoundSessions` یک پیاده‌سازی server-side آزمایشی برای اتصال تمدید session به کلید خصوصی مرورگر فراهم می‌کند.

این قابلیت در متن Experimental و prerelease اعلام شده و به پشتیبانی آزمایشی مرورگر وابسته است. برای استفاده production به‌عنوان قابلیت پایدار روی آن حساب نکنید.

## منابع

- [Authentication and authorization in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/)
- [What's new in ASP.NET Core in .NET 11](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-11)
