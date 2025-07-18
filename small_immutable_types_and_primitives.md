[← Go back](./intro.md#3-small-immutable-types--primitives)

# Small Immutable Types and Primitives

## Small Immutable Types

In Neoncode, immutable types with a memory footprint smaller than or equal to 64 bits are considered **small immutable types**.

This means they fit to be passed by value. The compiler optimises these types accordingly.

### Small contract types (`small`)

If a type is marked with `small`, it signals that the type is meant to be a **small immutable type**. The compiler will enforce this.

> Note: This disallows inheritance.
> It's only applicable for **concrete types** (i.e. classes)

## Primitives
Primitives are built-in, small, pass-by-value, and serialisable.

Here's a full list of Neoncode's primitives:

### Table

|  Type Name  |     Aliases     |                      Description                      |
|-------------|-----------------|-------------------------------------------------------|
| `uint`      | `nat`, `index`  | 64 bit unsigned integer                               |
| `int`       |                 | 64 bit signed integer                                 |
| `char`      |                 | 32 bit single character in UTF-32                     |
| `bool`      | `bit`           | Boolean, with values `true` or `false`                |
| `fp16`      | `half`          | Floating point number with half precision (16 bit)    |
| `fp32`      | `float`         | Floating point number with single precision (32 bit)  |
| `fp64`      | `double`        | Floating point number with double precision (64 bit)  |
| `uint64`    | `nat64`         | 64 bit unsigned integer                               |
| `int64`     |                 | 64 bit signed integer                                 |
| `uint32`    | `nat32`         | 32 bit unsigned integer                               |
| `int32`     |                 | 32 bit signed integer                                 |
| `uint16`    | `nat16`         | 16 bit unsigned integer                               |
| `int16`     |                 | 16 bit signed integer                                 |
| `uint8`     | `nat8`, `byte`  | Single byte, 8 bit unsigned integer                   |
| `int8`      |                 | 8 bit signed integer                                  |

**Notes**
- "nat" stands for **natural number**
- Prefer `index` over `int` when talking about an index in an array or different collection.

## Numeric Types

All built-in primitive types are **numeric** except for booleans (`bool`), characters (`char`).  
Numeric means that arithmetic operations can be performed on them by default.
(You can also create custom grammar allowing you to create custom numeric types, whether they're primitive or not.)

## Notation

Each of the numeric types can be written in decimal, hexadecimal, and binary notation.
E.g. `21` = `0x15` = `0b10101`

Booleans can be `true` or `false`.
Characters can be any character in UTF-32.

[→ Next: Immutability and Const by Default](./immutability_and_const_by_default.md)