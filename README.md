# CDictTemplate
This is a dictionary template, it allows you to create and use dicionaries (maps) in C.

## Usage
To create a dict, you have to first define the dict type in the global space with:

<code>import_dict(\<key-type\>, \<val-type\>, \<dict-type-name\>);</code>

For example, <code>import_dict(int, const char *, int_str_dict);</code>.

Aftwerwards, you can treat <code>\<dict-type-name\></code> as a dict type and do the normal dict operations:

```c
int_str_dict mydict = new(mydict); // Create new dictionary.
dict_set(mydict, 3, "three"); // Add 3 -> three to dict.
dict_set(mydict, 8, "eight"); // Add 8 -> eight to dict.
const char *var = dict_remove(mydict, 3); // Remove 3 from dict.
int num = dict_size(mydict); // Get number of elements in the dictionary.
dict_set(mydict, 8, "seven + one"); // Set mydict[8] to "seven + one".
dict_set(mydict, 0, "zero"); // Add 0 -> zero to dict.
int exist = dict_exists(mydict, 3); // Check if 3 exists in the dict keys.
dict_enum(mydict, enum_proc); // Enum dictionary with an enum callback.
dict_clear(mydict); // Remove all elements in the dict.
dict_free(mydict); // Free the dict (removes all elements automatically).
```

You can define dicts with all types - integers, strings, pointers and structures. Integer and string example:

```c
#include <stdio.h>
#include "dict.h"

import_dict(int, int, num_dict); // Dict<int, int>
import_dict(int, const char *, int_str_dict); // Dict<int, string>

int int_int_enum_proc(num_dict dict, int key, int val)
{
	printf("%d -> %d\n", key, val);
	return 0;
}

int int_str_enum_proc(int_str_dict dict, int key, const char * val)
{
	printf("%d -> %s\n", key, val);
	return 0;
}

int main(int argc, char **argv)
{
	num_dict dict = new(num_dict);
	dict_set(dict, 0, 0x80);
	dict_set(dict, 1, 0x81);
	dict_set(dict, 2, 0x82);
	dict_set(dict, 3, 0x83);
	dict_enum(dict, int_int_enum_proc);
	dict_free(dict);

	int_str_dict dict2 = new(int_str_dict);
	dict_set(dict2, 1, "Pig");
	dict_set(dict2, 4, "Tiger");
	dict_set(dict2, -87, "Rabbit");
	dict_set(dict2, 99, "Human");

	printf("Number 4 is %s.\n", dict_get(dict2, 4));
	dict_enum(dict2, int_str_enum_proc);
	printf("Removing 4...\n");
	dict_remove(dict2, 4);
	dict_enum(dict2, int_str_enum_proc);

	return 0;
}
```

## Limitations
* While dicts can be defined in headers, doing so may result in those dicts being defined multiple times (once by each C file). This will not cause syntax errors (as all functions generated by <code>dict.h</code> are static), but can result in huge executables.
