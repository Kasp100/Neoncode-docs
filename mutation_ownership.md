[← Go back](./intro.md#4-mutation-ownership)

# Mutation Ownership

In Neoncode, when working with mutable types, [mutating permission](./mutating_access.md#reference-mutating-permission-mut) and **mutation ownership** become relevant.

Owning or borrowing mutations over an object means no one else can mutate the object.


## Mutation Control Levels

In Neoncode, there are three mutation control levels:

- [Owned Mutations (`own`)](#owned-mutations-own) (highest)
- [Borrowed Mutations (`borrow`)](#borrowed-mutations-borrow) (middle)
- [Shared Mutations (`shared`)](#shared-mutations-shared) (lowest)

Constructors return [`own` references](#owned-mutations-own) with mutating permission.

**Reference Passing Rules**:

- Higher mutation control levels cannot be obtained from lower mutation control levels.
- Except from [`own`](#owned-mutations-own) to [`borrow`](#borrowed-mutations-borrow), mutating permission must be dropped when passing to a lower mutation control level.
- Finally, as stated in [Reference Mutating Permission](./mutating_access.md#reference-mutating-permission-mut), mutating permission **cannot** be obtained from a reference without it.

The [Reference Providing Matrix](#reference-providing-matrix) further explains how this works.


### Owned Mutations (`own`)

**You own mutations of the object (1) and the object is part of you (2).**

- (1:) It cannot be mutated through other references unless the mutation ownership is temporarily [borrowed](#borrowed-mutations-borrow).

- (2:) The reference holder's state includes the object's state. This means mutating the object is considered mutating the reference holder.


```

class type_1
{
	bool a; // "bool" is an immutable type (values "true" or "false").
	bool b;
}

class type_2
{
	bool first;
	type_1 second; // "own" is implicit with fields.
}

```

- `type_1`'s state is composed of `a` and `b`.
- `type_2`'s state is composed of `first` and `second` (`a` and `b`).



### Borrowed Mutations (`borrow`)

**You temporarily own mutations of the object, until you are done using it locally.**

- Temporarily means you cannot obtain permanent references with equal or higher mutation control level, or with mutating permission from this reference.
  Permanent references include fields and local variables or parameters used in closures.

- As with [Owned Mutations](#owned-mutations-own) references: There are no other references through which the object can be mutated.
  This means any operations that require mutating permission over the object through different references are blocked.

- A borrow remains active until the borrowed reference is no longer used.
  Once the compiler determines that the reference has no further uses, the borrowed mutation control is returned to its original owner.

- Borrows can be obtained from references with mutations ownership ([`own`](#owned-mutations-own) / `borrow`).


### Shared Mutations (`shared`)

**You do not own mutations of the object.**

- The object may be mutated through other references.

- The object is separate from reference holder's state, but the reference itself still is.

- Use this mutation control level cautiously to avoid unintended side effects.


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

This matrix shows each mutation control level and mutating permission status that can be obtained from an existing reference without copying the referenced object.

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


## Mutation Control Examples

```

mut:string s0 = "a";        // ✅ Allowed. Creating new objects creates Owned Mutations references with mutating permission. "own" is implicit for locals.

borrow mut:string s1 = s0;  // ✅ Allowed. This starts a borrow of "s0".

s0.append("b");             // ❌ Error: "s0" temporarily lost mutating permission while the borrow is active.

s1.append("b");             // ✅ Allowed. Now both "s0" and "s1" are "ab". The borrow end after this - "s1" has no uses left.

s0.append("c");             // ✅ Allowed because the borrow just ended.

shared mut:string s2 = s0;  // ❌ Error: Cannot obtain a Shared Mutations reference with mutating permission from any Owned Mutations reference.

shared string s3 = s0;      // ✅ Allowed.

mut:string s3 = give s0;    // ✅ Allowed. Mutations ownership is given.

s0.append("d");             // ❌ Error: "s0" was given, so at this point it's a Shared Mutations reference without mutating permission.

```

```

class container<type T> mut
{
	var opt mut:T v;

	public constructor(own mut:T init_value)
	{
		v = give init_value;
	}

	public mut:T get() mut
	{
		ret v;
	}

	public own mut:T take() mut
	{
		ret give v = empty;
	}

}

```


[→ Next: Access Control & Imports](./access_control_and_imports.md)