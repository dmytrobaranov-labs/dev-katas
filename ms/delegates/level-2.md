# Level 2. Multicast: delegate chains

**Status:** `AI-generated · To verify`

[← Level 1](level-1.md) · [Part 1](README.md) · [Level 3 →](level-3.md)

Every delegate in .NET is multicast: `+=` creates a new delegate with an invocation list. Delegates are immutable, like strings.

Understand:

- In what order are the methods in the chain called?
- What does a chain of three `Func<int>` return? (Answer: the result of the last one.)
- What happens to the remaining methods if the second one throws?
- Why, after `d -= SomeMethod`, does a variable copied earlier still call `SomeMethod`?

## Exercise: a `SafeNotifier` that calls every subscriber even if one of them fails

```csharp
public class SafeNotifier
{
    public Action<string>? Handlers;

    // Call each handler separately and collect exceptions.
    // If there were errors -> AggregateException with all of them.
    public void Notify(string message) => throw new NotImplementedException();
}
```

Tests:

- Three handlers are called in subscription order (they write into a shared `List<string>`).
- Handler #2 throws, and #3 is still called; the result is an `AggregateException` with one inner exception.
- With no subscribers, `Notify` does nothing and doesn't crash.
- A chain of three `Func<int>` (1, 2, 3) returns 3 when called normally — a separate “demonstration” test.

Hint: `Handlers.GetInvocationList()` returns a `Delegate` array; cast each one to `Action<string>`.

[Reference solution](solutions.md#level-2)

[← Level 1](level-1.md) · [Part 1](README.md) · [Level 3 →](level-3.md)
