[← Go back](./intro.md#14-functional--lambda)

# Functional Programming

## Functional Interfaces

Neoncode has functional interfaces. These allow implementations to be written like lambda expressions.

Here are some functional interfaces included in the standard library:

```
pkg std::functional;

// Without side effects

public interface predicate<type T> func
{
	pure bool check(T value);
}

public interface mapper<type T, type R> func
{
	pure R map(T value);
}

public interface comparator<type T> func
{
	pure int compare(T a, T b); // -1, 0, +1
}

// With side effects

public interface runnable mut func
{
	void run() mut;	
}

public interface supplier<type R> mut func
{
	R get() mut;
}

public interface function<type T, type R> mut func
{
	R apply(T value) mut;
}

```

# Lambda

Lambda allows anonymous implementations of functional interfaces.

## Example

```
pkg main;

import std::console;
import std::container;
import std::conversions;

entrypoint main(array<string> args)
{
	shared mut:container<int> c = (0);

	runnable r = () mut ->
	{
		var int i = c.get();
		i++;
		c.set(i); // Because "c" is mutated here, shared mutability is needed with "c".
	};

	console::print_line(conversions::number_to_string(c.get())); // 0
	r.run();
	console::print_line(conversions::number_to_string(c.get())); // 1
	r.run();
	console::print_line(conversions::number_to_string(c.get())); // 2
}

```


[→ Next: Concurrency](./concurrency.md)