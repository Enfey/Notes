
## Python Basics
### Running python script
`python scriptname.py`
Can also simply type python 3.14 to enter interactive mode.

### Installing packages
Browse PyPi for packages, use pip to install packages with simple install flag
`pip install packagename`
Can then import packages with the import command and alias them with the as command.
Can also import direct objects from the library with the `from...import` command.
`from bs4 import BeautifulSoup as bsoup`


### Numbers
- Typical operators: `+`, `/`, `-`, `+` to perform arithmetic, parentheses for grouping.
- Integers have type `int`, fractional/real numbers have type `float`.
- Division always returns a float, to do floor division use the `//` operator to return an int. `%` = modulus as standard.
- `**` to calculate powers
- Also supports decimal and fraction types, and has built in support for complex numbers

### Text
- Python can manipulate text, type `str`. Can be enclosed in either '' or ""
- Concat with `+`, repeated with `*`
- Strings can be indexed as as expected. No separate character type. String slices are also support.
```python
word[:2]   # character from the beginning to position 2 (excluded)

word[4:]   # characters from position 4 (included) to the end

word[-2:]  # characters from the second-last (included) to the end
```
- Start always included and the end always excluded. Ensures `s[:i] + s[i:]` is always equivalent to `s`.
- Python strings are immutable.
- `len()` returns length of a string.
- A formatted string literal/f-string is a string literal prefixed with `f`. These strings may contain replacement fields which are expressions delimited by curly braces. While other string literals have a constant value, formatted strings are really expressions evaluated at runtime. F strings may be concatenated.
	- Replacement fields can contain any expressions, except for empty expressions. Cna also contain comments.
	- If a conversion is specified, the result of evaluating the expression is converted before formatting. Conversion `'!s'` calls [`str()`](https://docs.python.org/3/library/stdtypes.html#str "str") on the result, `'!r'` calls [`repr()`](https://docs.python.org/3/library/functions.html#repr "repr"), and `'!a'` calls [`ascii()`](https://docs.python.org/3/library/functions.html#ascii "ascii").
- Strings implement all of the common [sequence](https://docs.python.org/3/library/stdtypes.html#typesseq) operations
	- slicing, indexing, `len()`, `in` (infix), `not in` (infix), `concat` (infix), adding string to itself, `min`, `max()`
	- capitalize() - Return copy of the string with first character capitalised.
	- count(`sub, range[start?, end?]`) - return number of non overlapping occurrences of a substring `sub` in the range `start, end`.
	- endswith(`suffix, [start?, end?]`) - Return True if string ends with specified suffix, otherwise return False. Suffix type can be either str or num etc, or tuple of differing types, looks for all suffixes in tuple. Can also specify `start ... end` range.
	- find(`sub, s[start?:end?]`) - returns lowest index in the string where substring `sub` is found within the slice `s[start:end]`. `-1` returned if sub is not found.
	- format() - performs a string formatting operation. The string on which this method is called can contain literal text or replacement fields delimited by braces. Returns a copy of the string where each replacement field is replaced with the string value of the corresponding argument
		- `"The sum of 1 + 2 is {0}".format(1+2)"`
	- index() - like find, but raises an error when substring is not found
	- isalnum()
	- isdigit() 
	- isascii() 
	- islower()
	- isnumeric()
	- lstrip(`chars`) - return a copy of the string with leading characters removed. Can specify the `chars` argument as a set of characters to be removed. All permutations of the values of `char` are stripped.
	- split(`sep?, maxsplits?`) - return list of the words in the string using `sep` as the delimiter string. If maxsplit is given, at most maxsplits are done. If sep not specified or is `None` a different splitting algorithm is used: runs of consectuive whitespace are regarded as a single separator and the result will contain no empty strings.
	- replace()
List goes on.
### Control flow
- `if, elif, else`
- `for`
	- Slightly different to traditional for loops. Define both iteration step and halting condition. Iterate over items of any sequence type in order of sequence. e.g., `for x in sequence`
	- To iterate over sequence whilst modifying that same sequence, can use `copy()` and iterate over that, or iteratively build new, updated sequence.
	- `Range()` - if need to iterate over sequence of numbers, end point is never part of sequence e.g., `range(5) = 0..4`. To iterate over indices of a sequence can combine `range` and `len` e.g., `for i in range(len(s))`
		- The object returned by range is an iterable.
	- `Break` statement to break out of innermost enclosing loop
	- `Continue`, well continues!
	- `For` or `while` loops may be paired with an `else` clause. If loop finished without executing `break`, `else` clause executes. In a `for` loop, else clause executed after the loop finishes final iteration with no `break`. In `while` loop, executed when loop condition becomes `false.`
- `pass` - does nothing, used when statement required syntactically but program requires no action. 
- `match` takes an expression and compares its value to successive patterns given as one or more case blocks. similar to to pattern matching. can combine several literals in a single pattern using `|`. Use `_` as wildcard fallthrough which never fails match.
- `Def()` introduces function definition. Followed by function name and parentheses enclosed list of formal parameters (no type annotation necessary.)
	 - Functions without return value actually return `None` 
	- Can specify default argument values which are then optional for the caller to give. 
		![[Pasted image 20251011222733.png]]
	- Default however is only evaluated once, makes a difference when the default is a mutable object such as a list, dictionary, or instances of most classes. 
		![[Pasted image 20251011222904.png]]
- Functions can also be called using keyword arguments of the form `kwarg = value` 
	![[Pasted image 20251012002616.png]]
	![[Pasted image 20251012002641.png]]
- When a final formal parameter of the form `**` it receives a dictionary. Also parameters of the form `*` receive a tuple, but must precede the `**` parameter if it appears in the function definition. 
	![[Pasted image 20251012002846.png]]
- Due ability to pass arguments by position or by keyword, can optionally restrict the way arguments can be passed such that can look at function definition and immediately determine whether items are passed by position, position or keyword, or by keyword.
	![[Pasted image 20251012003022.png]]

### Data Structures
- Lists
	- ` append(x)` 
		- Add an item to the end of the list. Similar to `a[len(a):] = [x]` 
	- ` extend(iterable)` 
		- Extend the list by appending all the items from the iterable  `a[len(a):] = iterable`
	- ` insert(i, x)` 
		- Insert an item `x` at a given position. The first argument is the index of the element before which to insert, so `a.insert(0, x)` inserts at front of list, and `a.insert(len(a), x) = a.append(x)`
	- ` remove(x)` 
		- Remove first item from the list whose value is equal to `x`, raise error if fail.
	- ` pop([i])` 
		- Remove the item from the given position in the list and return it. If no index specified, removes last item if list. Error if list empty or index out of range.
	- ` clear()` 
		- Removes all items from the list.
	- ` index(x [start?, end?])` 
		- Return index of the first occurrence of `x` in list. Error raised if no such item. Can use slice to limit search to subseqeuence. Returned index still computed relative to the beginning of full sequence however.
	- ` count(x)` 
		- Return number of times `x` appears in the list
	- ` sort(*, key=None, reverse=False)` 
		- Sort the items of the list in place.
	- ` reverse()` 
		- Reverse elements of the list in place.
	- `copy()` 
		- Return shallow copy of the list, similar to `a[:]`
	- `del` 
		- Differs from pop statement, can be used to remove an item given its index instead of its value. Can be used to remove slices from a list or clear the entire list. 
	- Using lists to implement stack:
		- To add item to top of stack, use `append()`, to retrieve item from top of stack, use `pop()` without explicit index.
	- Using lists as queues: DONT
		- Use `collections.deque` which was designed to have fast appends and pops from both ends.
			- appendleft(x) - add x to left side of deque
			- append(x) - add x to right side of deque
			- extend(iterable) - extend right side of deque by appending elements from iterable argument.
			- extendleft(iterable) - extend left side of deque by append elements from iterable.
			- pop() - remove and return element from right side of deque
			- popleft() - remove and return element from left side of deque
	- List Comprehensions
		- Provide concise way to create lists.
		- Consists of brackets containing an expression followed by a `for` clause, then zero or more `for` or `if` clauses. Result is a newlist resulting from evaluating the expression in the context of the `for` and guard clauses which follow it.
			![[Pasted image 20251012004844.png]]
	- Nested list comprehensions
		- The initial expression in a list comprehension can be any arbitrary expression, including another list comprehension. 
		![[Pasted image 20251012005349.png]]

- Tuples and sequences 
	- Sequence types are types allowing you to store multiple values in organised and efficient fashion e.g., strings, bytes, lists, tuples, buffers.
	- A tuple is a sequence type, consists of a number of values of differing type separated by commas.
	- Tuples may contain other tuples as elements
	- Tuples are immutable, but can contain mutable objects.
	- Construction of tuples containing 0 or 1 items has differing syntax.
		- `empty = ()`
		- `singleton = 'hello', #note trailing comma`
	- The statement `t = 12345, 54321, 'hello!'` is an example of tuple packing. Reverse operation i.e., sequence unpacking also possible.
		- `x, y, z = t` - works for any sequence, assuming as many variables on LHS as there are elements in sequence. 
- Sets
	- Unordered collection, no duplicate elements. Supports familar mathematical operations such as union, intersection, difference. Curly braces or the `set()` function can be used to create functions. To create an emptyset, have to use `set()` and not `{};` as the latter creates an empty dictionary. ![[Pasted image 20251012010358.png]]
	- Set comprehensions also supported
		- `a = {x for x in 'abracadabra if x not in 'abc'}`
		- `{'r', 'd'}`
- Dictionaries & Mapping types
	- A dict is a high-level optimised hash table. When you store a KVP, hash the key, according to the type of the key. Takes hash and does `h % table_size`. If slot empty, stores whole KVPthere, if not, probes (pertubation based) for next slot if the stored key's hash does not match the current hash. If the hashes match, compare keys to check equality. Once inserted, python sets the value pointer.
	- Lookup process, compute hash, compute index using `h % table size`, probe until matching key, or empty slot(key not found). 
	- When table too full, resized to next power of two.
	- Keys can be any immutable type, but not lists as these can be modified in place and lead to different hashes being computed.
	- ![[Pasted image 20251012011640.png]]
	- Can also create dictionaries using dict comprehensions
	- Dicts compare equal if and only if they have the same `(key, value)` pairs regardless of ordering.
	- Support number of operations
		- `list(d)` 
			- Return a list of all keys used in dict `d`
		- `len(d)`
			- Return number of items in dict `d`
		- `d[key]`
			- Return the value in `d` associated with `key`
		- `d[key] = value`
			- Set `d[key]` to `value`
		- `del d[key]`
			- Remove `d[key]` from `d`. Raises an error if key not in mapping.
		- `key in d`
			- `True` returned if `d` has key `key`, else `False`
		- `key not in d`
			- No explanation necessary really
		- `iter(d)`
			- Return an iterator over the keys of the dictionary
		- `clear()`
			- Remove all items from the dictionary
		- `copy()`
			- Return a shallow copy of the dictionary
		- `get(key, default=None, /)`
			- Return the value for `key` if `key` is in the dictionary, else default. If default is not give, defaults to `None`.
		- `pop(key, /)`
		- `pop(key, default, /)`
			- If key in dict, remove it and return its value, else return default. Raises error if no default and key not in dict.
		- `popitem()`
			- Remove and return `(key, value)` pair from the dictionary, pairs are returned in `LIFO` order.
		- `reversed(d)`
			- Return reverse iterator over the keys of the dictionary.
		- `items()`
			- Returns a new view of the dictionaries keys. Often used when looping through dictionaries.
- Looping techniques
	- When looping through dictionaries:
		- Key and associated value can be retrieved at the same time using the items() method
			`for k, v in dict.items():
				`dosomething(k, v)`
	- When looping through a sequence the position index and corresponding value can be retrieved at the same time using the enumerate() function. 
		`for i, v in enumerate(['tic', 'tac', 'toe']):
			`dosomething(k, v)`
	- To loop over a sequence in reverse first specify the sequence in a forward direction then call the `reversed` function.
		 `for i in reversed(range(0, 10))`
	- To loop over a sequence in sorted order, use the `sorted()` function which returns a new sorted list while leaving the source unaltered.
		`list = [x, y, z]`
		`for i in sorted(list):
			`dosomething(i)`
	- To loop over two or more sequences at the same time, the entries can be paired with the zip function
		`for x, y in zip(list):`
			`dosomething(x, y)`
	- Using `set()` on a sequence eliminates duplicate elements. The use of `sorted()` in combination with `set()` is often used to loop over unique elements of a sequence in sorted order.
		`for x in sorted(set(list)):
			`dosomething(x, y)`

### Modules

### IO

### Errors and exceptions