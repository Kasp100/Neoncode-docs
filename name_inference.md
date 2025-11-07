[← Go back](./intro.md#11-name-inference)

# Name Inference

It often happens that variables (or fields,...) have the exact same name as their type. In some code bases, this happens particularly often.
Neoncode removes this redundancy by allowing *name inference*.

## Using Name Inference

In Neoncode, name inference happens when a variable, field, or (generic) parameter is written without a name: just type.

Naming collisions are not automatically resolved. It is up to the programmer to resolve these (e.g. naming one manually instead of inferring it).

## How names are inferred

Consider the declaration:

`shared mut:array<string,8>`

1. `shared` and `mut:` are left out, as they define the type of reference and API used.

(`array<string,8>`)

2. Generic parameters are left out, keeping name inference simple and names less likely to change

**Final name**: `array`

## Why no type inference

Neoncode does not support type inference. There are two big reasons for that.
1. Type inference sometimes causes bugs.
	An API may change its return type (perhaps to one that is similar in API to the old one).
	This means a variable may change type without causing errors, which in turn causes bugs.
2. The main reason: clarity
	It is much clearer knowing what is being worked with than having to hover or do guesswork

## Example
```
pkg main;

import std::codegen::public_constructor;
import std::codegen::public_getters;
import std::codegen::public_setters;

public class person mut copyable
{
	string name;
	var nat age;
	auto:public_constructor();
	auto:public_getters(name, age);
	auto:public_setters(age);
}

public class example mut
{
	mut:person;

	public constructor(person p)
	{
		person = copy p;
	}

	public increase_age(nat) mut
	{
		nat prev_age = person.get_age();
		person.set_age(prev_age + nat);
	}

}

```

[→ Next: Serialisation and Deserialisation](./custom_expression_grammar.md)