[← Go back](./intro.md#4-mutation-ownership)

# Mutation Ownership

In Neoncode, when working with mutable types (or unknown types from generics), **mutating permission** and **mutation ownership** become relevant.


## Giving Mutating Permission

As described in [the previous chapter](./mutating_access.md), a reference with mutating permission is needed to mutate an object.

This is also required to provide new references with mutating permission.


## Mutation Control Levels

In Neoncode, there are three mutation control levels, from safest to least safe:

1. [Owned Mutations](#owned-mutations-own) (safest)
2. [Borrowed Mutations](#borrowed-mutations-borrow)
3. [Shared Mutations](#shared-mutations-shared) (least safe)

These levels describe how safe an object is from mutations through other references.


### Rules

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


## Default Mutation Ownership

Without keyword:

- **Fields** and **local variables** default to [`own` - Owned Mutations](#owned-mutations-own).
- **Parameters** and **return values** default to [`borrow` - Borrowed Mutations](#borrowed-mutations-borrow).


## Giving Up Mutations Ownership (`give`)

An [Owned Mutations](#owned-mutations-own) reference can be provided from an existing reference either by handing over mutations ownership or by copying the object.

A `give` expression transfers mutations ownership:

1. **The giver loses**:
	- ownership
	- mutating permission (if present).
2. **The receiver gains**:
	- ownership
	- mutating permission if the original reference had it.

**Examples**:
- `own string` → `shared string` (giver), `own string` (receiver)
- `own mut:string` → `shared string` (giver), `own mut:string` (receiver)
- `borrow mut:string` → **cannot be given - no mutations ownership**
- `shared mut:string` → **cannot be given - no mutations ownership**

**Notes**:
- The giver keeps a [Shared Mutations](#shared-mutations-shared) reference **without mutating permission**.
- The result may be discarded, which makes the object effectively finalised.


[→ Next: Access Control & Imports](./access_control_and_imports.md)
