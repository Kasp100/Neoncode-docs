[← Go back](./intro.md#15-concurrency)

# Concurrency

Modern programs often need to do things simultaneously. For example, handling user input from a GUI or communicating over a network. Concurrency allows a program to make progress on multiple tasks simultaneously. This improves performance, responsiveness, and takes advantage of modern multi-core processors.

Objects shared across threads must be thread-safe or synchronised.


## Synchronised Access

A non-thread-safe object can be shared across threads only with [synchronised access](#synchronised-objects-sync).


### Synchronised Objects (`sync`)

A synchronised object requires synchronised access for read and write operations.

**Syntax**: The keyword `sync` placed between `shared` and `mut:` declares that an object is synchronised.

E.g., `shared sync mut:bank_account`, `shared sync bank_account`


### Locking (`lock`, `unlock`)

Read and write operations with a synchronised object require synchronisation using a **lock**.

While an object is locked, mutations from other threads wait.
If the object is mutated during the lock, reading operations from other threads also wait.

**Syntax**:

- `lock obj` locks `obj`.  
  The compiler automatically chooses between read lock and write lock.  
  If the object (`obj`) is mutated during the lock, a write lock is inferred.

- `unlock obj` unlocks `obj`.  
  Every lock must definitely be matched by an unlock on every control-flow path in code block.  
  With multiple locks, each object must be unlocked in **reverse order**.


### Example

```

shared sync mut:bank_account b = ();

lock b;

b.deposit(100);

unlock b;

```


## Thread-Safe Types

Thread-safe types internally handle synchronisation. Instances of these types do not need external synchronisation.

**Syntax**: `thread_safe` before curly brackets in type declaration, among `mut` and `io`.


### Example

```

interface repository<type K, type V> mut thread_safe
{
	result<K, repository_err> create(own V value) mut io;

	void save(own K key, own V value) mut io;

	result<V, repository_err> delete(own K key) mut io;

	result<V, repository_err> get(own K key) io;
}

```


## Multithreading

A few `system` commands allow for multithreading.

- Starting a thread: `thread_handle system: start_thread(func{void() mut io} runnable)`
- Wait for a thread to finish: `void system: join_thread(thread_handle thread_to_wait_for)`


Example:

```

func{void() io} r1 = func: () -> { console::print_line("New thread"); };

thread_handle t1 = system: start_thread(r1);

```


### Example

```
pkg main;

import std::console;

void main(array<string> args) io
{
	shared sync mut:counter c = ();

	func{void() shared_mut io} r = func: () ->
	{
		lock c;

		c.count();

		console::print_line(c.get_value());

		unlock c;
	};

	system: start_thread(r);
}

class counter mut
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