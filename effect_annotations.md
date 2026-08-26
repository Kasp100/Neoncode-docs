[← Go back](./intro.md#5-effect-annotations)

# Effect Annotations

Neoncode uses **effect annotations** to explicitly declare certain side effects that a function or method may perform.

The following effect annotations are available:

- `mut`: may mutate **Owned Mutations** state or reassign fields.
- `share_mut`: may mutate **Shared Mutations** objects.
- `io`: may perform **I/O (input/output) operations**.

Effect annotations are placed at the end of a function or method's parameter declarations.

## Syntax Example

```
pkg examples::effect_annotations;

class counter mut
{
	nat v = 0;

	public void increment() mut // May mutate Owned Mutations state.
	{
		v = v + 1;
	}

	public nat get_value()
}

class example
{
	shared mut:counter c;

	public void increment_counter() share_mut // May mutate Shared Mutations state.
	{
		c.increment();
	}

	public void print() io
	{
		std::console::print_line(c.value());
	}

}

```

Multiple effects may be specified when a function can perform more than one kind of side effect.

```

public void save_changes() mut io
{
	// May mutate Owned Mutations state and perform I/O.
}

```


## `mut`

The `mut` effect indicates that a function or method may mutate objects through **Owned Mutations** references or reassign fields of the object.

```

class counter mut
{
	mut:int value;

	public void increment() mut
	{
		value = value + 1;
	}
}

```

A method that mutates an Owned Mutations field must be marked with `mut`.


## `share_mut`

The `share_mut` effect indicates that a function or method may mutate objects with **Shared Mutations**.

```

class example
{
	shared mut:string s = "";

	public void add_space() share_mut // May mutate Shared Mutations state.
	{
		s.append(' ');
	}

}

```

Because Shared Mutations objects may be accessed from multiple references or threads, mutation through them is subject to the synchronization rules of Shared Mutations.

## `io`

The `io` effect indicates that a function or method may perform input/output operations.

```

public void print(string s) io
{
	std::console::print_line(s);
}

```

### Pure Functions

A function is **pure** when it has none of the `mut`, `share_mut`, or `io` effects and none of its parameters have **mutating permission** (`mut:`).

For example:

```

public int square(int x)
{
	ret x * x;
}

```

This function is pure because it does not perform I/O, mutate Owned Mutations state, mutate Shared Mutations objects, or receive a reference with mutating permission.

In contrast:

```

public void increment(mut:counter c)
{
	c.increment();
}

```

is not pure, even though it has no effect annotation, because `c` has mutating permission and can therefore be used to mutate an object.

> Effect annotations make important side effects explicit while allowing functions without such effects to be recognised as pure when their parameters also cannot provide mutation access.


[→ Next: Access Control & Imports](./access_control_and_imports.md)