[← Go back](./intro.md#14-pure-functions)

# Pure Functions

> The concept of pure functions is not specific to Neoncode

A **pure function** is a function that always returns the **same result** for the **same input** and has **no side effects**.

Pure functions:
- Do not read or write mutable state
- Do not perform I/O
- Do not mutate arguments
- Do not rely on or modify global or shared data

This means their behavior is entirely predictable, like math.

> Mathematical operations are pure functions, e.g. 7+2 is always 9

Pure functions are a foundation of safe and maintainable code. They:

- Improve performance:
    - The compiler can optimise pure functions more effectively
- Improve testability:
    - No dependencies
    - Easy to test in isolation
- Enhance readability:
    - Clear intent
    - Clear behavior
- Reduce bugs:
    - No unintended side effects
- Enable concurrency:
    - No shared state means no race conditions
- Allow safe refactoring:
    - Local changes won’t affect global state

## Keyword `pure`

- Can be used instead of `static` if the function is in fact pure.
- Should be preferred over `static`
- States the intention of the programmer
- Enforces compiler checks to ensure the function is pure

Here's an example:

```
pkg main;

import std::codegen::public_getters;

class person mut
{
    string name;
    var nat age;

    public constructor(string name)
    {
        if(is_empty(name))
        {
            throw illegal_argument("name cannot be empty", name);
        }
        this.name = name;
        this.age = 0;
    }

    auto:public_getters(name, age);
    
    public void increment_age() mut
    {
        ++age;
    }

    public pure bool is_empty(string)
    {
        ret string.get_size() == 0;
    }

}
```


## Pure Function Sets

A pure function set is collection of pure functions.

```
pkg std;

import std::collections::list;

public pure_function_set collection_utilities
{
	public <item_type> list<item_type> list_of(array<item_type> items)
	{
		mut:list<item_type> l = ();

		foreach items (item_type i)
		{
			l.add(i);
		}

		ret l;
	}

}
```

[→ Next: Functional & Lambda](./functional_and_lambda.md)