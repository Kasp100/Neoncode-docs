[← Go back](./intro.md#14-functional--lambda)

# Functional Programming & Lambda

## Functions & Closures

`func` is a native type that holds a function (or closure).

A closure keeps a captured environment of [Shared Mutations](./mutation_ownership.md#shared-mutations-shared) references.
Mutations to objects behind these references are not considered mutations of the captured environment.

The return type, optional generic parameters, parameter types enclosed in `()`, optional IO marking, and optional mutating marking are enclosed in `{}` after `func`.
Example: `func{void <type T> (T) mut io}`

If the mutating marking (`mut`) is included, it means this function mutates its captured environment. [This is like how mutating method are declared in types.](./mutating_access.md#mutable-types-and-mutating-methods)

Calling `func.run(...)` on the function runs it in the current thread. See [Concurrency](./concurrency.md) on how to run it in a new thread.


## Lambda

TODO


## Examples


### Comparator

```
pkg main;

import std::console;

void main(array<string> args) io
{
	func{T <subtype T { std::comparable }> (T a, T b)} comparator = () ->
	{
		ret a < b;
	};

	console::print_line(comparator.run(7, 5)); // false

	console::print_line(comparator.run(0, 2)); // true
}

```


### Counter

```
pkg main;

import std::console;

void main(array<string> args) io
{
	var shared int n = 0;

	mut:func{void() mut} r = () ->
	{
		n++;
	};

	console::print_line(n); // 0

	r.run();

	console::print_line(n); // 1

	r.run();

	console::print_line(n); // 2
}

```


[→ Next: Concurrency](./concurrency.md)