[← Go back](./intro.md#3-primitives)

# Primitives
Here's a full list of Neoncode's primitive types:

## Table

|    Type Name + Aliases    |       Name Description       |                          Implementation Description                          |
| ------------------------- | ---------------------------- | ---------------------------------------------------------------------------- |
| `char`                    | Character                    | Extended grapheme cluster in UTF-8, representing a user-perceived character  |
| `nat`, `index`, `nat64`   | Natural number (64 bit)      | 64 bit unsigned integer                                                      |
| `nat32`                   | Natural number (32 bit)      | 32 bit unsigned integer                                                      |
| `nat16`                   | Natural number (16 bit)      | 16 bit unsigned integer                                                      |
| `byte`, `nat8`            | Byte, natural number (8 bit) | Single byte, 8 bit unsigned integer                                          |
| `int`, `int64`            | Integer (64 bit)             | 64 bit signed integer                                                        |
| `int32`                   | Integer (32 bit)             | 32 bit signed integer                                                        |
| `int16`                   | Integer (16 bit)             | 16 bit signed integer                                                        |
| `int8`                    | Integer (8 bit)              | 8 bit signed integer                                                         |
| `real`, `real64`, `fp64`  | Real number (64 bit)         | Floating point number with double precision (64 bit)                         |
| `real32`, `fp32`          | Real number (32 bit)         | Floating point number with single precision (32 bit)                         |
| `real16`, `fp16`          | Real number (16 bit)         | Floating point number with half precision (16 bit)                           |
| `bool`, `bit`             | Boolean, bit                 | Single bit                                                                   |

**Notes**
- `nat` stands for **natural number**.
- `index` is preferred over `int` for indexing.

## Notation

**Character literals** are written in between single quotes and may contain an escape sequence.

Example: `'C'`

**Number literals** can be written in decimal, hexadecimal, and binary notation.

Example: `21` = `0x15` = `0b10101`

**Boolean literals** can be `true` or `false`.

[→ Next: Immutability and Const by Default](./immutability_and_const_by_default.md)