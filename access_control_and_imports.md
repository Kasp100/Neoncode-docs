[← Go back](./intro.md#6-access-control--imports)

# Access Control & Imports

Access control is essential for writing safe code. This can be done by setting visibility levels on members of packages and types.

Here is a full list of all access control / visibility keywords and their meaning.


## Visibility Levels

|       Keyword / Name       |       Members of Types & Pure Function Sets       |                     Package members *                     |
| -------------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| `private`                  | Accessible within the same type                   | Accessible within the same package only, not subpackages  |
| `public`                   | Accessible from everywhere                        | Accessible from any package                               |
| `protected`                | Accessible to subtypes                            | *Invalid for package members*                             |
| [`exclusive`](#exclusive)  | Accessible to a specific list of package members  | Accessible to a specific list of package members          |

\* A **package member** can be one of the following:
- types (classes, abstract classes, and interfaces)
- functions
- constants
- operator modules
- compile functions


## Defaults

There are a few defaults to take into account.
- **Package members** are private by default. They can be set public using the `public` keyword.
- **Fields** are irrevertably **private**. This is by design - getters and setters are used to allow access from the outside. 
- **Fields** inside **serialisable** types are only directly readable from the outside within `serialising` blocks.
- **Methods and constructors** are in classes and abstract classes private by default.
- **Abstract methods** (incl. methods inside interfaces) are **public by default**.
- **Constants, static methods**


### Reasoning

> Fields are always private by default, and this visibility cannot be changed.

This design ensures that a type maintains full control over how its internal state is accessed and modified. Public fields allow unrestricted external access, making it impossible to enforce invariants.  
By keeping fields private, types can expose controlled access through methods. This is valuable in inheritance hierarchies - where subclasses may override getters or setters to implement custom behaviour.  


## Exclusive

The `exclusive` keyword restricts access to the specified package members or patterns. Patterns use **package member pattern matching** to determine which members may access the declaration.
Patterns can match by exact name, qualified name, wildcards, inheritance (extends), or combinations thereof.

|                     Package Member Pattern                     |                                               Who can use                                               |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| *package member path*                                          | The specified package member                                                                            |
| `shallow` + `pkg` + *package path*                             | Any package member in *package path*, without its subpackages                                           |
| `deep` + `pkg` + *package path*                                | Any package member in *package path* **and** its subpackages                                            |
| `extends` + *package member path*                              | Any type that inherits from the type found by *package member path*                                     |
| *package member pattern* + `extends` + *package member path*   | Any type found by *package member pattern* that inherits from the type found by *package member path*   |

Note: a *package path* may consist of several `::` as well.


## Imports

Imports are used to bring **public** package members from other packages (and subpackages) into a **file** to be used.

Example: `import my_domain::my_project::my_class` 

Package members from the current package (see the `pkg` package declaration in the file), including packages which contain the current package package do not need importing.


[→ Next: Arrays](./arrays.md)