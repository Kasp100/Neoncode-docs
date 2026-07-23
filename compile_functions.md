[← Go back](./intro.md#13-compile-functions)

# Compile Functions

Compile functions are special functions that are run at **compile time** (instead of runtime) for compile-time **code generation** or **contracts**.  
The `auto:` directive tells the compiler to run a compile function.

## Example Usage
```
pkg main;

// optional (imports are optional for built-in compile functions like "constructor")
import std::codegen::constructor;
import std::codegen::getters;

public class person
{
	string name;
	date birthdate;

	public auto:constructor;
	public auto:getters(name, birthdate);
}
```
Here, there are two compile functions being called: "constructor" and "getters".

It's a powerful system for writing less code.

### `auto:constructor`
This compile function creates a **constructor** based on all unitialised fields (which in the above example are "name" and "birthdate").
The resulting constructor will look like this:
```
public constructor(string name, date birthdate)
{
	this.name = name;
	this.birthdate = birthdate;
}
```

### `auto:getters(...)`
This compile function creates **getters** for the fields passed in its parameters.
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
pkg main;

// optional (imports are optional for built-in compile functions like enum)
import std::codegen::enum;

public auto:enum skill_level
{
	BEGINNER,
	INTERMEDIATE,
	ADVANCED
}

```

## Writing Compile Functions

The language allows hooking in on the compilation process with the keyword `compile_function`.

Here is an example for a compile function called "getters".
```
public compile_function getters(type_member_emitter e, cf_param... field_names)
throws invalid_argument
{
	// generation code here
}
```

In the above example `e` can be one of the following types:
- `statement_emitter` for code inside blocks
- `type_member_emitter` for code inside type definitions (constants, fields, constructors, and methods)
- `pure_function_set_member_emitter` for code inside type definitions (constants, fields, constructors, and methods)
- `package_member_emitter` for code at the package-level (classes, abstract classes, and interfaces)

`cf_param` stands for "compile function parameter" and holds **compiler tokens**.
`...` allows a variable amount of parameters to be passed. It translates to `array<cf_param>`.

These allow you to append to the AST (abstract syntax tree).

Compile functions are package members, so they are private by default. They need to be imported if used from another package.

[→ Next: Pure Functions](./pure_functions.md)