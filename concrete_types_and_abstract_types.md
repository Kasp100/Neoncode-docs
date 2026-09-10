[← Go back](./intro.md#9-concrete-types-and-abstract-types)

# Concrete Types and Abstract Types

A concrete type can be instantiated, while abstract types must be instantiated through a concrete subtype that implements its abstract methods.


## Concrete Types

Concrete types can be instantiated, but cannot have abstract methods or subtypes.

Example:

```
pkg examples::concrete_types;

public type point
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


## Fully Abstract Types

Fully abstract types, cannot be instantiated directly only have abstract methods. Methods inside fully abstract types are implicitly public and abstract.

Example:

```
pkg examples::abstract_types;

import examples::concrete_types::point;

public abstract type shape // This abstract type requires subtypes to be immutable.
{
	real get_area();
}

public abstract type locatable mut // This abstract type does not require subtypes to be immutable, but allows it.
{
	point locate();
}

// Concrete type implementing two contracts
public type rectangle impl shape, locatable
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

	public impl real get_area() // "impl" is used to make clear that this method implements a
	{
		ret width * height;
	}

	public impl point locate()
	{
		ret location;
	}

}

```


## Partially Implemented Abstract Types

Partially implemented abstract types may define state, concrete behavior, and abstract methods.

Unlike in fully abstract types, `abstract` is used to explicitly define an abstract method.

Example:

```
pkg examples::partially_implemented_abstract_types;

import examples::concrete_types::point;
import examples::abstract_types::locatable;

import some_graphics_lib::canvas;

public abstract type drawable mut impl locatable partial_impl
{
	point location;

	implementers constructor(point init_location)
	{
		location = init_location;
	}

	public impl point locate()
	{
		ret location;
	}

	public abstract void draw(mut:canvas c);

}

public type square impl drawable mut
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

	public impl void draw(mut:canvas c)
	{
		point pos = super.locate(); // "super" refers to this type's supertype.

		int x = pos.get_x();
		int y = pos.get_y();

		c.fill_rectangle(x, y, x + side, y + side;)
	}

}

```


## Combining Fully Abstract Types

Fully abstract types can also "implement" other fully abstract types to combine the contracts.

Example:

```
pkg examples::combining_abstract_types;

import std::equatable;
import std::hashable;

public abstract type value_obj impl equatable, hashable {}
// Note: "equatable" and "hashable" must also be fully abstract types.

```


## Extendable and Extensions

An extension is an external implementation of a fully abstract type for an extendable type that does not already implement it.
An extension does not define a type, but adds a supertype to an existing type.

Extensions get a generated name. They are also package members, so they can be public/private and imported from other packages.

Similar to operator modules, they are activated with `use`.

Example:

```
pkg examples::extensions;

import some_graphics_lib::canvas;

public type circle extendable
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
	public impl void get_area()
	{
		ret math::PI * radius * radius;
	}
}


void main()
{
	use impl_shape_for_circle;

	shape s = circle(10);

	std::console::print_line(s.get_area()); // 314.159265...
}

```


[→ Next: Neoncode Examples](./examples_1.md)