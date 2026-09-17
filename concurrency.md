[← Go back](./intro.md#concurrency)

# Concurrency

Modern programs often need to do things simultaneously. For example, handling user input from a GUI or communicating over a network. Concurrency allows a program to make progress on multiple tasks simultaneously. This improves performance, responsiveness, and takes advantage of modern multi-core processors.

Objects shared across threads must be synchronised.


## Multi-Thread Objects (`multi_thread`)

A multi-thread object is an object that is shared across threads.

**Syntax**: The keyword `multi_thread` declares that an object can be shared across threads.
The keyword should be placed before the [mutation ownership type](./mutation_ownership.md#mutation-control-levels) of the reference.

E.g., `multi_thread shared mut:bank_account`, `multi_thread repository get_repository()`


## Locking (`lock`, `unlock`)

Operations on a multi-thread object require synchronisation using a **lock**, though some operations may be [defined as multi-thread](#multi-thread-methods), removing the requirement for an external lock.

While an object is locked, mutations from other threads wait. If the object is mutated while locked, read operations from other threads also wait.

The implementation of locks is compiler-defined. The compiler may use any mechanism that satisfies the semantics of lock.

**Syntax**:

- `lock obj` locks `obj`.  
  The compiler automatically chooses between read lock and write lock.  
  If the object (`obj`) is mutated during the lock, a write lock is inferred.

- `unlock obj` unlocks `obj`.  
  Every lock must definitely be matched by an unlock on every control-flow path in the code block.  
  With multiple locks, each object must be unlocked in **reverse order**.

Example:

```

multi_thread shared mut:bank_account b = ();

lock b;

b.deposit(100);

unlock b;

```


## Multi-Thread Methods

Multi-thread methods internally handle synchronisation. They represent operations that **do not require locking by the caller**.

Fields can be accessed in these methods if either of the following requirements is met:
- the field is multi-thread, or
- the object itself is [locked](#locking-lock-unlock) (`lock self`).

**Syntax:** `multi_thread` before `mut` in a type declaration.

Example:

```

abstract type repository<type K, type V> mut
{
	result<K, repository_err> create(own V value) multi_thread mut io;

	void save(own K key, own V value) multi_thread mut io;

	result<V, repository_err> delete(own K key) multi_thread mut io;

	result<V, repository_err> get(own K key) multi_thread io;
}

```


## Working with Threads

A few `system` commands allow for multithreading.

- Starting a thread: `thread_handle system: start_thread(func{void() mut io} runnable)`
- Wait for a thread to finish: `void system: join_thread(thread_handle thread_to_wait_for)`


Examples:

```

func{void() io} r1 = func: () -> { console::print_line("New thread"); };

thread_handle t1 = system: start_thread(r1);

```


```
pkg main;

import std::console;

void main(array<string> args) io
{
	multi_thread shared mut:counter c = ();

	func{void() share_mut io} r = func: () ->
	{
		lock c;

		c.count();

		console::print_line(c.get_value());

		unlock c;
	};

	system: start_thread(r);
}

type counter mut
{
	var nat v = 0;

	public void count() mut
	{
		++v;
	}

	public nat get_value()
	{
		ret v;
	}

}

```


[→ Next: Equality](./equality.md)