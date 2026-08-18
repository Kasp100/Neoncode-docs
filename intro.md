# Neoncode: A Programming Language

A programming language designed for safety and quick development.

Neoncode is object-oriented, functional and general-purpose. It features types, interfaces, classes, and more.

## Documentation Overview

### Contents

#### 1. [Intro](#neoncode-a-programming-language)
1. [Overview](#documentation-overview)
2. [About Neoncode](#about-neoncode)
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
3. [Reference Mutation Access](./mutating_access.md#reference-mutation-access)
4. [Difference between `var` and `mut:`](./mutating_access.md#difference-between-var-and-mut)
5. [`mut` in different places](./mutating_access.md#mut-in-different-places)

#### 4. [Mutation Ownership](./mutation_ownership.md)
1. [Defaults](./mutation_ownership.md#defaults)
3. [Controlled Mutations](./mutation_ownership.md#controlled-mutations-own)
4. [Shared Mutability](./mutation_ownership.md#shared-mutability-shared)
5. [Borrowed Mutability](./mutation_ownership.md#borrowed-mutability-borrow)
6. [Giving up ownership](./mutation_ownership.md#giving-up-ownership)
7. [Constants](./mutation_ownership.md#constants-const)
8. [Optional](./mutation_ownership.md#optional)

#### 5. [Access Control & Imports](./access_control_and_imports.md)
1. [Visibility Levels](./access_control_and_imports.md#visibility-levels)
2. [Defaults](./access_control_and_imports.md#defaults)
3. [Reasoning](./access_control_and_imports.md#reasoning)
4. [Exclusive](./access_control_and_imports.md#exclusive)
5. [Imports](./access_control_and_imports.md#imports)

#### 6. [Arrays](./arrays.md)
1. [Types of Arrays](./arrays.md#types-of-arrays)
2. [Fixed-size & Runtime-sized](./arrays.md#fixed-size--runtime-sized)
3. [Arrays of immutable type](./arrays.md#arrays-of-immutable-type)
4. [Arrays of immutable `own` references](./arrays.md#arrays-of-immutable-own-references)
5. [Arrays of mutable `own` references](./arrays.md#arrays-of-mutable-own-references)
6. [Arrays of immutable `shared` references](./arrays.md#arrays-of-immutable-shared-references)
7. [Arrays of mutable `shared` references](./arrays.md#arrays-of-mutable-shared-references)
8. [Array Example](./arrays.md#array-example)

#### 7. [Developer Guidelines and Naming Conventions](./developer_guidelines_and_naming_conventions.md)
1. [Developer Guidelines](./developer_guidelines_and_naming_conventions.md#developer-guidelines)
2. [Naming Conventions](./developer_guidelines_and_naming_conventions.md#naming-conventions)

#### 8. [Object Oriented](./object_oriented.md)
1. [Implementing Interfaces](./object_oriented.md#implementing-interfaces)
2. [Extending Classes](./object_oriented.md#extending-classes)
3. [Using Abstract Classes](./object_oriented.md#using-abstract-classes)
4. [Extending Interfaces](./object_oriented.md#extending-interfaces)

#### 9. [Examples #1](./examples_1.md)
1. [Mutable Person Class](./examples_1.md#1-mutable-person-class)
2. [Bank Account](./examples_1.md#2-bank-account)
3. [Licence Plate](./examples_1.md#3-licence-plate)

#### 10. [Name Inference](./name_inference.md)
1. [Using Name Inference](./name_inference.md#using-name-inference)
2. [How names are inferred](./name_inference.md#how-names-are-inferred)
3. [Why no type inference](./name_inference.md#why-no-type-inference)
4. [Example](./name_inference.md#example)

#### 11. [Operator Modules](./operator_modules.md)
1. [Operators](./operator_modules.md#operators)
2. [Operator Functions](./operator_modules.md#operator-functions)
3. [Usage Tips](./operator_modules.md#usage-tips)
4. [Examples](./operator_modules.md#examples)

#### 12. [Compile Functions](./compile_functions.md)
1. [Example Usage](./compile_functions.md#example-usage)
2. [Writing Compile Functions](./compile_functions.md#writing-compile-functions)

#### 13. [Pure Functions](./pure_functions.md)
1. [Inside types](./pure_functions.md#inside-types)
2. [Pure Function Sets](./pure_functions.md#pure-function-sets)

#### 14. [Functional & Lambda](./functional_and_lambda.md)

#### 15. [Concurrency](./concurrency.md)

#### 16. [Equality](./equality.md)

#### 17. [Indexing Syntax](./indexing_syntax.md)

#### 18. [Generics](./generics.md)

#### 19. [Optional](./optional.md)

#### 20. [Constants](./constants.md)


## About Neoncode

Neoncode is a general-purpose, compiled programming language.

The goals of Neoncode are optimal safety, quick development, and beginner friendliness.

The language supports paradigms like object-oriented and functional programming.


## Hello World Example

```
pkg main;

import std::console;

entrypoint main(array<string> args)
{
	console::print_line("Hello, world!");
}
```


[→ Next: ](./native_types.md)