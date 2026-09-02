# Neoncode: A Programming Language


## Introduction

Neoncode is a statically typed, compiled, object-oriented programming language designed around **safe defaults and explicit capabilities**.

The language aims to make important operations - such as **mutation, reassignment, mutation control, concurrency, and I/O** - explicit. Code only gains these capabilities when they are intentionally requested, making it easier to reason about what code is allowed to do while keeping ordinary code clean and ergonomic.

**Safe by default. Explicit when necessary.**


### At a Glance

| Property           | Neoncode                             |
| ------------------ | ------------------------------------ |
| Compilation        | Compiled through LLVM                |
| Memory management  | Compiler-inserted reference counting |
| Type system        | Static, with no type inference       |
| Programming model  | Object-oriented                      |
| Generics           | Supported                            |
| Nullability        | No nullable types                    |
| Error handling     | Result/error values                  |
| Concurrency        | Compiler-managed synchronization     |
| C interoperability | Major design goal                    |


### References and Values

From the programmer's perspective, Neoncode uses reference semantics throughout the language. Variables, parameters, and fields refer to objects or values, and assigning with `=` reassigns the corresponding reference.

Neoncode does not require programmers to manually manage memory. The compiler inserts the necessary reference-counting operations automatically.

Memory management and mutation control are separate concepts. The `own`, `borrow`, and `shared` mutation-control levels describe **who has control over an object's mutations**, not who is responsible for its memory or lifetime.


### Explicit Mutation

Mutation is treated as an explicit capability.

A reference must have **mutating permission** to perform mutations on the object it refers to. Mutation control can additionally be described using `own`, `borrow`, and `shared`, allowing Neoncode to distinguish exclusive mutation control from shared mutation.

Reassignment is a separate operation and is explicitly enabled with `var`.

This separation allows Neoncode to distinguish:

* changing **what a reference refers to**
* changing **the state of an object**
* controlling **who may mutate an object**


### Explicit Effects

Neoncode uses effect annotations to make important side effects visible in function and method declarations.

For example:

* `mut` indicates mutation of owned state or reassignment of fields.
* `share_mut` indicates mutation of shared state.
* `io` indicates that a function may perform input/output.

Functions without these effects can be treated as pure when their parameters also do not provide mutating permission.


### Concurrency

Neoncode is designed to make synchronized concurrent access explicit while allowing the compiler to handle the underlying synchronization mechanisms.

Mutation control and concurrency are related but distinct concepts. `own` and `borrow` references cannot be shared across threads, while shared mutation requires appropriate synchronization such as atomic operations or mutexes.

The programmer describes **which accesses must be synchronized**, while the compiler is responsible for inserting the required synchronization behaviour.


### Error Handling

Neoncode uses **result/error values** for ordinary error handling. Errors are therefore represented explicitly in function results rather than being implicitly propagated through a hidden control-flow mechanism.

Exception-based error handling may be introduced in the future, but is not currently part of the language.


### Optional Values

Neoncode does not have nullable types. The term **optional** is reserved for things that are genuinely optional rather than serving as a general representation of null.

This distinction allows the language to avoid treating the absence of a value as an implicit property of every reference.


### Interoperability

**C interoperability is a major goal of Neoncode.** The language is intended to be capable of interacting with existing C libraries and systems software, allowing Neoncode programs to make use of the extensive ecosystem of C APIs.


### Current Status

Neoncode is currently in **very early development**.

The language specification is largely complete, but remains subject to change as implementation work reveals new requirements and edge cases. The compiler's parser and AST are currently under active development.

The specification should therefore be considered a description of the **current intended language**, rather than a guarantee that every described feature is already implemented or permanently finalized.


## Documentation Overview


### Contents


