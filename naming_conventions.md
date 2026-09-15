[← Go back](./intro.md#naming-conventions)

# Naming conventions

The following naming conventions are recommended as the default style for Neoncode programs. They are conventions rather than syntactic requirements, and implementations and libraries may use other conventions where appropriate, particularly when interoperating with external APIs.


## General names

Names should generally use **lowercase snake case**:

```
user_name
calculate_total
maximum_value
ip_address
```

Spaces and dashes in names should be written as underscores:

```
"hello world" → hello_world
"some-value"  → some_value
```

This convention applies to most identifiers, including variables, functions, types, fields, methods, and other named declarations.


## Constants

Names representing [constants](./constants.md) should use **uppercase snake case**:

```
MAX_CONNECTIONS
DEFAULT_TIMEOUT
PI
```


## Generic parameters

Generic parameters should generally use a **single uppercase character** when their meaning is clear from context:

```
T first<type T>(sequence<T> items) {...}
```

The following letters are recommended for common meanings:

| Parameter | Meaning |
| --------- | ------- |
| `T`       | type    |
| `I`       | index   |
| `E`       | element |
| `K`       | key     |
| `V`       | value   |
| `L`       | length  |
| `S`       | size    |

For example:

```
V get<type K, type V>(map<K, V> map, K key) {...}
```

When a single-letter name would be unclear, generic parameters should instead use **uppercase snake case**:

```
TARGET convert<type SOURCE, type TARGET>(SOURCE value) {...}
```

Generic parameter names should communicate their semantic role rather than merely being chosen for brevity.
