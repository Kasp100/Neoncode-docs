[← Go back](./intro.md#7-arrays)

# Arrays

Arrays are used to hold N amount of objects of the same type.  
Array references require the `mut:` prefix to be able to change the array's values (i.e. to set an element to reference a different object, not change the object).  

## Types of Arrays

In Neoncode, there are four types of arrays:

**Fixed-size array**: size is a constant known at compile time  
**Runtime-sized array**: size is determined when the array is created and cannot be changed later  

Which each can be divided further according to their semantics:

**`shared` semantics**: `sharing_array` - accept and returns `shared` references
**`own` semantics**: `owning_array` - accepts and returns `own` references

Here's full list:

|                      Syntax                      |                  Alias                  |          Description          |
|--------------------------------------------------|-----------------------------------------|-------------------------------|
| `sharing_array<type element_type, uint size>`    |                                         | Fixed-size sharing array      |
| `sharing_array<type element_type>`               |                                         | Runtime sized sharing array   |
| `owning_array<type element_type, uint size>`     | `array<type element_type, uint size>`   | Fixed-size owning array       |
| `owning_array<type element_type>`                | `array<type element_type>`              | Runtime size owning array     |

### Conversion Rules
- A fixed-size array can be converted into a runtime-sized array.
- A runtime-sized array **cannot** be converted into a fixed-size array, since its size is not known at compile time.

## Reference Semantics of Array Elements

It is possible to have an array that carries mutable references. E.g. `array<mut:string_builder>`
This way it is possible to get `mut:` references from the array.

In arrays, all assignments and reads operate on references. Mutating an element does not mutate the array, but the object (to which the array has a reference).
This model applies to each type of array, and ensures high performance with predictable semantics.

### `sharing_array`

In sharing arrays, elements behave like `shared` fields in a class:

Multiple arrays or variables may share references to the objects behind its element.

### `owning_array`

In owning arrays, elements behave like regular (`own`) fields inside a class:

Only the array has a mutable reference to the objects behind its elements.

## Interacting with Arrays

Both fixed-size and runtime-sized arrays have the same interface.

Here's a full list:

### Getting the size of an array

`get_size()` returns the size of the array. This is independent of whether it's a fixed-size or runtime-sized array.

### Getting elements from the array

Depending on the element type (`element_type`), it returns a mutable or immutable reference.

#### Sharing arrays

Sharing arrays cannot have empty values, but can return empty optionals if the index is out of range.

|                            Signature                            |                                     Summary                                     |       If `index >= size`       |
|-----------------------------------------------------------------|---------------------------------------------------------------------------------|--------------------------------|
| `opt element_type get(uint index)`                              | Returns the element at the given index, or an empty optional if out of bounds.  | returns empty optional         |
| `element_type get_throws(uint index) throws index_out_of_range` | Returns the element at the index or throws if out of bounds.                    | throws `index_out_of_range`    |

#### Owning arrays

In owning arrays, getting elements removes them. This ensures no `mut:` references are duplicated, abiding by its `own` semantics.
Owning arrays may have empty values. This is handled through optionals or exceptions.

|                                         Signature                                         |                               Summary                               |       If `index >= size`       |      If no value present     |
|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------------|------------------------------|
| `opt element_type move_out(uint index)`                                                   | Moves the element at the given index or returns an empty optional.  | returns empty optional         | returns empty optional       |
| `element_type move_out_throws(uint index) throws index_out_of_range, no_element_present`  | Returns the element at the index or throws.                         | throws `index_out_of_range`    | throws `no_element_present`  |

### Setting elements in an array

Setting elements in an array is considered mutating it. It requires a `mut:` reference to the array.

#### Sharing arrays

Setting elements in a sharing array requires it to have `shared` semantics (default for local variables, parameters, and return values)

|                                      Signature                                      |                               Summary                               |       If `index >= size`       |
|-------------------------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------------|
| `opt index_out_of_range set(uint index, element_type new_value) mut`                | Attempts to set the value at the index. Returns an optional error.  | returns optional with error    |
| `void set_throws(uint index, element_type new_value) mut throws index_out_of_range` | Sets the value or throws if out of bounds.                          | throws `index_out_of_range`    |

#### Owning arrays

Setting elements in owning arrays can be done in the following ways depending on the type:
- `copy` (for copyable types)
- `opt:move(v)`: Getting a value out of an `opt`. This makes the optional empty.
- `pass`: Giving up the reference. Cannot be used with fields or if the reference is needed later.

|                                          Signature                                          |                               Summary                               |       If `index >= size`       |
|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------------|
| `opt index_out_of_range move_in(uint index, own element_type new_value) mut`                | Attempts to move the value to the index. Returns an optional error. | returns optional with error    |
| `void move_in_throws(uint index, own element_type new_value) mut throws index_out_of_range` | Moves the value or throws if out of bounds.                         | throws `index_out_of_range`    |

## Array Examples

```
pkg main;

import std::stringable;

class player mut implements stringable
{
    string name;
    var int score = 0;

    public constructor(string name)
    {
        this.name = name;
    }

    public void increment_score() mut
    {
        ++score;
    }

    public string to_string() override
    {
        ret name + " " + score;
    }

}

class using_arrays_example
{
    void use_sharing_array(logger l)
    {
        mut:sharing_array<mut:player> sa =
        (
            player("Alice"),
            player("Bob")
        );

        player b = sa.get(1);

        l::info(b);// output: "Bob"

        // setting

        mut:player c = ("Carol");

        sa.set(1, c);

        l.info(std::array_utilities::to_string(sa));// output: "Alice", "Carol"
    }

}

```

[→ Next: Developer Guidelines and Naming Conventions](./developer_guidelines_and_naming_conventions.md)