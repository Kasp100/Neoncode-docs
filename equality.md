[← Go back](./intro.md#17-equality)

# Equality

If a type implements the interface `equatable`, the `==` operator can be used with it.

## `equatable` interface

```neoncode
pkg std;

public interface equatable
{
	/** Check whether this object is conceptually equal to the other. */
	bool equals(object other);
}

```

## Operator implementation

The operator (`==`) is implemented using automatically applied [custom expression grammar](custom_expression_grammar.md).

There's no need to explicitly use `parse`.

```
pkg std::default_expression_grammar;

public expression_grammar equals_operator
{
	1 bool (equatable lhs) == (object rhs)
	{
		ret lhs.equals(rhs);
	}

	1 bool (equatable lhs) != (object rhs)
	{
		ret !lhs.equals(rhs);
	}
}

```