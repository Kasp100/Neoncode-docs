[← Go back](./intro.md#15-concurrency)

# Concurrency

Modern programs often need to do things simultaneously. For example, handling user input from a GUI or communicating over a network. Concurrency allows a program to make progress on multiple tasks simultaneously. This improves performance, responsiveness, and takes advantage of modern multi-core processors.


## Multithreading

A `system` command creates threads.

```

runnable r1 = () -> { console::print_line("Running " + system: thread_name + "..."); };

thread t1 = system: start_thread(r1, "Thread 1");

```


### Example

```
pkg main;

import std::console;

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
	mutex shared mut:int_container c;

	thread::start
	(
		() ->
		{
			lock c
			{
				var int v = c.get();
				v++;
				c.set(v);
			}
		}
	);
}

```