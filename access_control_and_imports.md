[← Go back](./intro.md#6-access-control--imports)

# Access Control & Imports

Access control is essential for writing safe code. This can be done by setting visibility levels on members of packages and types.

Here is a full list of all access control / visibility keywords and their meaning.


## Visibility Levels

|       Keyword / Name       |                 Members of Types                 |                     Package members *                     |
| -------------------------- | ------------------------------------------------ | --------------------------------------------------------- |
| private (no keyword)       | Accessible within the same type                  | Accessible within the same package only, not subpackages  |
| `public`                   | Accessible from everywhere                       | Accessible from any package                               |
| `implementers`             | Accessible to implementers                       | *Invalid for package members*                             |
| `extensions`               | Accessible to extensions                         | *Invalid for package members*                             |
| [`exclusive`](#exclusive)  | Accessible to a specific list of package members | Accessible to a specific list of package members          |

\* **Package members** can the following:
- package functions
- package constants
- types
- operator modules
- extension modules


## Defaults

The defaults define the visibility level if no visibility keyword is used.


### Unmodifiable default visibility levels

- Methods inside [fully abstract types](./concrete_types_and_abstract_types.md#fully-abstract-types) are irrevertably public by default.
- Fields are irreverably private by default. Outside access is provided in a controlled way through getters and setters.


### Modifiable default visibility levels

- Package members are private by default
- Methods and constructors are private by default, except for those inside fully abstract types.


### Reasoning

> Fields are always private by default, and this visibility cannot be changed.

This design ensures that a type maintains full control over how its internal state is accessed and modified. Public fields allow unrestricted external access, making it impossible to enforce invariants.  
By keeping fields private, types can expose controlled access through methods.


## Exclusive

The `exclusive` keyword restricts access to the specified package members or patterns. Patterns use **package member pattern matching** to determine which members may access the declaration.
Patterns can match by exact name, qualified name, wildcards, inheritance (extends), or combinations thereof.

|                   Package Member Pattern                   |                                               Who can use                                               |
| ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| *package member path*                                      | The specified package member                                                                            |
| `shallow` + `pkg` + *package path*                         | Any package member in *package path*, without its subpackages                                           |
| `deep` + `pkg` + *package path*                            | Any package member in *package path* **and** its subpackages                                            |
| `impl` + *package member path*                             | Any type that inherits from the type found by *package member path*                                     |
| *package member pattern* + `impl` + *package member path*  | Any type found by *package member pattern* that inherits from the type found by *package member path*   |

Note: a *package path* may consist of several `::` as well.


## Imports

Imports are used to bring packages and package members into the current file so they can be referenced by just their name instead of their full path.

Example:

```

pkg examples::imports;

import my_domain::my_project::my_type; // "my_type" now refers to "my_domain::my_project::my_type" in this file.

// Use "my_type"...

```

If `::` is used after the `import` keyword, the current package is automatically inserted.

Example:

```

pkg my_domain;

import ::my_project::my_type; // "my_type" now refers to "my_domain::my_project::my_type" in this file.

```


[→ Next: Arrays](./arrays.md)