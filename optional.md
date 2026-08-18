[← Go back](./intro.md#19-optional)

# Optional

In Neoncode, variables can never hold a `null` value - the language does not support nullability.  
Instead, optional (`opt`) is used to explicitly represent values that may or may not be present.

Nullable:
- Found in older languages
- Does not signal intention clearly, may as well be accidental
- `null` is considered low-level, associated with segmentation faults

Optional:
- Found in newer and some older languages (with both nullable and optional)
- Signals **intention**: a value may or may not be present
- Not meant to cause segmentation faults if a value is empty

The compiler ensures optional is used safely, without unexpected exceptions or segmentation faults.


## Declaring optionals

A reference is optional when it is declared with `opt`, e.g. `opt int get_optional_number()`.


## Optional Syntax

- The keyword `empty` represents an empty optional.
- `o present` returns `true` only if the optional `o` has a value. This dereferences the optional.
- `o absent` returns `true` only if the optional `o` is empty.
- `o or v` returns `o` if it is present and `v` otherwise.
- `opt int o = 7` assigns the present value `7` to the optional.


## Dereferencing optionals

An optional reference may be used as a regular reference when the compiler knows for sure there's a value present.
The example demonstrates this.


## Example
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
		if(occupant present)
		{
			occupant.crash();
			driving_vehicle.crash();
			ret empty;
		}

		occupant = driving_vehicle;
		ret success();
	}

    public bool is_occupied()
    {
        ret occupant present;
    }

}

```

[→ Next: Constants](./constants.md)