[← Go back](./intro.md#8-developer-guidelines-and-naming-conventions)

# Developer Guidelines

1. Avoid using `var` if the field or variable will never be reassigned.
2. Avoid using `shared` if the object or array is never referenced elsewhere.
3. Avoid using `mut:` if the object or array will never be mutated from the scope it's referenced by.
4. Avoid using `mut` if the method does not mutate the object, or the class is an unmutable type.

# Naming Conventions
These are the proposed naming conventions to use in NeonCode.

|                      Use                      |          Style          |             Examples             |
| --------------------------------------------- | ----------------------- | -------------------------------- |
| Default: package members, type members, etc.  | Lowercase snake casing  | `vehicle`, `speed`, `get_speed`  |
| Constant names, generic parameters *          | Uppercase snake casing  | `MAX_SPEED`, `<type T>`          |

\* With generic parameters, it is conventional to keep use a single character if it is clear.

For example:
- Default: `T`
- Element type in a collection: `E`
- Length of an array: `L`


[→ Next: Object Oriented](./object_oriented.md)