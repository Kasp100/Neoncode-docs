[← Go back](./intro.md#16-equality)

# Equality

If a type implements the interface `equatable`, the `==` operator can be used with it.

## `equatable` interface

```neoncode
pkg std;

public interface equatable mut
{
	/** Check whether this object is conceptually equal to the other. */
	bool equals(equatable other);
}

```

## Operator implementation

The operator (`==`) is implemented using **default** [operator functions](operators_and_operator_function_sets.md).

There's no need to explicitly write `use`.
