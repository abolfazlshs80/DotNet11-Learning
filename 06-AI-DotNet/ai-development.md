# هوش مصنوعی در اکوسیستم .NET 11

# AI Development

## Microsoft.Extensions.AI

این library abstraction مشترکی برای I/O مدل‌های زبانی و embedding ارائه می‌کند. Microsoft.Extensions.AI زنجیره‌هایی برای chat، tool calling و middleware فراهم می‌کند.

```csharp
// Package: Microsoft.Extensions.AI
// Package provider: implementation-specific

// IChatClient را از provider implementation خود resolve کنید.
// service.AddSingleton<IChatClient>(...)
```

نام و نسخه provider را به صورت pinned نگه دارید؛ یک abstraction نصب‌شده به معنی support همه providerها نیست.

## MCP

Model Context Protocol قرارداد اتصال AI به tools و resources است. MCP در اکوسیستم .NET و C# می‌تواند client/server باشد اما نباید آن را صرفاً یک feature جدید Core .NET 11 دانست.

## Microsoft Agent Framework

Agent Framework orchestration، tool calls، workflow و memory را مدیریت می‌کند. کاربرد آن در agentهای چندمرحله‌ای است؛ برای endpointهای ساده chat مستقیم کد کمتر و قابل‌auditتر است.

## API Chat

```csharp
app.MapPost("/api/ai/chat", async (
    ChatRequest request,
    IChatClient chat,
    CancellationToken cancellationToken) =>
{
    var response = await chat.GetResponseAsync(
        request.Message,
        cancellationToken: cancellationToken);

    return Results.Ok(new { response.Text });
});
```

Signature واقعی SDK/package باید compile شود؛ این مثال ساختار مفهومی را نشان می‌دهد.

## Tool Calling

Tool call خروجی را از مدل به یک تابع C# می‌رساند:

- Tool name و schema را محدود نگه دارید
- Authorization را در function اجرا کنید
- درخواستها و outputها را sanitize کنید
- secrets را در prompt نگذارید

## Database و RAG

برای جستجوی semantic از EF Core vector search یا external vector store استفاده کنید. نتایج را به prompt محدود کنید و citation نگه دارید.

## تصمیم‌گیری

| انتخاب | کاربرد |
|---|---|
| Microsoft.Extensions.AI | abstraction مدل و middleware |
| MCP | tool/resource exchange استاندارد |
| Agent Framework | orchestration چندمرحله‌ای |
| ASP.NET API | مدیریت auth، HTTP و observability |

## منابع

- [Microsoft.Extensions.AI](https://learn.microsoft.com/en-us/dotnet/ai/microsoft-extensions-ai)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/)

