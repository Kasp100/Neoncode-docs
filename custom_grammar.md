[← Go back](./intro.md#12-custom-grammar)

# Custom Grammar

Neoncode allows custom grammar using the `grammar_set` keyword. This indicates the definition of a **grammar set**.  
Custom grammar will be parsed if the `parse` keyword is used. This makes the parser take the custom grammar into account.

## Using Custom Grammar Sets

Grammar sets will apply within blocks (between `{` and `}`) where the `parse` keyword is used.
If the `parse` is not within `{` and `}`, it applies to the **whole file**.

**Tip**:
- Use `import some_lib::some_sub_package::big_decimal_math` and `parse big_decimal_math` if parsing at multiple locations is needed.
- Use `parse some_lib::some_sub_package::big_decimal_math` if parsing is needed at just one location, typically done for the whole file.

## Defining Custom Grammar Sets

A **rule** inside grammar sets is a special kind of **pure function**.

Rules follow the following syntax:
- Optionally start with their **subordination**, an integer to determine the precedence of each rule. (Where **0** is the most precedence / least subordination.)
- Followed by their **return type**.
- Followed by their individual grammar, where curly brackets indicate parameters to be passed.
- Ending in their **function body**

## Example

```
pkg temperature;

public grammar_set temperature_notation
{
	0 temperature (double value)°K
	{
		ret temperature(value);
	}

	0 temperature (double value)°C
	{
		ret temperature(value+273.15);
	}

	0 temperature (double value)°F
	{
		ret temperature((value+459.67)/1.8);
	}

}

public grammar_set temperature_arithmetic
{
	1 temperature (temperature a)*(temperature b)
	{
		ret temperature(a.get_value_kelvin()*b.get_value_kelvin());
	}

	1 temperature (temperature a)/(temperature b)
	{
		ret temperature(a.get_value_kelvin()/b.get_value_kelvin());
	}

	2 temperature (temperature a)+(temperature b)
	{
		ret temperature(a.get_value_kelvin()+b.get_value_kelvin());
	}

	2 temperature (temperature a)-(temperature b)
	{
		ret temperature(a.get_value_kelvin()-b.get_value_kelvin());
	}

}

parse temperature_arithmetic;
parse temperature_notation;

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
```

[→ Next: Compile Functions](./compile_functions.md)