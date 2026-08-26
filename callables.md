[← Go back](./intro.md#14-callables)

# Callables

`func` or "callable" is a generic native type that represents a function.

A callable may contain a **captured environment**: local variables or parameters from their [construction site](#callable-expressions).


## Types of callables

Types of callables are distinguished by their **generic arguments**.
These generic arguments represent its **function properties**, including return type, parameter types, etc.


### Syntax

A callable type is written like `func` followed by its **generic arguments** enclosed in `{}`.


#### Generic arguments structure

The structure of `func`'s generic arguments is like method declarations, but without code block, method name and parameter names.

1. Return type, including mutation control level and mutating permission
2. Generic parameters (optional)
3. Parameters enclosed in `()`, without names
4. Effect annotations for the state of its captured environment:
	- `mut` if this callable mutates its captured environment.
	- `share_mut` if this callable mutates [shared](./mutation_ownership.md#shared-mutations-shared) objects through its captured environment.
	- `io` if this callable may perform I/O.


## Running callables

Calling `func.run(...)` on the callable runs it in the current thread. See [Concurrency](./concurrency.md) on how to run it in a new thread.


## Callable expressions

Expressions that produce callables start with `func`, followed by generic arguments enclosed in `{}` (optional if it would be repeated), and a `:`, and end with a lambda expression or function reference.


### Lambda expression

A lambda expression looks like `( /* parameter names */ ) -> { /* code */ }`.

- Parameter names are enclosed in `()`, separated by `,`. The `()` are not required if there is less than two parameters.
- The `{}` enclose the code of this callable. These brackets are optional if the block contains a single statement.


### Function reference

A function reference looks like `expression.function_name` or `static_function_identifier`.

- `expression` is any expression that gives an object on which `function_name` can be called with (generic) arguments matching the callable type, e.g. `s.append`.
- `static_function_identifier` is a reference to any static function or constructor, e.g. `std::math::add` or `array`.

Depending on the callable type, providing generic arguments may be required.


### Shorter syntax

`func{int (int, int)} f_add = func{int (int, int)}: (a, b) -> ret a + b;`

can be written like

`func{int (int, int)} f_add = func: (a, b) -> ret a + n;`

because the generic arguments are already visible.


## Closures

The state of a callable is composed of just a **captured environment**: local variables or parameters from the callable's construction site which are used in the callable's code.

A callable which uses local variables or parameters from its construction site is called a "closure".

These local variables or parameters must use an immutable type or be [Shared Mutations (`shared`)](./mutation_ownership.md#shared-mutations-shared) references with optional mutating permission.

The consequence of this is that the only possible mutations of callable would be reassigning a local variable or parameter from the construction site. 


## Examples


### 1. Callable Expression

```
pkg examples::callables;

func{int ()} get_callable()
{
	ret func{int ()}: () -> { ret 7; }
}

```


### 2. Comparator

```
pkg examples::callables;

import std::console;

void main(array<string> args) io
{
	func{T <type T>(T, T)} comparator = func: (a, b) ->
	{
		ret a < b;
	};

	console::print_line(comparator.run<int>(7, 5)); // false

	console::print_line(comparator.run<int>(0, 2)); // true
}

```


### 3. Counter

```
pkg examples::callables;

import std::console;

void main(array<string> args) io
{
	var int n = 0;

	mut:func{void() mut} counter = func: () ->
	{
		n++; // "n" is part of counter's captured environment
	};

	console::print_line(n); // 0

	counter.run();

	console::print_line(n); // 1

	counter.run();

	console::print_line(n); // 2
}

```


### 4. String Manipulation

```
pkg examples::callables;

import std::console;

void main(array<string> args) io
{
	shared mut:string s = "*";

	func{void() share_mut} string_manipulator = func: () ->
	{
		s.append('*'); // "s" is part of string_manipulator's captured environment
	};

	console::print_line(s); // *

	string_manipulator.run();

	console::print_line(s); // **

	string_manipulator.run();

	console::print_line(s); // ***
}

```


[→ Next: Concurrency](./concurrency.md)