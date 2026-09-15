[← Go back](./intro.md#constructors)

# Constructors

A **constructor** is a special method used to initialise an instance of a type. Unlike constructors in many object-oriented languages, Neoncode constructors are explicitly named or anonymous and have an explicit return type.

Constructors may be used to establish invariants and initialise the fields of an instance. A constructor returns the initialised instance through `self`.


## Anonymous constructors

A type may define at most one anonymous constructor. An anonymous constructor has no name and allows an instance to be created using the type name directly.

```
type element
{
    string name;

    public constructor element (own string init_name)
    {
        name = give init_name; // or "self.name = give name" if "init_name" was "name"
        ret self;
    }
}
```

The constructor can then be called as:

- `element("neon")`, or

- `("neon")` if the type is explicit.

An anonymous constructor is optional. If a type does not define one, this construction syntax is not available for that type.

Examples:

```

element create_element_1()
{
    element new_element = element("neon");

    ret new_element;
}

element create_element_2()
{
    element new_element = ("neon");

    ret new_element;
}

element create_element_3()
{
    element = element("neon"); // Local variable name is inferred from the type.

    ret element;
}

element create_element_4()
{
    element = ("neon"); // Local variable name is inferred from the type.

    ret element;
}

element create_element_5()
{
    ret element("neon");
}

element create_element_6()
{
    ret ("neon");
}

```



## Named constructors

A type may define any number of named constructors. Named constructors are called using the type name followed by the constructor name.

```
type user
{
    string name;

    public constructor user from_name(own string init_name)
    {
        name = give init_name;
        ret self;
    }

    public constructor user anonymous()
    {
        name = "Anonymous";
        ret self;
    }
}
```

These constructors are called as:

```
user user = user.from_name("Kasp");
user anonymous = user.anonymous();
```

Named constructors are useful when a type has multiple distinct ways of being initialised.


## Constructor return types

Constructors have explicit return types. A constructor may return the constructed type directly:

```
public constructor user (string init_name)
{
    name = give init_name;
    ret self;
}
```

A constructor may instead return another type, such as `result<T>`, when initialisation can fail:

```
public constructor result<user> (own string init_name)
{
    if(name == "")
    {
        ret result::err("invalid name");
    }

    name = give init_name;

    ret result::of(self);
}
```

A constructor returning `result<user>` must return a `result<user>` rather than implicitly converting a `user` into a `result<user>`.

This allows construction failures to be handled explicitly without requiring exceptions.


## Supertype initialisation

When a type derives from a semi-abstract type, its constructor must initialise the inherited portion of the instance by explicitly invoking a constructor of the supertype.

```
semi_abstract type vehicle
{
    string plate;

    implementers constructor vehicle (own string init_plate)
    {
        plate = give init_plate;
        ret self;
    }
}

type car impl vehicle
{
    string model;

    public constructor(own string init_plate, own string init_model)
    {
        super = (init_plate); // or "vehicle(init_plate)", because "vehicle" is semi-abstract

        model = give init_model;

        ret self;
    }
}
```

The `super =` statement explicitly selects the supertype constructor used to initialise the inherited portion of the instance.

Constructors therefore do not implicitly invoke a particular supertype constructor. The constructor to use must be specified explicitly.


## Constructors and static methods

Constructors and static methods are both type-level operations that can produce instances, but constructors have additional initialisation semantics.

A constructor may initialise fields and the inherited portion of an instance. A static method does not have these special initialisation semantics and may be used for any operation that produces a value.

For example:

```
public static result<user> load_from_file(string path) io
{
    ...
}
```

is an ordinary static method, even though it returns a `user`.

Named constructors provide an explicit and convenient way to distinguish different forms of initialisation without requiring construction to be tied to a single unnamed operation.


