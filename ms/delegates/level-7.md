# Level 7. Async delegates and the async void trap

**Status:** `AI-generated · To verify`

[← Level 6](level-6.md) · [Part 1](README.md)

An async lambda passed where an `Action` is expected becomes `async void`: nobody awaits it and nobody catches its exception. Where a `Func<Task>` is expected, you get a normal `async Task`. This is a favourite interview topic.

Understand:

- What will `Action a = async () => { await Task.Delay(10); throw new Exception(); };` turn into? What happens when you call `a()`?
- Why are event handlers the only place where `async void` is acceptable?
- How do you raise an event and wait for all asynchronous subscribers?

## Exercise A: prove the problem with tests

- A method `RunAll(IEnumerable<Action> actions)` calls each one. Pass an async lambda with `await Task.Delay(50)` and a write into a list. A test right after `RunAll` shows the entry isn't there yet.
- Write `RunAllAsync(IEnumerable<Func<Task>> actions)` with `await Task.WhenAll(...)`. Same scenario: the entry is there right after `await`.
- An exception from an async lambda in `RunAllAsync` reaches the test (`Assert.ThrowsAsync`).

## Exercise B: an asynchronous event

```csharp
public class AsyncOrder
{
    // A custom type instead of EventHandler: subscribers return Task
    public event Func<AsyncOrder, Task>? PaidAsync;

    // Call all subscribers and wait for all of them
    public Task PayAsync() => throw new NotImplementedException();
}
```

Tests:

- Two subscribers with different delays; after `await PayAsync()` both have finished.
- One subscriber throws — the exception reaches the test, and the second subscriber still ran.

Hint: `GetInvocationList()` from level 2, `Cast<Func<AsyncOrder, Task>>()`, `Task.WhenAll`.

[Reference solution](solutions.md#level-7)

Next: the [interview questions](README.md#interview-questions) for the whole part.

[← Level 6](level-6.md) · [Part 1](README.md)
