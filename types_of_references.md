[← Go back](./intro.md#5-types-of-references)

# Types of References

Mutations to objects of mutable types are controlled through the keywords `own`, `borrow`, `shared`, and `give`.

As described in [Immutability and Const by Default](./immutability_and_const_by_default.md), a `mut:` reference is needed to mutate an object or to pass it to other `mut:` references.

> **Important language notes**:
> - In Neoncode, "ownership", "borrowing", "sharing", and "view" are terminology for **mutating permission** rather than memory ownership like in traditional languages.
> - 


## Defaults

- **Fields** and **local variables** are `own` by default, without keyword, but can be set `shared`
- **Parameters** and **return values** are `borrow` by default, but can be set explicitly `own` or `shared`


## Controlled Mutations (`own`)

You own the object, no one else can mutate it unless you let them [**borrow**](#borrowed-mutability-borrow) it **with a `mut:` reference**.

Properties:
- Mutating the object is considered mutating the owner, so the type it's inside must be mutable.
- Can be freely passed to immutable `shared` references.
- `own` is often paired with `mut:` (to mutate a mutable type)


## Shared Mutability (`shared`)

The object is owned by no one and can possibly be mutated by others.

- Mutating the referenced object is **not** considered mutating the container.
- Can be freely passed to immutable **and mutable** `shared` or `borrow` references.

This is the most permissive type of reference. It carries the most risk. Use `shared` cautiously to avoid unintended side effects.


## Borrowed Mutability (`borrow`)

This concept is only applicable for **return values and parameters.**

The object is owned by someone else, but it won't be mutated by its owner or by other borrowers while you have the reference.  
**With `mut:`**, you get exclusive mutating permission.

`borrow` references:
- **Cannot** be passed to `own` or mutable `shared` references
- **Can** be passed to immutable `shared` references
- **Can** be passed to mutable `borrow` references


## Parameters, Return Values and Local Variables

Parameters, return values, and local variables default to `own` semantics.  
The caller or the assigner must provide an `own` reference.

## Giving up ownership

An `own` reference can be provided either by giving or by copying the object.

A `give` expression transfers ownership. After giving, the reference becomes **`shared` without `mut:`**.  
If the original reference had `mut:`, the mutating permission is also provided.


## Constants (`const`)
Constants are static fields in a class, non-reassignable, and immutable.
Conventionally, their names are in UPPERCASE_SNAKE_CASING.


## Optional

In Neoncode, variables can never hold a `null` value - the language does not support nullability.  
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

Example value type: `int`

`o` in the table represents an optional with type `int`.

|  Return Reference Type (from example)  |        Syntax        |                                           Description                                           |
| -------------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------- |
| `opt int`                              | `opt:empty`          | Create an empty optional.                                                                       |
| `bool`                                 | `opt:is_present(o)`  | Return `true` if `o` has a value. Used safely, allows dereferencing `o` (compiler-enforced).    |
| `bool`                                 | `opt:is_empty(o)`    | Return `true` if `o` is empty.                                                                  |
| `int`                                  | `opt:or(o, v)`       | If `o` is empty, return `v`, otherwise return the value of `o` as a non-optional.               |
| `int` (may throw exception)            | `opt:or_throw(o)`    | If `o` is empty, throw `unexpected_empty`, otherwise return the value of `o` as non-optional.   |


#### Notes

- `opt int x = 7` assigns a present value to the optional reference.

### Dereferencing optionals

An optional reference may be used as a regular reference when the compiler knows for sure there's a value present.
The example demonstrates this.


### Example
```
pkg main;

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

		occupant = driving_vehicle;
		ret success();
	}

    public bool is_occupied()
    {
        ret opt:is_present(occupant);
    }

}

```


[→ Next: Access Control & Imports](./access_control_and_imports.md)
