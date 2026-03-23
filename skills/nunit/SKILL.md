---
name: nunit
description: This skill should be used when writing .NET tests with NUnit, NSubstitute, and FluentAssertions, or when the /doctdd command dispatches the testing-expert agent during Phase 3. Provides test structure, mocking patterns, assertion conventions, and integration test setup.
disable-model-invocation: true
version: 1.0.0
---

**NUnit + NSubstitute + FluentAssertions** technical knowledge for writing .NET tests.

## Tech Stack

- Test Framework: NUnit
- Mocking: NSubstitute
- Assertions: FluentAssertions
- Integration Testing: Microsoft.AspNetCore.Mvc.Testing (WebApplicationFactory)
- Coverage: coverlet
- Runner: `dotnet test`

## Unit Test Structure

```csharp
[TestFixture]
public class ExampleServiceTest
{
    private IExampleRepository _repository;
    private ILogger<ExampleService> _logger;
    private ExampleService _sut;

    [SetUp]
    public void Setup()
    {
        _repository = Substitute.For<IExampleRepository>();
        _logger = Substitute.For<ILogger<ExampleService>>();
        _sut = new ExampleService(_logger, _repository);
    }

    [Test]
    public async Task Should_Return_Result_When_Data_Exists()
    {
        // Arrange
        _repository.GetByIdAsync(1).Returns(new ExampleEntity { Id = 1 });

        // Act
        var result = await _sut.GetByIdAsync(1);

        // Assert
        result.Should().NotBeNull();
        result.Id.Should().Be(1);
    }

    [Test]
    public void Should_Throw_When_Id_Is_Invalid()
    {
        // Arrange & Act
        var act = () => _sut.GetByIdAsync(-1);

        // Assert
        act.Should().ThrowAsync<ArgumentException>();
    }
}
```

## NSubstitute Patterns

```csharp
// Create substitute
var service = Substitute.For<IExampleService>();

// Setup return value
service.GetByIdAsync(1).Returns(new Example { Id = 1 });

// Setup async return
service.GetByIdAsync(Arg.Any<int>()).Returns(Task.FromResult(result));

// Setup exception
service.GetByIdAsync(-1).Throws(new ArgumentException());

// Verify call received
service.Received(1).GetByIdAsync(1);

// Verify call NOT received
service.DidNotReceive().DeleteAsync(Arg.Any<int>());
```

## FluentAssertions Patterns

```csharp
// Value assertions
result.Should().Be(expected);
result.Should().NotBeNull();
result.Should().BeGreaterThan(0);

// String assertions
name.Should().StartWith("prefix");
name.Should().Contain("keyword");

// Collection assertions
list.Should().HaveCount(3);
list.Should().Contain(item);
list.Should().BeInAscendingOrder(x => x.Name);

// Object assertions
result.Should().BeEquivalentTo(expected);

// Exception assertions
action.Should().Throw<ArgumentException>()
    .WithMessage("*invalid*");
await asyncAction.Should().ThrowAsync<InvalidOperationException>();
```

## Parameterized Tests

```csharp
[TestCase("2022-07-01", "2023-08-01", ExpectedResult = true)]
[TestCase("2023-01-01", "2022-12-31", ExpectedResult = false)]
public bool Should_Validate_DateRange(string start, string end)
{
    return DateHelper.IsValid(DateTime.Parse(start), DateTime.Parse(end));
}

[TestCaseSource(nameof(GetTestCases))]
public void Should_Process_Correctly(TestInput input, Expected expected)
{
    var result = _sut.Process(input);
    result.Should().BeEquivalentTo(expected);
}
```

## Integration Test Structure

```csharp
[TestFixture]
public class ExampleControllerTest : CustomWebApplicationFactory
{
    [SetUp]
    public void Setup()
    {
        // Setup test context (mock user, auth, etc.)
    }

    [Test]
    public async Task Should_Return_Success_When_Valid_Request()
    {
        // Arrange — setup test data, mock external dependencies

        // Act — call API endpoint via HttpClient
        var response = await Client.GetAsync("/api/Example/Get");

        // Assert
        response.StatusCode.Should().Be(HttpStatusCode.OK);
        var content = await response.Content
            .ReadFromJsonAsync<ExampleResponse>();
        content.Should().NotBeNull();
    }
}
```

**WebApplicationFactory pattern:**
- Inherits from `WebApplicationFactory<Program>`
- Override `ConfigureWebHost` to replace services with test doubles
- Use `ConfigureTestServices` to inject mocks
- Provides `HttpClient` for calling API endpoints

## Test Project Structure (Typical)

```
ProjectName.UnitTests/
├── Controllers/        # Controller unit tests
├── Services/           # Service unit tests
├── Helpers/            # Helper/utility tests
└── Models/             # Model/DTO tests

ProjectName.IntegrationTests/
├── ApiTest/            # API endpoint tests
├── Base/               # Test infrastructure
│   ├── CustomWebApplicationFactory.cs
│   └── RepositoryBaseTest.cs
├── Helpers/            # Test utilities
└── GlobalSetup.cs      # One-time setup
```

## Test Naming Convention

Use descriptive names that express the scenario:

```csharp
// Pattern: Should_{Expected}_{When/Given}_{Condition}
Should_Return_UserInfo_When_User_Is_Authenticated()
Should_Throw_BadRequest_When_Input_Is_Invalid()
Should_Not_Call_Repository_When_Cache_Hit()
```

## Implementation Checklist

1. **Read existing test files** — match the project's conventions exactly
2. **Check test infrastructure** — look for base classes, helpers, factories
3. **Match DI setup** — understand how services are mocked in test setup
4. **One test per AC** (minimum) — cover both positive and negative cases
5. **Tests must be independent** — no shared mutable state, no order dependency
6. **Run with `dotnet test`** — verify tests execute correctly
