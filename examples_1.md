[← Go back](./intro.md#10-examples-1)


# Neoncode Examples


## 1. Mutable Person Class

```

pkg examples::mutable_person_class;

import std::time::date;

public class person mut
{
	date birthdate; // Assuming that a person cannot change their birthdate.
	var string name; // Assuming that a person may change their name.

	public constructor(date init_birthdate, own string init_name)
	{
		date = init_birthdate;
		name = give init_name;
	}

	public date get_birthdate()
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

pkg examples::bank_account;

import std::collections::sequence;

public class bank_account mut
{
	mut:sequence<transaction> transactions_history;

	public constructor(bank init_keeper)
	{
		keeper = init_keeper;
		transactions_history = sequence::of<transaction>();
	}

	public void perform_transaction(own transaction tr) mut
	{
		transactions_history.add(give tr);
	}

	public sequence<transaction> get_transactions_history()
	{
		ret transactions_history; // An immutable view can always be returned
	}

	public real get_balance()
	{
		var real balance = 0;

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

	real amount;

	string comment;

	public constructor(bank_account source, bank_account destination, real amount, own string comment) {
		this.source = source; // "this" is a reference to the object self. It can is used to make sure the field is assigned to, not the parameter.
		this.destination = destination;
		this.amount = amount;
		this.comment = give comment;
	}

	public real get_balance_diff(bank_account other)
	{
		if(other == source)
		{
			ret -amount;
		}

		if(other == destination)
		{
			ret amount;
		}

		ret 0;
	}

}


```


### 3. Licence Plate

```

pkg examples::license_plate;

public class license_plate
{
	nat LENGTH = 7;
	array<char> VALID_CHARS = array::of('A','B','C'); // To be expanded

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

	bool is_valid(char check_char)
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