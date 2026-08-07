[← Go back](./intro.md#3-immutability-and-const-by-default)

# Immutability and Const by Default

Languages like C++, Java, and C# use extra keywords to prevent unintentional mutations to data.
In Neoncode, this is flipped upside down: keywords are used to **allow** mutations to data.

This allows quicker development and more readable code.

## Immutability by Default
References to mutable types require `mut:` before the type name to allow calling **mutating methods** (methods marked with `mut`) via that reference.
An object may be mutated from somewhere else if it is `shared`.  

> Developers need to clearly state their intentions by using `mut:` or not.

- `mut:` cannot be used on immutable types, as they cannot have mutating methods.
- `copy` produces a mutable copy.

## Const by Default
Variables and fields cannot be reassigned by default.

### Keyword `var`
To enable **reassignment** after the initial assignment of a variable or field, use `var`.

> Neoncode doesn't use a keyword like "final" or "const" to ensure a reference or value isn't changed. Instead, `var` is used to explicitly allow reassignment.
> `const`, however is a keyword used for **constants**, static fields in a class and not changeable or mutable.

## Mutable declarations
`mut` (without colon) has two usages:

### 1. To allow a method to mutate the data of the object.

`mut` is placed after the closing curly bracket, making the method **mutating**.
This allows mutating operations to occur.

When overriding a method from a supertype, a method declared non-mutating cannot be declared mutating.

> When creating abstract methods (like in interfaces or abstract classes), this must be taken into account and be thought about wisely.

### 2. To allow a type to have mutating methods.

`mut` is placed after the type name, making it a mutable type.

> This means that the keywords `shared`, `own`, and `mut:` can be used with fields, variables, parameters, and return types. 

[→ Next: Types of References](./types_of_references.md)