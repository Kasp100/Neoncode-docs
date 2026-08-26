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
pkg my_project;

operator __ ^ _
{
	subordination 1;
	associativity right;
}

operator _ root __
{
	subordination 1;
	associativity right;
}

operator __ * __
{
	subordination 2;
	associativity left;
}

operator __ / __
{
	subordination 2;
	associativity left;
}

operator __ + __
{
	subordination 3;
	associativity left;
}

operator __ - __
{
	subordination 3;
	associativity left;
}
```


```
pkg my_project::temperature;

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

public operator_function_set temperature_notation
{
	temperature (double v)°K
	{
		ret temperature_conversions::from_kelvin(v);
	}

	temperature (double v)°C
	{
		ret temperature_conversions::from_celcius(v);
	}

	temperature (double v)°F
	{
		ret temperature_conversions::from_fahrenheit(v);
	}
}

use temperature_notation;

public class temperature serialisable
{
	double value_kelvin;

	public constructor(double set_value_kelvin)
	{
		value_kelvin = set_value_kelvin;
	}

	public double get_value_kelvin()
	{
		ret value_kelvin;
	}

}

pure_function_set temperature_conversions
{
	temperature from_kelvin(double value)
	{
		ret temperature(value);
	}

	temperature from_celcius(double value)
	{
		ret temperature(value+273.15);
	}

	temperature from_fahrenheit(double value)
	{
		ret temperature((value+459.67)/1.8);
	}
}
```

[→ Next: Compile Functions](./compile_functions.md)