# dev-katas

Development labs by [Dmytro Baranov](https://www.linkedin.com/in/dmytrobaranov/): katas, practicums and small exercises, grouped by stack.

| Folder | Stack | What's inside |
|---|---|---|
| [`ms/`](ms/) | C# / .NET | TDD katas, C# and ASP.NET Core practicums |
| [`py/`](py/) | Python | Algorithms, math and ML exercises |
| [`go/`](go/) | Go | Go exercises |

Each lab lives in its own subfolder with a short README: the goal, how to run it and what I learned.

Infrastructure and cloud labs live in the sibling repository [`ops-katas`](https://github.com/dmytrobaranov-labs/ops-katas).

## Running the tests

```bash
# .NET
cd ms && dotnet test

# Python
cd py && pytest

# Go
cd go && go test ./...
```

CI runs only for the folder that changed.
