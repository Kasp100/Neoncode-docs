[← Go back](./intro.md#13-compile-functions)

# Compile Functions

Compile functions are special functions that are run at **compile time** (instead of runtime) for compile-time **code generation** or **contracts**.  
The `auto:` directive tells the compiler to run a compile function.

## Example Usage
```
pkg main;

import std::codegen::public_constructor;
import std::codegen::public_getters;

public class person
{
    string name;
    date birthdate;

    auto:public_constructor();
    auto:public_getters(name, birthdate);
}
```
Here, there are two compile functions being called: "constructor" and "getters".

It's a powerful system for writing less code.

### `auto:public_constructor`
This compile function creates a **public constructor** based on all unitialised fields (which in the above example are "name" and "birthdate").
The resulting constructor will look like this:
```
public constructor(string name, date birthdate)
{
    this.name = name;
    this.birthdate = birthdate;
}
```

### `auto:public_getters`
This compile function creates **public getters** for the fields passed in its parameters.
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

In case of mutable references (i.e., fields with `mut:`), an **immutable reference is returned**.

## Writing Compile Functions

The language allows hooking in on the compilation process with the keyword `compile_function`.

Depending on the signature of the compile function, it can be used either inside code blocks, type definitions, pure function set definitions, or at package-level.

Here is an example for a compile function called "constructor".
```
public compile_function constructor(type_member_emitter t, array<compile_function_parameter> head, array<token> body)
throws invalid_argument
{
	// insert code here to generate
}
```
1. Compile functions can either be public or private inside of their package.
2. `type` is used for compile functions targetting type definitions, giving the ability to add type members.
3. `array<compile_function_parameter>` is in every compile function, it contains an `array<token>` that contains compile tokens.

In the above example, `t` can be one of the following types:
- `statement_emitter` for code inside blocks
- `type_member_emitter` for code inside type definitions (constants, fields, constructors, and methods)
- `pure_function_set_member_emitter` for code inside type definitions (constants, fields, constructors, and methods)
- `package_member_emitter` for code at the package-level (classes, abstract classes, and interfaces)

These allow you to look at, and add to the code inside these AST (abstract syntax tree) nodes.

Compile functions are package members — They also need to be imported with `import` if they originate from another package.

[→ Next: Pure Functions](./pure_functions.md)