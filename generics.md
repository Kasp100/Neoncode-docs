[← Go back](./intro.md#18-generics)

# Generics

In Neoncode, generics allow types, methods, pure functions, and operator functions to be implemented with different types and constant values.

Types, methods, pure functions, and operator functions can declare **generic parameters**, while users must provide valid **generic arguments**.

Generic parameter declarations and arguments are enclosed in `<>`.

## Parameters and arguments

Here are four examples of generic parameters and arguments.

|        Parameter Declaration        |    Argument Examples    |                                    Notes                                    |
| ----------------------------------- | ----------------------- | --------------------------------------------------------------------------- |
| `<type T>`                          | `<nat>`, `<string>`     | `T` can be any type, but is syntactically treated as a mutable type.        |
| `<subtype A {animal, water_float}>` | `<duck>`, `<sea_otter>` | `A` must be a subtype of `animal` **and** `water_float`.                    |
| `<nat LENGTH>`                      | `<8>`, `<0>`            | Literal can be substituted with a constant reference of the required type.  |
| `<bool BOUNDED>`                    | `<true>`, `<false>`     | Code that is unreachable will not be generated due to optimisations.        |

The first two are generic **type paramters**, while the second two are generic **value parameters**.

### Type parameter kinds

- Type: `type TYPE_PARAM_NAME`
- Subtype: `subtype TYPE_PARAM_NAME {SUPERTYPE_A, SUPERTYPE_B, SUPERTYPE_C}`

### Value parameter kinds

- [Any default numeric types](./native_types.md#default-numeric-types)
- [Any different size numeric types](./native_types.md#different-size-numeric-types)
- [Boolean](./native_types.md#boolean)


[→ Next: Optional](./optional.md)