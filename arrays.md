[← Go back](./intro.md#7-arrays)

# Arrays

Arrays are used to hold N amount of objects of the same type in an order.  


## Types of Arrays

There are several kinds of arrays in Neoncode.

Note: in Neoncode, "ownership", "borrowing", "sharing", and "view" are terminology for mutating permission rather than memory ownership.

|     Type Name + Parameter     |  Can others mutate the elements?  |  Can you mutate the elements?  |
| ----------------------------- | --------------------------------- | ------------------------------ |
| `array<type E>`               | No                                | No                             |
| `owning_mut_array<type E>`    | No                                | Yes\*                          |
| `sharing_view_array<type E>`  | Yes                               | No                             |
| `sharing_mut_array<type E>`   | Yes                               | Yes                            |

\* Considered to also be a mutation of the array and, as a consequence, a mutation of the owner of that array if the array is **not** `shared`.

Each of these array types has a fixed-length sibling:
- name is prefixed with `fl_` for "fixed-length"
- an additional generic parameter `nat L` for its length

Example: `fl_array<type E, nat L>`

The length of these fixed-length arrays is known at compile time, making them safer and slightly faster.


## Array APIs

### Replacing elements

|       Array Type       |                   API (E: mutable type)                   |       API (E: immutable type)       |
| ---------------------- | --------------------------------------------------------- | ----------------------------------- |
| `array`                | `own E set(index, own E replacement) mut`                 | `E set(index, E replacement) mut`   |
| `owning_mut_array`     | `own mut:E set(index, own mut:E replacement) mut`         | **Usage discouraged**               |
| `sharing_view_array`   | `shared E set(index, shared E replacement) mut`           | **Usage discouraged**               |
| `sharing_mut_array`    | `shared mut:E set(index, shared mut:E replacement) mut`   | **Usage discouraged**               |

Replaces an element in the array, returning the original value at `index`.

This is considered a mutation of the array, even if it's a `sharing_view_array` or a `sharing_mut_array`.


### Getting views of elements

|       Array Type       |           API           |
| ---------------------- | ----------------------- |
| `array`                | `borrow E get(index)`   |
| `owning_mut_array`     | `borrow E get(index)`   |
| `sharing_view_array`   | `shared E get(index)`   |
| `sharing_mut_array`    | `shared E get(index)`   |

Gets a view (a reference **without `mut:`**) of the element at `index`.

This is never considered a mutating operation.

#### O(1) Lookup

Because arrays support constant time lookup, the `[]` syntax is supported according to the language's philosophy. For example:

```
array<int> arr = array::of<int>(-1, 2, 3, 5, 7);
int elem3 = arr[3];
```

`elem3` contains 5.


### Mutating elements

|      Array Type      |                 API                 |
| -------------------- | ----------------------------------- |
| `owning_mut_array`   | `borrow mut:E get_mut(index) mut`   |
| `sharing_mut_array`  | `shared mut:E get_mut(index)`       |

Gets a reference which allows the caller to mutate the element at `index`.

For `owning_mut_array`, this method is marked with `mut` (because it requires returning a `mut:` reference), but the method itself does not mutate the array.
Rather, it returns a mutable `borrow` reference to the caller, meaning the caller can temporarily mutate the element, which is owned by the array.

For `sharing_mut_array`, this method is **not** marked with `mut` because the array does not own its elements, but keeps mutable `shared` references.

The other array types do not keep `mut:` references.


### Getting the lenght

`nat get_length()`

Getting the length of an array is trivial and constant time.


## Array Example

```
pkg main;

import std::stringable;

public class using_arrays_example
{
    public void use_owning_mut_array(logger l)
    {
        mut:owning_mut_array<player> players = owning_mut_array::of<player>
        (
            player("Alice"),
            player("Bob")
        );

        borrow mut:player b = players.get_mut(1)

        l::info(b.to_string()); // output: "Bob 0"

        b.increment_score();

        l::info(players[1].to_string()); // output: "Bob 0"
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