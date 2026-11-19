# Level 5. Wrappers (decorators built on delegates)

**Status:** `AI-generated · To verify`

[← Level 4](level-4.md) · [Part 1](README.md) · [Level 6 →](level-6.md)

A higher-order function takes a function and returns a new one with added behaviour. The original code doesn't change.

Understand:

- How is a delegate-based wrapper different from the classic Decorator from Head First (a class with the same interface)? When is each more convenient?
- Why do `WithRetry(WithTiming(f))` and `WithTiming(WithRetry(f))` measure different things?
- Why is `Memoize` a bad idea for a function with side effects?

## Exercise: a library of wrappers

```csharp
public static class Wrappers
{
    public static Func<T, TResult> WithTiming<T, TResult>(
        Func<T, TResult> inner, Action<TimeSpan> report);

    public static Func<T, TResult> WithRetry<T, TResult>(
        Func<T, TResult> inner, int maxAttempts);

    public static Func<T, TResult> Memoize<T, TResult>(
        Func<T, TResult> inner) where T : notnull;

    public static Func<T, TResult> WithLogging<T, TResult>(
        Func<T, TResult> inner, Action<string> log);
}
```

Tests:

- `WithTiming`: `report` is called exactly once, even if `inner` threw (and the exception is rethrown).
- `WithRetry`: the function fails twice and succeeds on the 3rd attempt → the result is returned, 3 calls. Always fails → after `maxAttempts` calls the last exception is rethrown.
- `Memoize`: two calls with the same argument → `inner` called once; with different arguments → twice.
- `WithLogging`: the log gets “entry with argument” and “exit with result” lines in the right order.
- Composition: `WithRetry(WithTiming(f, report), 3)` with two failures calls `report` 3 times; `WithTiming(WithRetry(f, 3), report)` calls it once.

Extra: an async version `WithRetryAsync(Func<Task<T>>)` with a delay between attempts passed as a `Func<int, TimeSpan>` delegate (attempt number → delay). In tests, pass `TimeSpan.Zero` so the tests stay fast.

[Reference solution](solutions.md#level-5)

[← Level 4](level-4.md) · [Part 1](README.md) · [Level 6 →](level-6.md)
