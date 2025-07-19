[← Go back](./intro.md#5-types-of-references)

# Types of References

Mutations to objects of mutable types are controlled through various keywords.

> In some other languages, the concept of ownership and borrowing is about **who is reponsible for deallocating them**, here it is about **who can mutate and who cannot**.

> When talking about a **mutable reference**, a reference declared with `mut:` is meant.

**As described in [Immutability and Const by Default](./immutability_and_const_by_default.md), a `mut:` reference is needed to mutate an object or to pass it to other `mut:` references.**

**`own`** means the object **cannot** be mutated from elsewhere unless borrowed. This is default in **fields** and **locals**.  
**`shared`** means the object **can** be mutated from elsewhere.  
**`borrow`** means the object **can temporarily** be mutated here (if `mut:` is present). This is default for **parameters** and **return values**.

`own`, `shared`, and `borrow` **cannot** be used with **immutable types**, as these concepts are reduntant when the data is immutable.

## Defaults

- **Fields** and **local variables** are `own` by default, without keyword, but can be set `shared`
- **Parameters** and **return values** are `borrow` by default, but can be set explicitly `own` or `shared`

## Controlled Mutations (`own`)

The referenced object can be **viewed** elsewhere (by whoever has a reference), but can only be **mutated** here (if `mut:` is used, by calling mutating methods), can be [**borrowed**](#borrowed-borrow).

- Mutating the referenced object is considered mutating the container.
- Can be freely passed to immutable `shared` references.
- `own` is often paired with `mut:` (to mutate a mutable type)

## Mutably Shared (`shared`) 

The referenced object can be mutated from elsewhere.

> This is the most permissive type of reference. It carries the most risk.

- Mutating the referenced object is **not** considered mutating the container.
- Can be freely passed to immutable **and mutable** `shared` or `borrow` references.

> Use `shared` cautiously to avoid unintended side effects.

## Borrowed Mutability (`borrow`)

**Only for return values and parameters!**

The referenced object originates from an `own` reference, and can temporarily be **mutated** from here (if `mut:` is used).

`borrow` references:
- Cannot be passed to `own` or mutable `shared` references
- **Can** be passed to immutable `shared` references
- **Can** be passed to mutable `borrow` references

> If `borrow` is not paired with `mut:`, it's effectively an immutable `shared` reference.

## Parameters, Return Values and Local Variables

Parameters, return values, and local variables default to `own` semantics.
This forces the caller or the assigner to provide a uniquely mutable reference.

## Providing uniquely mutable references

A uniquely mutable reference can be provided either by passing or by copying the object.

### Using the `pass` keyword
The `pass` keyword can be used to pass `own` references.
After passing a reference, the reference cannot be used anymore.  

> This behaves like the oppesite of declaring a variable: after passing, the reference is no longer usable and usages .

### Using `opt:move(v)`

The `opt:move(v)` function can be used to pass `own` references inside optionals. `v` is replaced with the name of any reassignable (`var`), optional (`opt`) field, local variable, or parameter.

After passing a reference, the optional is made empty. This is only possible if reassignment to `v` is allowed.

`opt:move(v)` allows fields to transfer ownership inside mutating methods.

## Constants (`const`)
Constants are static fields in a class, non-reassignable, and immutable.
Conventionally, their names are in UPPERCASE_SNAKE_CASING.

## Optional

In Neoncode, variables can never hold a `null` value — the language does not support nullability.  
Instead, optional (`opt`) is used to explicitly represent values that may or may not be present.

Nullable:
- Found in older languages
- Does not signal intention clearly, may as well be accidental
- `null` is considered low level, associated with segmentation faults

Optional:
- Found in newer and some older languages (with both nullable and optional)
- Signals **intention**: a value may or may not be present
- Not meant to cause segmentation faults if a value is empty

The compiler ensures optional is used safely, without unexpected exceptions or segmentation faults.

### Declaring optionals

A reference is optional when it is declared with `opt`, e.g. `opt int get_optional_number()`.

### `opt:` functions

Example Value Type: `int`

|  Return Reference Type (from example)  |        Syntax        |                                                   Description                                                   |
|----------------------------------------|----------------------|-----------------------------------------------------------------------------------------------------------------|
| `opt int`                              | `opt:of(v)`          | Create a present optional from the present value `v`.                                                           |
| `opt int`                              | `opt:empty`          | Create an empty optional.                                                                                       |
| `bool`                                 | `opt:is_present(o)`  | Return `true` if the optional `o` has a value. If used safely, it allows dereferencing `o` (compiler-enforced). |
| `bool`                                 | `opt:is_empty(o)`    | Return `true` if the optional `o` is empty.                                                                     |
| `int`                                  | `opt:or(o, v)`       | If the optional `o` is empty, return `v`, otherwise return the value of `o` as a non-optional.                  |
| `int` (may throw exception)            | `opt:or_throw(o)`    | If the optional `o` is empty, throw `unexpected_empty`, otherwise return the value of `o` as a non-optional.    |
| `opt int`                              | `opt:move(o)`        | Moves the optional value, making `o` empty. \*                                                                  |

\* Used to provide `own` references.

### Dereferencing optionals

An optional reference may be used as a regular reference when the compiler knows for sure there's a value present.
The example demonstrates this.

### Example
```
pkg main

public class vehicle mut
{
	var bool damaged = false;

	public void crash() mut
	{
		damaged = true;
	}

    public bool is_damaged()
    {
        ret damaged;
    }

}

public class success {}

public class parking_lot mut
{
	var opt shared mut:vehicle occupant = opt:empty;

	public opt success park(shared mut:vehicle driving_vehicle) mut
	{
		if(opt:is_present(occupant))
		{
			occupant.crash(); // Allowed after is_present
			driving_vehicle.crash();
			ret opt:empty;
		}

		occupant = opt:of(driving_vehicle);
		ret opt:of(success());
	}

    public bool is_occupied()
    {
        ret opt:is_present(occupant);
    }

}

```

[→ Next: Access Control & Imports](./access_control_and_imports.md)