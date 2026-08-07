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
1. [Table](./native-types.md#table)
2. [Notation](./native-types.md#notation)

#### 3. [Immutability and Const by Default](./immutability_and_const_by_default.md)
1. [Immutability by Default](./immutability_and_const_by_default.md#immutability-by-default)
2. [Const by Default](./immutability_and_const_by_default.md#const-by-default)
3. [Mutable Declarations](./immutability_and_const_by_default.md#mutable-declarations)

#### 4. [Types of References](./types_of_references.md)
1. [Defaults](./types_of_references.md#defaults)
3. [Controlled Mutations](./types_of_references.md#controlled-mutations-own)
4. [Shared Mutability](./types_of_references.md#shared-mutability-shared)
5. [Borrowed Mutability](./types_of_references.md#borrowed-mutability-borrow)
6. [Parameters, Return Values and Local Variables](./types_of_references.md#parameters-return-values-and-local-variables)
7. [Giving up ownership](./types_of_references.md#giving-up-ownership)
8. [Constants](./types_of_references.md#constants-const)
9. [Optional](./types_of_references.md#optional)

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