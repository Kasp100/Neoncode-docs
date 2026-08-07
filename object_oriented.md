[← Go back](./intro.md#8-object-oriented)

# Object Oriented Programming

Neoncode is object-oriented, so it allows inheritance and polymorphism.

## Implementing Interfaces

A class or abstract class can implement interfaces using the `impl` keyword.
Methods that implement an interface must use the `override` keyword after its brackets.

Code example:
```
pkg main;

import imaginary_lib::canvas;

public interface drawable
{
	void draw(mut:canvas c);
}

public class rectangle impl drawable
{
	nat width;
	nat height;

	public constructor(nat set_width, nat set_height)
	{
		width = set_width;
		height = set_height;
	}

	public void draw(mut:canvas c) override
	{
		c.fill_rect(0, 0, width, height); // "canvas" is an imaginary graphics API
	}

}

```
**Note**: A class or abstract class can implement **several interfaces**, but can extend only one class or abstract class.

## Extending Classes

Extending classes is possible if the class is marked `extendable` or is astract.
This makes it possible to override all methods marked `overridable`.

The supertype constructor needs to be called with `super(...)` (where `...` is replaced with parameters) inside the subtype's constructor.
`super` is also the keyword that allows calling the supertype's implementation.

Example:
```
pkg main;

import imaginary_lib::canvas;

public class rectangle extendable
{
	nat width;
	nat height;

	public constructor(nat set_width, nat set_height)
	{
		width = set_width;
		height = set_height;
	}

	public void draw(mut:canvas c) overridable
	{
		c.fill_rect(0, 0, width, height);
	}

	public nat calculate_area()
	{
		ret width * height;
	}

}

public class rectangle_with_triangle_inside extends rectangle
{
	public constructor(nat set_width, nat set_height)
	{
		super(set_width, set_height); // When extending, the supertype constructor needs to be called first inside the constructor
	}

	public void draw(mut:canvas c) override // It needs to be explicitly stated that overriding is intended
	{
		super.draw(c); // Call the supertype to do its behaviour
		c.set_colour(c.get_colour().get_contrasting());
		c.fill_triangle(0, 0, width, height);
	}
}

```

## Using Abstract Classes

Abstract classes are partially implemented classes. They do not need the keyword `extendable` to be able to be extended.

They can be fully or partially implemented by a regular or abstract class with the `extends` keyword.  
> Keep in mind that it's only possible to extend one class or abstract class.

An `abstract` method is a method without implementation.
An implemented method marked `overridable` allows overriding in subtypes.

All methods a subtype implements, need to be marked with `override` to explicitly state the intention of implementing.

Code example:
```
pkg main;

import imaginary_lib::canvas;

public abstract class rectangular_painting
{
	nat width;
	nat height;

	public constructor(nat set_width, nat set_height)
	{
		width = set_width;
		height = set_height;
	}

	public abstract void draw(mut:canvas c);

	public nat calculate_area()
	{
		ret width * height;
	}

}

public class triangle_painting extends rectangular_painting
{
	public constructor(nat set_width, nat set_height)
	{
		super(set_width, set_height);
	}

	public void draw(mut:canvas c) override
	{
		super.draw(c); // Call the supertype to do its behaviour
		c.set_colour(c.get_colour().get_contrasting());
		c.fill_triangle(0, 0, width, height);
	}
}

```

## Extending Interfaces

Interfaces can also be **extended** by other interfaces using the `extends` keyword. This allows combining interfaces together.  
This does not require the other to be marked `extendable`.


## Default Interface Implementations

Interfaces can have default implementations of methods.
By marking a method `default`, an implementation is allowed;

```

pkg main;

import imaginary_lib::canvas;

public interface drawable
{
	void draw(mut:canvas c) default
	{
		c.fill_rect(10, 10, 20, 20);
	}
}
```


[→ Next: Neoncode Examples](./examples_1.md)