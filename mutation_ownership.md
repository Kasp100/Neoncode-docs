[← Go back](./intro.md#4-mutation-ownership)

# Mutation Ownership

In Neoncode, when working with mutable types (or unknown types from generics), **mutating permission** and **mutation ownership** become relevant.


## Mutation Control Levels

In Neoncode, there are three mutation control levels, from safest to least safe:

1. [Owned Mutations](#owned-mutations-own) (safest)
2. [Borrowed Mutations](#borrowed-mutations-borrow)
3. [Shared Mutations](#shared-mutations-shared) (least safe)

These levels describe how safe an object is from mutations through other references.

**Rules**:

- A reference with a less safe control level **cannot** provide references with a safer control level. *
- A reference with a safer control level **can** provide references with a less safe control level **only without mutating permission**.
- New objects start with the safest control level alongside mutating permission.

\* Unless the object if copied or [mutation ownership is lost when passing](#giving-mutating-permission).


### Owned Mutations (`own`)

You own mutations of the object. It cannot be mutated through other references unless the mutations are temporarily [borrowed](#temporarily-owned-mutations-borrow).

The mutations owner's state also includes the object's state. This means mutating the object is considered mutating the mutations owner.


### Borrowed Mutations (`borrow`)

You may temporarily use an [Owned Mutations (`own`) reference](#owned-mutations-own) without owning mutations of the object.

There's no other reference through which is can mutate the object. This means operations that require mutating permission over the object are blocked.

A borrow remains active until the borrowed reference is no longer used. Once the compiler determines that the reference has no further uses, the borrowed mutation control is returned to its owner.

> This concept is only applicable for **and parameters, return values, local variables.**


### Shared Mutations (`shared`)

You don't own mutations of the object. It may be mutated through other references.

The object is separate from reference holder's state, but the reference itself still is.

> Use this mutation control level cautiously to avoid unintended side effects.


## Default Mutation Control Levels

Without keyword:

- **Fields** and **local variables** default to [`own` - Owned Mutations](#owned-mutations-own).
- **Parameters** and **return values** default to [`borrow` - Borrowed Mutations](#borrowed-mutations-borrow).


## Giving Mutation Ownership (`give`)

An [Owned Mutations](#owned-mutations-own) reference can be provided from an existing reference either by handing over mutations ownership or by copying the object.

A `give` expression transfers mutations ownership and mutating permission (if present) over an object.

There are two kinds:


### 1. Give and Downgrade

**Syntax**: `give local_reference`

- `local_reference` must be the name of a local variable or parameter in this scope.
- `local_reference` is downgraded to a [Shared Mutations](#shared-mutations-shared) reference **without mutating permission**.


### 2. Give and Reassign

**Syntax**: `give local_or_field_reference = replacement_expression`

- `local_or_field_reference` must be the name of a local variable or parameter in this scope, or the name of a field within the current type.
- `local_or_field_reference` must be [reassignable](./mutating_access.md#reassignable-references) (`var`).
- `local_or_field_reference` is reassigned to the result of `replacement_expression`, which be the correct reference type.


## Reference Providing Matrix

This matrix shows which mutation control levels and mutating permission status can be obtained from an existing reference without copying the referenced object.

How to interpret these:
- "**give**" means the existing reference can provide the requested reference only by [giving up its current mutation ownership](#giving-mutation-ownership-give).
- "**pass**" means the existing reference can provide the requested reference through ordinary reference passing, while retaining its own mutation control level.

|  From  |                    |   `own mut:T`   |     `own T`     | `borrow mut:T`  |   `borrow T`    | `shared mut:T`  |   `shared T`    |
| ------ | ------------------ | --------------- | --------------- | --------------- | --------------- | --------------- | --------------- |
| **To** | **`own mut:T`**    | give            | /               | /               | /               | /               | /               |
|        | **`own T`**        | give            | give            | /               | /               | /               | /               |
|        | **`borrow mut:T`** | pass, give      | /               | pass            | /               | /               | /               |
|        | **`borrow T`**     | pass, give      | pass, give      | pass            | pass            | /               | /               |
|        | **`shared mut:T`** | give            | /               | /               | /               | pass            | /               |
|        | **`shared T`**     | pass, give      | pass, give      | pass            | pass            | pass            | pass            |


[→ Next: Access Control & Imports](./access_control_and_imports.md)
