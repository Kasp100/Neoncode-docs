[← Go back](./intro.md#15-concurrency)

# Concurrency

Modern programs often need to do things simultaneously. For example, handling user input from a GUI or communicating over a network. Concurrency allows a program to make progress on multiple tasks simultaneously. This improves performance, responsiveness, and takes advantage of modern multi-core processors.

## Multithreading

A thread can be made using `std::thread`.

**API**: `thread::start(runnable)`

### `runnable`: a functional interface
```
pkg std::functional;

public interface runnable mut func
{
	void run() mut;
}
```

### Example

```
pkg main;

import std::console;
import std::runnable;
import std::thread;

entrypoint main(array<string> args)
{
	program p = ();
	thread::start(() -> p.run());
	console::print_line("Thread 1");
}

class program
{
	public void run()
	{
		console::print_line("Thread 2");
	}
}

```

## Mutex

If a mutable object is accessed from multiple threads and at least one can mutate the object, then it **must** be guarded by a mutex unless it's atomic.
This ensures thread safety.

### Example

```
pkg main;

class int_container mut
{
	var int value;

	public int get()
	{
		ret value;
	}

	public void set(int new_value) mut
	{
		value = new_value;
	}
}

entrypoint main(array<string> args)
{
	shared mutex mut:int_container c;

	thread::start(() ->
	{
		lock c
		{
			var int v = c.get();
			v++;
			c.set(v);
		}
	});
}

```