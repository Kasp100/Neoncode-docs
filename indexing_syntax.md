[← Go back](./intro.md#17-indexing-syntax)

# Indexing Syntax

Allowing indexing using square brackets on a type requires it implementing the interface `indexable`.

Example: getting the 8th element from an array

We define an array: `array<string> strings = ("ab", "cd", "ef", "gh", "ij");`

Now, `strings[3]` will give the **fourth** element from the array, which will be "gh".


## Time Complexity Note

The `[]` syntax implies constant-time (`O(1)`) lookup, so indexable types that do not guarantee this use a `get(index)` method.


## Operator implementation

The operator (`[]`) is implemented using default [operator functions](operators_and_operator_function_sets.md).

There's no need to explicitly write `use`.
