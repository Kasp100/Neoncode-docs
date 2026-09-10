[← Go back](./intro.md#20-constants)

# Constants

In Neoncode, constants hold immutable values, are initialised once, and cannot be reassigned.

Constants can exist at package level and inside type definitions. They do not use a special keyword - the absence of reassignment and mutation-related keywords and the assignment at their declaration already guarantees constant properties. Constants in types can be set **public**, unlike fields.

Conventionally, their names are in UPPERCASE_SNAKE_CASING.


## Examples

**Package constant**:

```
pkg examples::package_constant;

public real PI = 3.141592;

```


**Constant field**

```
pkg examples::constant_field;

public type calendar_week
{
    public nat DAYS_IN_WEEK = 7;
}

```
