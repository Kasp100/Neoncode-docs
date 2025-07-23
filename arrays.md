[← Go back](./intro.md#7-arrays)

# Arrays

Arrays are used to hold N amount of objects of the same type.  
Array references require the `mut:` prefix to be able to change the array's values (i.e. to set an element to reference a different object, not change the object).  

## Types of Arrays

`array` is an overloaded type — it has several types, based on its generic parameters.

The following overloads exist:
- `array<immut_type elem_type>`
- `array<immut_type elem_type, nat size>`
- `array<mut_type__own elem_type>`
- `array<mut_type__own elem_type, nat size>`
- `array<mut_type__own_mut elem_type>`
- `array<mut_type__own_mut elem_type, nat size>`
- `array<mut_type__shared elem_type>`
- `array<mut_type__shared elem_type, nat size>`
- `array<mut_type__shared_mut elem_type>`
- `array<mut_type__shared_mut elem_type, nat size>`

### Fixed-size & Runtime-sized

**Fixed-size arrays**: size is a constant known at compile time: `nat size`  
**Runtime-sized arrays**: size is determined when the array is created and cannot be changed later (without size parameter)  

Conversion Rules:
- A fixed-size array can be converted into a runtime-sized array.
- A runtime-sized array **cannot** be converted into a fixed-size array, since its size is not known at compile time.

### Arrays of immutable type

These are the overloads with `immut_type`.

**Example**: `array<string>` (`string` is immutable)

**API**:
`nat get_size()`
`opt elem_type get(nat index)`
`elem_type get_throws(nat index) throws index_out_of_range`
`elem_type set(nat index, elem_type replacement) mut`
`elem_type set_throws(nat index, elem_type replacement) mut throws index_out_of_range`

### Arrays of immutable `own` references

These are the overloads with `mut_type__own`.

**Example**: `array<own string_builder>` (`string_builder` is mutable)

**API**:
`nat get_size()`
`opt borrow elem_type get(nat index)`
`borrow elem_type get_throws(nat index) throws index_out_of_range`
`own elem_type set(nat index, own elem_type replacement) mut`
`own elem_type set_throws(nat index, own elem_type replacement) mut throws index_out_of_range`

**Note**: `get` and `get_throws` return an immutable `borrow` reference. Immutable borrow references are effectively the same as immutable shared references.

### Arrays of mutable `own` references

These are the overloads with `mut_type__own_mut`.

**Example**: `array<own mut:string_builder>` (`string_builder` is mutable)

**API**:
`nat get_size()`
`opt borrow mut:elem_type get(nat index)`
`borrow mut:elem_type get_throws(nat index) throws index_out_of_range`
`own mut:elem_type set(nat index, own mut:elem_type replacement) mut`
`own mut:elem_type set_throws(nat index, own mut:elem_type replacement) mut throws index_out_of_range`

### Arrays of immutable `shared` references

These are the overloads with `mut_type__shared`.

**Example**: `array<shared string_builder>` (`string_builder` is mutable)

**API**:
`nat get_size()`
`opt shared elem_type get(nat index)`
`shared elem_type get_throws(nat index) throws index_out_of_range`
`shared elem_type set(nat index, shared elem_type replacement) mut`
`shared elem_type set_throws(nat index, shared elem_type replacement) mut throws index_out_of_range`

**Note**: `get` and `get_throws` return an immutable `borrow` reference. Immutable borrow references are effectively the same as immutable shared references.

### Arrays of mutable `shared` references

These are the overloads with `mut_type__shared_mut`.

**Example**: `array<shared mut:string_builder>` (`string_builder` is mutable)

**API**:
`nat get_size()`
`opt shared mut:elem_type get(nat index)`
`shared mut:elem_type get_throws(nat index) throws index_out_of_range`
`shared mut:elem_type set(nat index, shared mut:elem_type replacement) mut`
`shared mut:elem_type set_throws(nat index, shared mut:elem_type replacement) mut throws index_out_of_range`

## Array Example

```
pkg main;

import std::stringable;

public class using_arrays_example
{
    public void use_sharing_array(logger l)
    {
        mut:array<shared mut:player> players =
        (
            player("Alice"),
            player("Bob")
        );

        shared player b = players[1]; // or `players.get(1)`

        l::info(b); // output: "Bob 0"

        // setting

        shared mut:player c = ("Carol");

        players[1] = c; // or `players.set(1, c)`

        l.info
        (
            std::array_utilities::to_string(players) // output: "Alice 0", "Carol 0"
        );
    }

}

public class player mut impl stringable
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


```

[→ Next: Developer Guidelines and Naming Conventions](./developer_guidelines_and_naming_conventions.md)