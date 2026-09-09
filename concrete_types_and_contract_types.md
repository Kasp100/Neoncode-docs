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


## Concrete Implementation Type

Concrete types do not have abstract methods.

Example:

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


## Contract Types

Contract types only have abstract methods. They are implicitly public and abstract.

Example:

```
pkg examples::contract_types;

import examples::concrete_types::point;

public contract_type shape // This contract type requires subtypes to be immutable
{
	real get_area();
}

public contract_type has_location mut // This contract type does not require subtypes to be immutable, but allows it
{
	point get_location();
}

// Concrete type implementing two contracts
public impl_type rectangle impl shape, has_location
{
	point location;
	nat width;
	nat height;

	public constructor(point init_location, nat init_width, nat init_height)
	{
		location = init_location;
		width = init_width;
		height = init_height;
	}

	public real get_area() impl
	{
		ret width * height;
	}

	public point get_location() impl
	{
		ret location;
	}

}

```


## Abstract Implementation Types

Abstract implementation type can have


Example:

```
pkg examples::abstract_implementation_types;

import examples::concrete_types::point;
import examples::contract_types::has_location;

import some_graphics_lib::canvas;

public abstract impl_type drawable mut impl has_location // This type may be mutable because "has_location" allows it
{
	point location;

	implementers constructor(point init_location)
	{
		location = init_location;
	}

	public point get_location() impl
	{
		ret location;
	}

	public void draw(mut:canvas c) abstract;

}

public impl_type square impl drawable mut
{
	var nat side;

	public constructor(nat init_side)
	{
		side = init_side;
	}

	public void get_side()
	{
		ret side;
	}

	public void set_side(new_side) mut
	{
		side = new_side;
	}

	public void draw(mut:canvas c) impl
	{
		point pos = super.get_location(); // "super" refers to the abstract implementation type this is a subtype of.

		int x = pos.get_x();
		int y = pos.get_y();

		c.fill_rectangle(x, y, x + side, y + side;)
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

public contract_type value_obj impl equatable, hashable {}
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


void main()
{
	use impl_drawable_for_circle;

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