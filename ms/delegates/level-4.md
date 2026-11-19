# Level 4. Events

**Status:** `AI-generated · To verify`

[← Level 3](level-3.md) · [Part 1](README.md) · [Level 5 →](level-5.md)

An event (`event`) is a delegate field that outsiders can only subscribe to (`+=`) and unsubscribe from (`-=`). Only the owning class can raise it or overwrite it with `=`. It is the Observer pattern built into the language.

Understand:

- What breaks if you write `public Action? Paid;` instead of `public event Action? Paid;`? Give two concrete abuses.
- Why the standard signature `(object? sender, TEventArgs e)` and the `EventHandler<T>` class?
- Why call `Paid?.Invoke(this, args)` rather than `if (Paid != null) Paid(this, args)`? (Hint: threads.)
- How does subscribing to an event of a long-lived object lead to a memory leak?

## Exercise: an order with a payment event

```csharp
public class OrderPaidEventArgs : EventArgs
{
    public required int OrderId { get; init; }
    public required decimal Amount { get; init; }
}

public class Order(int id, decimal amount)
{
    public event EventHandler<OrderPaidEventArgs>? Paid;
    public bool IsPaid { get; private set; }

    // Pay; paying again -> InvalidOperationException, the event is not raised
    public void Pay() => throw new NotImplementedException();
}
```

Tests:

- After `Pay()` the event is raised once, `sender` is this order, and the arguments carry the correct `OrderId` and `Amount`. Use `Assert.Raises<OrderPaidEventArgs>(h => order.Paid += h, h => order.Paid -= h, () => order.Pay())`.
- With no subscribers, `Pay()` doesn't crash.
- After unsubscribing, the handler is no longer called (the call counter stays 0).
- A second `Pay()` throws, and there is no second event.

Extra: subscribers `EmailSender` and `InventoryService` in separate classes. Test that both react to a single payment and that `Order` knows nothing about them.

[Reference solution](solutions.md#level-4)

[← Level 3](level-3.md) · [Part 1](README.md) · [Level 5 →](level-5.md)
