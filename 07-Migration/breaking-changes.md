# Breaking Changes و سازگاری مهاجرت

# Breaking Changes

## بررسی Systematic

هر package را در سه سطح بررسی کنید:

- TargetFramework support
- API compatibility
- Runtime/provider compatibility

```powershell
dotnet list package --outdated
dotnet list package --vulnerable
dotnet list package --deprecated
```

## چک‌لیست

- [ ] SDK و runtime در CI pin شده‌اند
- [ ] EF provider و SQL Server compatibility تست شده‌اند
- [ ] ASP.NET packageها از یک Family compatible هستند
- [ ] packageهای native و OS RID تست شده‌اند
- [ ] reflection/trimming در Native AOT نیست
- [ ] APIهای obsolete حذف شده‌اند
- [ ] JSON serialization contractهای breaking ندارند
- [ ] Time zone و encoding regression ندارند
- [ ] telemetry و log parserها سازگارند
- [ ] container base image runtime 11 دارد

## فهرست رسمی

مرجع نهایی breaking changes [صفحه compatibility .NET 11](https://learn.microsoft.com/en-us/dotnet/core/compatibility/11.0) است؛ release notes را با package releaseهای تیم مقایسه کنید.

## استراتژی

- SDK را در `global.json` pin کنید
- packageها را یکی‌یکی ارتقا دهید
- Integration tests را قبل و بعد اجرا کنید
- database migration را جدا و reversible نگه دارید
- feature flag برای rollout و rollback استفاده کنید

