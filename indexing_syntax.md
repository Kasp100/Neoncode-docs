[← Go back](./intro.md#18-indexing-syntax)

# Indexing Syntax

Allowing indexing using square brackets on a type requires it implementing the interface `indexable`.

## `indexable` interface and grammar

```neoncode
pkg std;

public interface indexable<type key_type, type value_type> mut
{
	value_type get(key_type key);
}

```

## Operator implementation

The operator (`[]`) is implemented using automatically applied [custom expression grammar](custom_expression_grammar.md).

There's no need to explicitly use `parse`.

```neoncode
std::default_expression_grammar;

public expression_grammar indexing_brackets
{
	0 <type key_type, type value_type> value_type (indexable<key_type, value_type>)[(key_type key)]
	{
		ret indexable.get(key);
	}
}

```

