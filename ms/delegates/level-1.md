# Level 1. A delegate as a type

**Status:** `AI-generated · To verify`

[← Level 0](level-0.md) · [Part 1](README.md) · [Level 2 →](level-2.md)

A delegate is a type that describes a method signature. An instance holds a reference to a method (and to an object, if the method is not static), and you can call it.

Understand (in writing, without the book):

- What does the compiler turn the line `delegate int BinaryOp(int a, int b);` into? Which methods does that class have?
- How is `op(2, 3)` different from `op.Invoke(2, 3)`?
- What is method group conversion: `BinaryOp op = Add;` instead of `new BinaryOp(Add)`?
- What do the `Target` and `Method` properties hold for a static method and for an instance method?

## Exercise: a calculator on a dictionary of delegates

```csharp
public delegate int BinaryOp(int a, int b);

public class Calculator
{
    // Register an operation under a symbol: "+", "*", etc.
    public void Register(string symbol, BinaryOp op) { }
    // Execute; an unknown symbol -> InvalidOperationException
    public int Execute(string symbol, int a, int b) => throw new NotImplementedException();
}
```

Tests to write first:

- `Execute("+", 2, 3)` returns 5 after registering the static method `Add`.
- An operation registered with the lambda `(a, b) => a * b` works the same way.
- An unknown symbol throws `InvalidOperationException`.
- Registering an instance method: a `Multiplier` object with a `factor` field — check that the delegate's `Target` is that object, and that for a static method `Target == null`.

Extra: replace `BinaryOp` with `Func<int, int, int>` and make sure the tests pass with no other changes.

[Reference solution](solutions.md#level-1)

[← Level 0](level-0.md) · [Part 1](README.md) · [Level 2 →](level-2.md)
