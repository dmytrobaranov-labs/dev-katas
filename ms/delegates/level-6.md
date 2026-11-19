# Level 6. Your own middleware pipeline

**Status:** `AI-generated · To verify`

[← Level 5](level-5.md) · [Part 1](README.md) · [Level 7 →](level-7.md)

In ASP.NET Core a request goes through a chain of `RequestDelegate`s: each middleware receives `next` and decides what to do before it, after it, or whether to call it at all. Here you build such a pipeline yourself, without ASP.NET, and afterwards Lock's middleware chapters will read easily.

Understand:

- Why is the pipeline built from the end, with the last component wrapped first?
- What does “short-circuit” mean and where is it needed (authorization, cache)?
- Why does the order of `app.UseAuthentication()` and `app.UseAuthorization()` matter?

## Exercise

```csharp
public class Context
{
    public string Path { get; init; } = "/";
    public List<string> Log { get; } = new();
    public int StatusCode { get; set; } = 200;
}

public delegate Task RequestDelegate(Context ctx);

public class PipelineBuilder
{
    public PipelineBuilder Use(Func<RequestDelegate, RequestDelegate> middleware);
    // Convenience overload, as in ASP.NET Core:
    public PipelineBuilder Use(Func<Context, Func<Task>, Task> middleware);
    public RequestDelegate Build(); // at the end of the chain — StatusCode = 404
}
```

Tests:

- Three middlewares write “A-in”, “B-in”, “C-in” to `Log` before `next` and “C-out”, “B-out”, “A-out” after. Check the exact order of all six lines.
- An authorization middleware: for `Path = "/admin"` it sets 401 and doesn't call `next`; the following middlewares write nothing to the log.
- An empty pipeline returns 404.
- An error-handling middleware first in the chain: catches the exception from the third one, sets 500, and the log contains “error handled”.

Extra: move the timing wrapper from level 5 into this pipeline and think about where to put it to measure the whole request.

[Reference solution](solutions.md#level-6)

[← Level 5](level-5.md) · [Part 1](README.md) · [Level 7 →](level-7.md)
