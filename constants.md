[← Go back](./intro.md#constants)

# Constants

In Neoncode, **constants** are variables that are initialised at their declaration, not reassignable, hold an [Owned Mutations reference](./mutation_ownership.md) without [mutating permission](./mutating_access.md), and are initialised [without IO](./effect_annotations.md). Any variable that meets this requirement becomes a compile-time constant and conventionally has a name in [uppercase snake casing](./naming_conventions.md).

Constants can exist at package level and inside type definitions. They do not use a special keyword - the absence of reassignment and mutation-related keywords and the assignment at their declaration already guarantees constant properties.


## Package constants

Constants declared at package-level are known as **package constants**. They are package members and can have a [custom visibility](./access_control_and_imports.md).

Example:

```
pkg examples::package_constants;

public real PI = 3.141592;
```


## Type member constants

Constants in types are known as **type member constants**.

There are two kinds:

- **Instance type member constants**: A field that happens to have constant properties.
- **Static type member constants**: A special type of field that is also accessible in static contexts.

Unlike ordinary fields, **static type member constants** may set a [custom visibility](./access_control_and_imports.md).


Examples:

```
pkg examples::type_member_constants;

public type calendar_week
{
	// instance constant - visible to instances of this type only, cannot be set public:
	nat WORKING_DAYS = 5;

	// static constant - visible from local static and non-static contexts:
	static nat WEEKEND_DAYS = 2; 

	// static constant with custom visibility - visible from all static and non-static contexts, according to the custom visibility:
	public static nat DAYS_IN_WEEK = 7;
}
```

## Local constants

Local constants are constants declared within the body of a function. They are local variables that happen to have constant properties.

Example:

```
public bool is_weekend(nat day)
{
	nat WEEKEND_START = 4;
	ret day > WEEKEND_START;
}
```


[→ Next: Naming Conventions](./naming_conventions.md)