[← Go back](./intro.md#13-compile-time-functions)

# Compile-Time Functions

Compile-time functions are functions that are run at **compile time** instead of runtime. They are limited in what they can do, but powerful for code generation.

The `auto:` directive tells the compiler to run a compile-time function.


## Example Usage

```

public class person
{
	string name;
	date birthdate;

	public auto:constructor;
	public auto:getters(name, birthdate);
}

```

Here, there are two compile-time functions being called: "constructor" and "getters".


### `auto:constructor`

This compile-time function creates a **constructor** based on all unitialised fields (which in the above example are "name" and "birthdate").
The resulting constructor will look like this:

```

public constructor(own string name, date birthdate)
{
	this.name = give name;
	this.birthdate = birthdate;
}

```


### `auto:getters(...)`

This compile-time function creates **getters** for the fields passed in its parameters.
The resulting methods will look like this:

```

public string get_name()
{
	ret name;
}

public date get_birthdate()
{
	ret birthdate;
}

```

In case of fields declared with `mut:`, a reference **without `mut:`** is returned.


## Enum Example

```

pkg examples::enum;

// optional (imports are optional for built-in compile-time functions like enum)
import std::codegen::enum;

public auto:enum skill_level
{
	BEGINNER,
	INTERMEDIATE,
	ADVANCED
}

```


## Compile-Time Function Syntax

The language allows hooking in on the compilation process with the keyword `compile_time` for package functions.

```

pkg examples::codegen;

public compile_time sequence<neon_method> getters(sequence<neon_token> tokens)
{
	ret sequence::of<neon_method>();
}

```


[→ Next: Callables](./callables.md)