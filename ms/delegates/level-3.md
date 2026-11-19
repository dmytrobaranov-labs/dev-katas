# Level 3. Func, Action, lambdas and closures

**Status:** `AI-generated · To verify`

[← Level 2](level-2.md) · [Part 1](README.md) · [Level 4 →](level-4.md)

In modern code you rarely declare your own delegate types: there is `Func<..., TResult>` (with a result), `Action<...>` (no result) and `Predicate<T>` (an older equivalent of `Func<T, bool>`). A lambda is a short form of an anonymous method, and a closure is a lambda capturing outer variables.

Understand:

- What exactly does a lambda capture: the variable's value or the variable itself?
- What does the compiler turn a lambda that captured a local variable into? (Hint: a hidden class.)
- Why, since C# 5, does `foreach` with lambdas inside behave differently from `for`?
- What is a `static` lambda (C# 9) and what is it for?

## Exercise A: your own mini-LINQ

```csharp
public static class MyLinq
{
    public static IEnumerable<T> MyWhere<T>(this IEnumerable<T> source, Func<T, bool> predicate);
    public static IEnumerable<TResult> MySelect<T, TResult>(this IEnumerable<T> source, Func<T, TResult> selector);
    public static int MyCount<T>(this IEnumerable<T> source, Func<T, bool> predicate);
}
```

Tests:

- `MyWhere` with an even-number filter over 1..10 gives 2, 4, 6, 8, 10.
- `MySelect(x => x * x)` gives squares.
- Laziness: the predicate call counter is 0 until enumeration starts (use `yield return`).
- `null` for `source` or `predicate` throws `ArgumentNullException` immediately, not during enumeration. This is the tricky part: think about why `yield` defers the check and how to work around it.

## Exercise B: the closure trap

- Create a `List<Func<int>>` in the loop `for (int i = 0; i < 3; i++) list.Add(() => i);` and pin down with a test that all three return 3.
- Fix it by copying `i` into a local variable inside the loop and prove it with a test: 0, 1, 2.
- Write a counter `Func<int> MakeCounter()`: each call of the returned function gives 1, 2, 3… Two counters must not interfere with each other.

[Reference solution](solutions.md#level-3)

[← Level 2](level-2.md) · [Part 1](README.md) · [Level 4 →](level-4.md)
