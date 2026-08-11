[← Go back](./intro.md#9-examples-1)

# Neoncode Examples

## 1. Mutable Person Class

```
pkg main;

import std::time::e_date;

public class person mut
{
	e_date birthdate; // Assuming that a person cannot change their birthdate.
	var string name; // Assuming that a person may change their name.

	public constructor(e_date birthdate, string name)
	{
		this.birthdate = birthdate; // "this" is a reference to the object self. It can is used to make sure the field is assigned to, not the parameter.
		this.name = name;
	}

	public e_date get_birthdate()
	{
		ret birthdate;
	}

	public void set_name(string new_name) mut
	{
		name = new_name;
	}

	public string get_name()
	{
		ret name;
	}

}
```

## 2. Bank Account

```
pkg main;

import std::collections::sequence;

public class bank_account mut
{
	string name;
	mut:sequence<transaction> transactions_history;
	shared bank keeper;

	public constructor(bank keeper)
	{
		this.keeper = keeper;
		transactions_history = ();
	}

	public void perform_transaction(transaction tr) mut
	{
		transactions_history.add(tr);
	}

	public sequence<transaction> get_transactions_history()
	{
		ret transactions_history; // An immutable view can always be returned
	}

	public float get_balance()
	{
		var float balance = 0;

		for_each transaction tr in transactions_history
		{
			balance += tr.get_balance_diff(this);
		}

		ret balance;
	}

}

public class transaction
{
	shared bank_account source;
	shared bank_account destination;

	float amount;

	string<255> comment;

	public constructor(bank_account source, bank_account destination, float amount, own string<255> comment) {
		this.source = source;
		this.destination = destination;
		this.amount = amount;
		this.comment = give comment;
	}

	public float get_balance_diff(bank_account for_account)
	{
		if(for_account == source)
		{
			ret -amount;
		}
		if(for_account == destination)
		{
			ret amount;
		}
		ret 0;
	}

}


public class bank mut
{
	string name;
	mut:sharing_view_sequence<bank_account> accounts = ();

	public constructor(own string name)
	{
		this.name = give name;
	}

	public string get_name()
	{
		ret name;
	}

	public void add(shared bank_account ba) mut
	{
		accounts.add(ba);
	}

}

```

### 3. Licence Plate

```
pkg main;

public class license_plate
{
	const nat LENGTH = 7;
	const array<char> VALID_CHARS = ('A','B','C'); // To be expanded

	array<char,LENGTH> chars;

	public constructor(array<char,LENGTH> chars) throws invalid_char, invalid_length
	{
		mut:array<char> validated_chars = array<char>(LENGTH, ' ');

		for_each char c in chars
		{
			if(!is_valid(c))
			{
				throw invalid_char(c);
			}
			else
			{
				validated_chars.add(c);
			}
		}

		chars = validated_chars;
	}

	static bool is_valid(char check_char)
	{
		for_each char c in VALID_CHARS
		{
			if(c == check_char)
			{
				ret true;
			}
		}

		ret false;
	}

}
```

[→ Next: Name Inference](./name_inference.md)