#### 1. [Intro](#neoncode-a-programming-language)
1. [Introduction](#introduction)
2. [Overview](#documentation-overview)
3. [Hello World Example](#hello-world-example)

#### 2. [Native Types](./native_types.md)
1. [Default numeric types](./native_types.md#default-numeric-types)
2. [Different size numeric types](./native_types.md#default-numeric-types)
3. [Boolean](./native_types.md#boolean)
4. [Text types](./native_types.md#text-types)
5. [Array types](./native_types.md#array-types)
6. [Generic parameter types](./native_types.md#generic-parameter-types)

#### 3. [Mutating Access](./mutating_access.md)
1. [Reassignable References](./mutating_access.md#reassignable-references)
2. [Mutable Types and Mutating Methods](./mutating_access.md#mutable-types-and-mutating-methods)
3. [Reference Mutating Permission (`mut:`)](./mutating_access.md#reference-mutating-permission-mut)
4. [Difference between `var` and `mut:`](./mutating_access.md#difference-between-var-and-mut)
5. [`mut` in different places](./mutating_access.md#mut-in-different-places)

#### 4. [Mutation Ownership](./mutation_ownership.md)
1. [Mutation Control Levels](./mutation_ownership.md#mutation-control-levels)
2. [Default Mutation Control Levels](./mutation_ownership.md#default-mutation-control-levels)
3. [Giving Mutation Ownership](./mutation_ownership.md#giving-mutation-ownership-give)
4. [Reference Providing Matrix](./mutation_ownership.md#reference-providing-matrix)

#### 5. [Effect Annotations](./effect_annotations.md)
1. [Syntax Example](./effect_annotations.md#syntax-example)
2. [`mut`](./effect_annotations.md#mut)
3. [`share_mut`](./effect_annotations.md#share_mut)
4. [`io`](./effect_annotations.md#io)
5. [Pure functions](./effect_annotations.md#pure-functions)

#### 6. [Access Control & Imports](./access_control_and_imports.md)
1. [Visibility Levels](./access_control_and_imports.md#visibility-levels)
2. [Defaults](./access_control_and_imports.md#defaults)
3. [Reasoning](./access_control_and_imports.md#reasoning)
4. [Exclusive](./access_control_and_imports.md#exclusive)
5. [Imports](./access_control_and_imports.md#imports)

#### 7. [Arrays](./arrays.md)
1. [Types of Arrays](./arrays.md#types-of-arrays)
2. [Fixed-size & Runtime-sized](./arrays.md#fixed-size--runtime-sized)
3. [Arrays of immutable type](./arrays.md#arrays-of-immutable-type)
4. [Arrays of immutable `own` references](./arrays.md#arrays-of-immutable-own-references)
5. [Arrays of mutable `own` references](./arrays.md#arrays-of-mutable-own-references)
6. [Arrays of immutable `shared` references](./arrays.md#arrays-of-immutable-shared-references)
7. [Arrays of mutable `shared` references](./arrays.md#arrays-of-mutable-shared-references)
8. [Array Example](./arrays.md#array-example)

#### 8. [Developer Guidelines and Naming Conventions](./developer_guidelines_and_naming_conventions.md)
1. [Developer Guidelines](./developer_guidelines_and_naming_conventions.md#developer-guidelines)
2. [Naming Conventions](./developer_guidelines_and_naming_conventions.md#naming-conventions)

#### 9. [Object Oriented](./object_oriented.md)
1. [Implementing Interfaces](./object_oriented.md#implementing-interfaces)
2. [Extending Classes](./object_oriented.md#extending-classes)
3. [Using Abstract Classes](./object_oriented.md#using-abstract-classes)
4. [Extending Interfaces](./object_oriented.md#extending-interfaces)

#### 10. [Examples #1](./examples_1.md)
1. [Mutable Person Class](./examples_1.md#1-mutable-person-class)
2. [Bank Account](./examples_1.md#2-bank-account)
3. [Licence Plate](./examples_1.md#3-licence-plate)

#### 11. [Name Inference](./name_inference.md)
1. [Using Name Inference](./name_inference.md#using-name-inference)
2. [How names are inferred](./name_inference.md#how-names-are-inferred)
3. [Why no type inference](./name_inference.md#why-no-type-inference)
4. [Example](./name_inference.md#example)

#### 12. [Operator Modules](./operator_modules.md)
1. [Operators](./operator_modules.md#operators)
2. [Operator Functions](./operator_modules.md#operator-functions)
3. [Usage Tips](./operator_modules.md#usage-tips)
4. [Examples](./operator_modules.md#examples)

#### 13. [Compile-Time Functions](./compile_time_functions.md)

#### 14. [Callables](./callables.md)

#### 15. [Concurrency](./concurrency.md)

#### 16. [Equality](./equality.md)

#### 17. [Indexing Syntax](./indexing_syntax.md)

#### 18. [Generics](./generics.md)

#### 19. [Optional](./optional.md)

#### 20. [Constants](./constants.md)


## Hello World Example

```

pkg examples::hello_world;

import std::console;

void main(array<string> args) io
{
	console::print_line("Hello, world!");
}

```


[→ Next: Native Types](./native_types.md)