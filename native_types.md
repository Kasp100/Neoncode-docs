[← Go back](./intro.md#2-native-types)

# Native Types

Native types are built into the language and are available without importing any modules. This page describes the types provided by the language itself.


## Default numeric types

The default numeric types are fixed-width numeric types used for general-purpose calculations.

|    Type name + aliases    |   Name description   |           Implementation notes           |
| ------------------------- | -------------------- | ---------------------------------------- |
| `nat`, `index`, `nat64`   | Natural number       | 64-bit unsigned integer                  |
| `int`, `int64`            | Integer              | 64-bit signed integer                    |
| `real`, `real64`          | Real number          | IEEE-754 binary64 floating-point number  |

**Note**: `index` is preferred over `int` for indexing.

**Number literals** can be written in decimal, hexadecimal, and binary notation.

- Decimal (base 10) is the default notation.
- Hexadecimal number literals use the prefix `0x`.
- Binary number literals use the prefix `0b`.

Example: `21` = `0x15` = `0b10101`


## Different size numeric types

These types provide numeric representations with widths different from the defaults. They are useful when memory layout, external interfaces, or specific algorithms require a particular size.

|  Type name + aliases  |        Name description        |           Implementation notes           |
| --------------------- | ------------------------------ | ---------------------------------------- |
| `nat8`, `byte`        | 8-bit natural number           | Unsigned 8-bit integer, a single byte    |
| `int8`                | 8-bit integer                  | Signed 8-bit integer                     |
| `nat16`               | 16-bit natural number          | Unsigned 16-bit integer                  |
| `int16`               | 16-bit integer                 | Signed 16-bit integer                    |
| `nat32`               | 32-bit natural number          | Unsigned 32-bit integer                  |
| `int32`               | 32-bit integer                 | Signed 32-bit integer                    |
| `real32`              | Single-precision real number   | IEEE-754 binary32 floating-point number  |


## Boolean

The `bool` type represents logical truth values.

|  Type name + aliases  |  Name description  |      Implementation notes      |
| --------------------- | ------------------ | ------------------------------ |
| `bool`, `bit`         | Boolean value      | Values are `true` and `false`  |


## Text types

Text types represent Unicode text. Character and string literals use UTF-8 encoding.

|  Type name  |   Name description   |             Implementation notes             |
| ----------- | -------------------- | -------------------------------------------- |
| `char`      | Unicode character    | Extended grapheme cluster encoded as UTF-8   |
| `string`    | Text                 | UTF-8 encoded sequence of characters         |

**Character literals** are written in between single quotes and may contain an escape sequence.

Examples: `'c'`, `'\n'`, `'🐱'`

**String literals** are written in between double quotes and can contain escape sequences.

Examples: `"This is line of text.\n"`, `"こんにちは"`


## Array types

Arrays are native collection types that store multiple values of the same type. The language provides several array forms with different ownership, size, and mutability semantics.

See [Arrays](./arrays.md#arrays).


## Generic parameter types

Generic parameters may accept different categories of compile-time values. Native generic parameter types include types and constant values.

See [Generics](./generics.md#generics).


[→ Next: Mutating Access](./mutating_access.md)