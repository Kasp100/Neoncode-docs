[← Go back](./intro.md#12-operators-and-operator-function-sets)

# Operators and Operator Function Sets

**Operators** and **operator function sets** work together to extend how you can write **expressions**.


## Operators

Operators (`operator`) define the syntax of an operator.

Operators are scoped to their package hierarchy and are not importable/exportable as named members.

They have:
- a grammar: a set of syntax tokens with parameters (untyped) within round brackets `()`, e.g. `(a) + (b)` which defines them
- a `subordination` level (the opposite of precedence, with 0 being the highest precedence)
- an optional `associativity` (`left`/`right` associativity)

### Using Operators

An operator is usable in the package they are defined in, including subpackages.
However, if they don't have any meaning, they are useless in expressions.

> Operators: define **syntax**  
> Operator function sets: define **meaning**/**semantics**


## Operator Function Sets

Operator function sets (`operator_function_set`) define what operators **do**.

### Using Custom Operator Function Sets

A custom operator function set applies where it is the `use` keyword is used.

Operator function sets will apply within blocks (between `{}` backets) where the `use` keyword is used with its package member name or path.
If the `use` statement is outside any `{}` brackets, it applies to the **whole file**.


## Usage Tips

It is possible to immediately make the operator function set apply to the whole file:

`use some_lib::some_sub_package::big_decimal_math`.

This works for most use cases.

Alternatively, you can import an operator function set so you can directly reference it by its name:

`import some_lib::some_sub_package::big_decimal_math`

and put `use` statements where it is needed:

`use big_decimal_math`

if you only want this operator function set to apply in specific locations.

### Defining Custom Operator Function Sets

Operator function sets may contain constants and **operator functions**, a kind of **pure function** that uses an operator instead of a name.

An operator function has a return type and requires **typed parameters**.


## Examples

```
pkg my_project;

operator (lhs) ^ (rhs)
{
	subordination 1;
	associativity right;
}

operator (lhs) root (rhs)
{
	subordination 1;
	associativity right;
}

operator (lhs) * (rhs)
{
	subordination 2;
	associativity left;
}

operator (lhs) / (rhs)
{
	subordination 2;
	associativity left;
}

operator (lhs) + (rhs)
{
	subordination 3;
	associativity left;
}

operator (lhs) - (rhs)
{
	subordination 3;
	associativity left;
}
```


```
pkg my_project::temperature;

operator (v) °K
{
	subordination 0;
}

operator (v) °C
{
	subordination 0;
}

operator (v) °F
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