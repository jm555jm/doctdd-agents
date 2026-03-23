---
name: dotnet-webapi
description: This skill should be used when implementing backend features with ASP.NET Core Web API, or when the /doctdd command dispatches the backend-expert agent during Phase 3. Provides controller conventions, DI patterns, service/repository layer structure, and middleware pipeline guidance.
disable-model-invocation: true
version: 1.0.0
---

**ASP.NET Core Web API** technical knowledge for backend implementation.

## Tech Stack

- Runtime: .NET (C#)
- Framework: ASP.NET Core Web API
- DI: Built-in Microsoft.Extensions.DependencyInjection
- Configuration: appsettings.json with Options<T> pattern

## Project Structure (Typical)

```
ProjectName/
├── Program.cs              # App startup, DI registration, middleware pipeline
├── Controllers/            # API endpoints
├── Services/
│   └── Interfaces/         # Service interfaces
├── Repositories/
│   └── Interfaces/         # Data access interfaces
├── Models/                 # DTOs, request/response contracts
├── Entities/               # Domain entities
├── Attributes/             # Custom filter attributes
├── Exceptions/             # Custom exception types
├── Extensions/             # DI registration extension methods
├── Helpers/                # Utility classes
└── appsettings.json        # Configuration
```

## Controller Conventions

```csharp
[Route("api/[controller]/[action]")]
[ApiController]
public class ExampleController : ControllerBase
{
    private readonly ILogger<ExampleController> _logger;
    private readonly IExampleService _exampleService;

    public ExampleController(
        ILogger<ExampleController> logger,
        IExampleService exampleService)
    {
        _logger = logger;
        _exampleService = exampleService;
    }

    [HttpGet]
    public async Task<ExampleResponse> GetSomething()
    {
        return await _exampleService.GetSomethingAsync();
    }

    [HttpPost]
    public async Task<ActionResult<CreateResponse>> Create(
        [FromBody] CreateRequest request)
    {
        var result = await _exampleService.CreateAsync(request);
        return Ok(result);
    }
}
```

**Key rules:**
- Constructor injection for all dependencies
- Async endpoints with `Task<T>` return types
- Prefer strong return types over generic `IActionResult` when possible
- Return DTOs, never expose domain entities directly
- One controller per domain area

## Service Layer Pattern

```csharp
// Interface
public interface IExampleService
{
    Task<ExampleResponse> GetSomethingAsync();
    Task<CreateResponse> CreateAsync(CreateRequest request);
}

// Implementation
public class ExampleService : IExampleService
{
    private readonly ILogger<ExampleService> _logger;
    private readonly IExampleRepository _repository;

    public ExampleService(
        ILogger<ExampleService> logger,
        IExampleRepository repository)
    {
        _logger = logger;
        _repository = repository;
    }

    public async Task<ExampleResponse> GetSomethingAsync()
    {
        // Business logic here
    }
}
```

**Key rules:**
- Every service has an interface
- Constructor injection only
- Async methods end with `Async` suffix
- `ILogger<T>` injected for structured logging

## DI Registration Pattern

```csharp
// In Program.cs or extension methods
builder.Services.AddTransient<IExampleService, ExampleService>();
builder.Services.AddScoped<IScopedService, ScopedService>();
builder.Services.AddSingleton<ISingletonService, SingletonService>();

// Decorator pattern for cross-cutting concerns (caching, logging)
builder.Services.AddTransient<IExampleService, ExampleService>()
    .Decorate<IExampleService, ExampleServiceCache>();

// HttpClient factory for external APIs
builder.Services.AddHttpClient<IExternalApiProxy, ExternalApiProxy>(client =>
{
    client.BaseAddress = new Uri(configuration["ExternalApiUrl"]);
    client.Timeout = TimeSpan.FromSeconds(5);
});
```

**Lifetime guide:**
- `Transient` — stateless services (most services)
- `Scoped` — request-scoped (user context, DB transaction)
- `Singleton` — app-wide (cache, configuration)

## Configuration Pattern

```csharp
// appsettings.json
{
  "FeatureConfig": {
    "Enabled": true,
    "MaxRetries": 3
  }
}

// Registration
builder.Services.Configure<FeatureConfig>(
    configuration.GetSection("FeatureConfig"));

// Injection
public class MyService
{
    private readonly FeatureConfig _config;
    public MyService(IOptions<FeatureConfig> options)
    {
        _config = options.Value;
    }
}
```

## Middleware Pipeline Order

The order in `Program.cs` matters:

```
Error handling → Health checks → CORS → Swagger (dev only)
→ Forwarded headers → Logging → Security headers
→ Authentication → Authorization → Map controllers → Run
```

New middleware must be placed at the correct position in the pipeline. Read `Program.cs` to determine where.

## Structured Logging

```csharp
_logger.LogInformation("Processing request for {UserId}", userId);
_logger.LogError(ex, "Failed to process {Action}, Reason: {Reason}", action, reason);
```

- Use template parameters (NOT string interpolation)
- Include relevant context (IDs, actions, reasons)
- Mask sensitive data (passwords, tokens)

## Implementation Checklist

1. **Read Program.cs** — understand DI registration and middleware order
2. **Read existing controllers** — match route, auth, and return type patterns
3. **Read existing services** — match interface/implementation patterns
4. **Check configuration** — understand appsettings.json structure
5. **Check data access** — is it EF Core, Dapper, or something else? Match the pattern
6. **Don't change DI lifetimes** of existing services without understanding the impact
