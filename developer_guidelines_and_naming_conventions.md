[← Go back](./intro.md#8-developer-guidelines-and-naming-conventions)

# Developer Guidelines

1. Avoid using `var` if the field or variable will never be reassigned.
2. Avoid using `shared` if the object or array is never referenced elsewhere.
3. Avoid using `mut:` if the object or array will never be mutated from the scope it's referenced by.
4. Avoid using `mut` if the method does not mutate the object, or the class is an unmutable type.

# Naming Conventions
These are the proposed naming conventions to use in NeonCode.

|                 Style                  |                      Use                   |
|----------------------------------------|--------------------------------------------|
| lowercase_snake_casing                 | Variable and type names, the default style |
| UPPERCASE_SNAKE_CASING                 | Constant names                             |
| s_lowercase_snake_casing (prefix `s_`) | Names for `serialisable` types             |


[→ Next: Object Oriented](./object_oriented.md)