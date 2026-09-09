[← Go back](./intro.md#9-concrete-types-and-contract-types)

# Concrete Types and Contract Types

Neoncode distinguishes between types that define implementations and types that define contracts.
This distinction separates the representation and behavior of concrete values from the requirements that types may be expected to satisfy.

There are three ways to define a type:

- `impl_type` defines a **concrete implementation type**, or simply a **concrete type**.
  Concrete types define an instantiable representation and its associated behavior.

- `abstract impl_type` defines an **abstract implementation type**.
  Abstract implementation types cannot be instantiated directly, but may define state, concrete behavior, and abstract operations.
  Concrete subtypes are required to provide these abstract operations.

- `contract_type` defines a **contract type**.
  Contract types describe behavioral requirements that implementation types may satisfy.
  They do not define an inherited representation or implementation.


## Concrete Implementation Type Example

```
pkg examples::concrete_types;

public impl_type point
{
	int x;
	int y;

	public constructor(int init_x, int init_y)
	{
		x = init_x;
		y = init_y;
	}

	public int get_x()
	{
		ret x;
	}

	public int get_y()
	{
		ret y;
	}

}

```


## Abstract Implementation Type Example

Concrete types become a subtype of an abstract implementation type through implementing it in the same way contract types are implement, through the `impl` keyword.  

All methods a subtype implements, need to be marked with `impl` to explicitly state the intention of implementing.

Example:

```
pkg examples::abstract_implementation_type;

import some_graphics_lib::canvas;

contract_type shape
{
	real get_area();
}

abstract impl_type drawable mut
{
	point position;

	public constructor(point init_position)
	{
		position = init_position;
	}

	implementers point get_position()
	{
		ret position;
	}

	public void draw(mut:canvas c) abstract;

}

impl_type square impl drawable, shape
{
	real side;

	public void draw(mut:canvas c) impl
	{
		point pos = super.get_position(); // "super" refers to the abstract implementation type this is a subtype of.

		int x = pos.get_x();
		int y pos.get_y();

		c.fill_rectangle(x, y, x + side, y + side;)
	}

	public real get_area() impl
	{
		ret side * side;
	}

}

```


## Combining Contract Types

Contract types can also be combined, requiring subtype to implement all contract type.

Example:

```
pkg examples::combining_contract_types;

import std::equatable;
import std::hashable;

public contract_type value_type impl equatable, hashable {}
// Note: "equatable" and "hashable" must also be contract types.

```


## Extendable and Extensions

An extension is an external implementation of a contract type for an extendable type that does not implement it.
An extension does not define a type, but adds a supertype to an existing type where it is activated.

Extensions get a generated name. They are also package members, so they can be public/private and imported from other packages.

Similar to operator modules, they are activated with `use`.

Example:

```
pkg examples::extensions;

import some_graphics_lib::canvas;

public impl_type circle mut extendable
{
	real radius;

	public constructor(real init_radius)
	{
		radius = init_radius;
	}

	extensions real get_radius()
	{
		ret radius;
	}

}

// "shape" extension for "circle"
// generated name: "impl_shape_for_circle"
public impl shape for circle
{
	public void get_area() impl
	{
		ret math::PI * radius * radius;
	}
}

use impl_drawable_for_circle;

void main()
{
	draw
	(
		circle(10),
		canvas(50, 50)
	);
}

void draw(drawable d, mut:canvas c)
{
	d.draw(c);
}

```


[→ Next: Neoncode Examples](./examples_1.md)