[← Go back](./intro.md#6-access-control--imports)

# Access Control & Imports

Access control is essential for writing safe code. This can be done by setting visibility levels on members of packages and types.

Here is a full list of all access control / visibility keywords and their meaning.

## Visibility Levels

|       Keyword / Name       |       Members of Types & Pure Function Sets       |                     Package members *                     |
|----------------------------|---------------------------------------------------|-----------------------------------------------------------|
| `private`                  | Accessible within the same type                   | Accessible within the same package only, not subpackages  |
| `public`                   | Accessible from everywhere                        | Accessible from any package                               |
| `protected`                | Accessible to subtypes                            | *Invalid for package members*                             |
| [`exclusive`](#exclusive)  | Accessible to a specific list of package members  | Accessible to a specific list of package members          |

\* A **package member** can be one of the following:
- types (classes, abstract classes, and interfaces)
- pure function sets
- grammar sets
- compile functions

## Defaults

There are a few defaults to take into account.
- **Package members** are private by default. They can be set public using the `public` keyword.
- **Fields** are irrevertably **private**. This is by design — getters and setters are used to allow access from the outside. 
- **Fields** inside **serialisable** types are only directly readable from the outside within `serialising` blocks.
- **Methods and constructors** are in classes and abstract classes private by default.
- **Abstract methods** (incl. methods inside interfaces) are **public by default**, and cannot be set `private`.
    > Having private methods makes them unable to be used.
- **Constants, static methods**

### Reasoning

> Fields are always private by default, and this visibility cannot be changed.

This design ensures that a type maintains full control over how its internal state is accessed and modified. Public fields allow unrestricted external access, making it impossible to enforce invariants.  
By keeping fields private, types can expose controlled access through methods. This is valuable in inheritance hierarchies — where subclasses may override getters or setters to implement custom behaviour.  

## Exclusive

The `exclusive` keyword restricts access to the specified package members or patterns. Patterns use **package member pattern matching** to determine which members may access the declaration.
Patterns can match by exact name, qualified name, wildcards, inheritance (extends), or combinations thereof.

|                         Package Member Pattern                         |                                                          Who can use                                                          |
|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| *package member name* (without `::`)                                   | A package member named *package member name* inside the current package (i.e., the package from the file's `pkg` declaration) |
| *package path* + `::` + *package member name*                          | A package member named *package member name* inside *package path*                                                            |
| *package path* + `::` + `*`                                            | Any package member in *package path*, without its subpackages                                                                 |
| *package path* + `::` + `...`                                          | Any package member in *package path* **and** its subpackages                                                                  |
| `extends` + *package member pattern*                                   | Any type that inherits from the types found by *package member pattern*                                                       |
| *package member pattern A* + `extends` + *package member pattern B*    | Any type found by *package member pattern A* that inherits from the types found by *package member pattern B*                 |

Note: a *package path* may consist of several `::` as well.

## Imports

Imports are used to bring **public** package members from other packages (and subpackages) into a **file** to be used.

`import std::collections::trace` 

Package members of the same package do not need importing.

[→ Next: Arrays](./arrays.md)