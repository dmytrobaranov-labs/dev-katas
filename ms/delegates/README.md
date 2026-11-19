# Part 1. Delegates, events and lambdas in C#

**Status:** `AI-generated · Human-verified up to level 0, task C, test 3`

[← Practicum overview](../README.md)

## How to work with this part

Eight levels, from a “hand-made” delegate built on an interface up to async. Each level takes 1–2 blocks of 50 minutes; the whole part takes 9–12 blocks.

The cycle for every level:

1. Read the topic in Troelsen. In the C# 5.0 / .NET 4.5 edition this is the chapter on delegates, events and lambda expressions (around chapter 10); for level 3, also the chapter on LINQ to Objects.
2. Close the book and answer the “Understand” questions at the start of the level in writing.
3. Do the exercise: tests first (xUnit), then the implementation. No peeking at the book or the reference solution.
4. Compare with the [reference solutions](solutions.md). Put differences and anything you forgot onto flashcards.

What to read for each level:

| When | Book | Why |
| --- | --- | --- |
| Before level 0 | Stellman, Head First C#, chapter 11 | Refresh the syntax, 1 block |
| Level 0, “Understand” questions | Richter, CLR via C#, 4th ed., chapter 17, the section on C#’s syntactical sugar for delegates; [sharplab.io](https://sharplab.io) | See what the compiler generates for a lambda: the closure class and the cached delegate |
| Levels 1–4 | Troelsen, chapter on delegates, events and lambdas | Main syntax reference |
| After level 3 | Richter, CLR via C#, 4th ed., chapter 17 in full; chapter 11 on events | What the compiler generates: `_target`, `_methodPtr`, `_invocationList` |
| Levels 5–6 | Head First Design Patterns, chapters 1–3; Lock, the middleware chapter | The same ideas in patterns and in ASP.NET Core |

Project setup: `dotnet new classlib -n Delegates` and `dotnet new xunit -n Delegates.Tests`, then `dotnet add Delegates.Tests reference Delegates`. All exercises live in one solution, one folder per level.

A level is done when, a day later, you can write its exercise from memory in 15–20 minutes.

## Levels

| Level | Topic | Status |
| --- | --- | --- |
| [0](level-0.md) | A delegate by hand, through an interface | `Human-verified up to task C, test 3` |
| [1](level-1.md) | A delegate as a type | `To verify` |
| [2](level-2.md) | Multicast: delegate chains | `To verify` |
| [3](level-3.md) | Func, Action, lambdas and closures | `To verify` |
| [4](level-4.md) | Events | `To verify` |
| [5](level-5.md) | Wrappers (decorators built on delegates) | `To verify` |
| [6](level-6.md) | Your own middleware pipeline | `To verify` |
| [7](level-7.md) | Async delegates and the async void trap | `To verify` |

All statuses are `AI-generated · …`; the table shows only the second half.

## Interview questions

Answer out loud in English, 1–2 minutes per question, and record yourself. A good answer: a one-sentence definition, a code example, a pitfall.

1. What is a delegate in C#, and how is it different from an interface with one method?
2. What is a multicast delegate? What happens to return values and exceptions in the invocation list?
3. What is the difference between a delegate field and an event?
4. Explain `Func`, `Action` and `Predicate`. Why do we rarely declare custom delegate types today?
5. What is a closure? Show the classic loop-variable bug and how to fix it.
6. How can events cause memory leaks, and how do you prevent them?
7. Why is `async void` dangerous? When is it acceptable?
8. How does the ASP.NET Core middleware pipeline use delegates?
9. How would you implement a retry or caching decorator without changing the original code?
10. How do you unit-test code that accepts a delegate or raises an event?

## Reference solutions

In a separate file, [solutions.md](solutions.md), so you don't see them by accident. Open it only after your own tests are green.
