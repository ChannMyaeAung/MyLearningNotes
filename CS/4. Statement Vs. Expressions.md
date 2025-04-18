# Expressions Vs. Statements

In programming languages like C and Go, understanding the distinction between **statements** and **expressions** is crucial for writing effective code.

**Expressions**:

- An expression is a combination of variables, constants, operators, and function calls that the language evaluates to produce a value.
- Expressions can be used wherever a value is expected, such as in assignments or function arguments.
- **Examples**:
  - `3 + 4` evaluates to `7`.
  - `x * y` results in the product of `x` and `y`.
  - `func(a, b)` calls `func` with `a` and `b` and returns a value.

**Statements**:

- A statement performs an action and may not produce a value.
- Statements control the flow of execution and can include expressions within them.
- **Examples**:
  - `if (condition) { /* ... */ }` executes a block of code based on a condition.
  - `for (int i = 0; i < n; i++) { /* ... */ }` iterates over a block of code multiple times.
  - `int x = 5;` declares a variable and initializes it.

**Key Differences**:

- **Evaluation**: Expressions are evaluated to yield a value, whereas statements are executed to perform an action and may not yield a value.
- **Usage**: Expressions can be part of statements (e.g., in assignments or conditions), but statements cannot be used as expressions.

**Language-Specific Nuances**:

- **C**:
  - In C, the increment (`++`) and decrement (`--`) operations can be used as both prefix and postfix expressions, and they yield values. For example, `int y = ++x;` increments `x` and assigns the result to `y`.
- **Go**:
  - In Go, the increment (`++`) and decrement (`--`) operations are statements, not expressions. They do not return a value and cannot be used in expressions. For instance, `y = x++` is invalid in Go.