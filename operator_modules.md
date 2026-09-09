[← Go back](./intro.md#12-operator-modules)

# Operator Modules

**Operator modules** (`operator_module`) extend how you can write **expressions**.

A custom operator module applies starting from where it is activated with a `use` statement until the end of the block (`}`) or for the rest of the file.

Operator modules can contain `operator` declarations and operator functions.

> Operators: define **syntax**  
> Operator functions: define **semantics**


## Operators

**Operators** (`operator`) inside operator modules define or confirm the **syntax** of an operator.

They have:
- a grammar: a set of syntax tokens with parameters as underscores (`_`), e.g. `__ + __` which defines them
- a `subordination` level (the opposite of precedence, with 0 being the highest precedence)
- an optional `associativity` (`left`/`right` associativity)


## Operator Functions

An operator function defines what an operator **does**.

It is like a function, but instead of a name+parameters based signature, a pattern with `()` brackets around each parameter.
The parameters must be typed here and the function must be pure.

**Conflicts**: If two or more operator functions implement the same operator with the same parameter types *and* are simultaneously applied, the compiler will **not try to disambiguate them** but give an **error**.


## Usage Tips

It is possible to immediately make the operator module apply to the rest of the file:

`use some_lib::some_sub_package::big_decimal_math`.

This works for most use cases.

Alternatively, you can import an operator module so you can directly reference it by its name:

`import some_lib::some_sub_package::big_decimal_math`

and put `use` statements where it is needed:

`use big_decimal_math`

if you only want this operator module to apply in specific locations.


## Examples

```
pkg examples::operators;

public operator_module my_operator_module
{
	operator __ + __
	{
		subordination 2;
		associativity left;
	}

	operator __ * __
	{
		subordination 1;
		associativity left;
	}

}

```

Now `1 + 2 * 3 == 1 + (2 * 3)`. Lower subordination means higher precedence.

```
pkg examples::temperature;

public impl_type temperature
{
	/** Value in Kelvin. */
	real value;

	public constructor(real init_value)
	{
		value = init_value;
	}

	public real get_value()
	{
		ret value;
	}

}

public module temperature_notation
{
	operator __ °K
	{
		subordination 0;
	}

	operator __ °C
	{
		subordination 0;
	}

	operator __ °F
	{
		subordination 0;
	}

	temperature (real v)°K
	{
		ret temperature(v);
	}

	temperature (real v)°C
	{
		ret temperature(v + 273.15)
	}

	temperature (real v)°F
	{
		ret temperature((value + 459.67) / 1.8)
	}
}

void main() io
{
	use temperature_notation;
	std::console::print_line(22°C.get_value()); // Prints "295.15"
}

```


[→ Next: Compile-Time Functions](./compile_time_functions.md)