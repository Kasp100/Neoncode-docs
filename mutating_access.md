[← Go back](./intro.md#3-mutating-access)

# Mutating Access

Unlike languages where a single keyword such as `const` may conflate or ambiguously represent different forms of immutability, Neoncode distinguishes reassignment from object mutation.  
In Neoncode, the keywords `mut` (mutating/mutable) and `var` (reassignable) are used to explicitly allow mutations to data where needed.

The benefit of this model is that code looks cleaner and intentions are more explicit.


## Reassignable References

By default, references keep pointing to the same object from the moment they were initialised.  
**Reassignable references** have the additional capability of being able to change **which object they refer to**.

Assigning and reassigning is always done with the `=` (single equals sign) operator.


### Syntax

Putting the keyword `var` first in the reference's declaration makes the reference **reassignable**.

**Example**:

```

var nat my_first_natural_number = 1;
my_first_natural_number = 3;          // ✅ Allowed because "my_natural_number" is a reassignable reference.

nat my_second_natural_number = 2;
my_second_natural_number = 5;         // ❌ Error because "my_second_natural_number" is a normal reference.

```


## Mutable Types and Mutating Methods

A **mutation** is considered a change of the state of an object.  
For example, a type named `lamp` may have a boolean state `on` which can be flipped `true` or `false`. This change is considered a mutation.

In Neoncode:
- A **mutable type** has a state that can change through its own **mutating methods**.
- A **mutating method** has permission to change the object's state.


### Syntax

- To make a type (interface, abstract class, or concrete class) **mutable**, the `mut` keyword is placed **after the type name** in the type's declaration.
- To make a method **mutating**, the `mut` keyword is placed immediately after the parameter declarations.

**Example**:

```

class lamp mut
{
    var bool on = false;

    public bool is_on()
    {
        ret on;
    }

    public void toggle() mut
    {
        on = !on;                 // ✅ Allowed because "toggle()" is marked mutating with "mut".
    }

    public void invalid_toggle()
    {
        on = !on;                 // ❌ Error because "invalid_toggle()" is not marked mutating.
    }

}

```


## Reference Mutating Permission (`mut:`)

In Neoncode, references can be used to call **non-mutating** methods on objects.  
A reference with **mutating permission** has the additional ability of mutating the object through the type's **mutating methods**.

Mutating permission is also required to provide new references with mutating permission.


### Syntax

In reference declarations, the prefix `mut:` before the type gives this reference **mutating permission** over the object it refers to.

**Note**: The colon may be omitted as a syntactic shorthand, but `mut:` is the canonical form used throughout this specification.

**Example**:

Using the `lamp` type from the previous example.

```

lamp l0 = lamp();      // ✅ Without "mut:", the reference does not have mutating permission.
l0.is_on();            // ✅ Allowed because "is_on()" is not mutating.
l0.toggle();           // ❌ Error because "l0" does not have mutating permission.

mut:lamp l1 = lamp();  // ✅ Allowed because "lamp" is a mutable type.
l1.is_on();            // ✅ "mut:" just adds mutating permission.
l1.toggle();           // ✅ Allowed because "l1" has mutating permission.

mut:int i = 7;         // ❌ Error because "int" is an immutable type.

```

For immutable types, mutating permission is useless, so this results in an error.

> Developers must clearly state their intentions by using `mut:` or not.


## Difference between `var` and `mut:`

The `mut:` prefix allows mutating the object behind the reference, while `var` allows reassigning the reference.


## `mut` in different places

- `mut:` in a **reference declaration** allows the reference to be used for **mutating access** of the object behind it.
- `mut` in a **type declaration** makes it a **mutable type**, meaning its instances may have state that can be modified through its **mutating methods**.
- `mut` in a **method declaration** makes it a **mutating method**, allowing it to modify the object's state.


[→ Next: Mutation Ownership](./mutation_ownership.md)