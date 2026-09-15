[← Go back](./intro.md#constants)

# Constants

In Neoncode, **constants** are variables that are initialised at their declaration, not reassignable, hold an [Owned Mutations reference](./mutation_ownership.md) without [mutating permission](./mutating_access.md), and are initialised [without IO](./effect_annotations.md). Any variable that meets this requirement becomes a compile-time constant and conventionally has a name in [uppercase snake casing](./naming_conventions.md).

Constants can exist at package level and inside type definitions. They do not use a special keyword - the absence of reassignment and mutation-related keywords and the assignment at their declaration already guarantees constant properties.


## Static type member contants

Constants in types can be made **static**, unlike normal fields.

Unlike ordinary fields, static constants can have a custom [visibility](./access_control_and_imports.md).


## Examples

**Package constant**:

```
pkg examples::package_constant;

public real PI = 3.141592;

```


**Constants in types**

```
pkg examples::constant_field;

public type calendar_week
{
    nat WORKING_DAYS = 5; // private non-static constant - accessible to instances of this type only, cannot be set public

    static nat WEEKEND_DAYS = 2; // private static constant - accessible from local static and non-static contexts

    public static nat DAYS_IN_WEEK = 7; // public static constant - accessible from all static and non-static contexts
}

```

