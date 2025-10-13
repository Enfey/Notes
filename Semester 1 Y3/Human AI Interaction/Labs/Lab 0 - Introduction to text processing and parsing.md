
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

### IO

### Errors and exceptions

### Classes and OOP

### Standard library



## NLTK
Finally! Onto the lab! NLTK is a suite of libraries and programs for symbolic and statistical natural language processing. It supports classification, tokenisation, stemming, tagging, parsing and semantic reasoning functionality.


### Accessing text and NLTK resources
NLTK comes with several integrated datasets:
```python
import nltk
nltk.download('gutenberg')
print(nltk.corpus.gutenberg.fileids())
```
NLTK comes with significant resources and a way to download them via a GUI or via the terminal.
```python
import nltk
nltk.download_gui()
```
Lets you pick and choose which corpora to download as well as choose from pre-existing machine learning models. 
Alternatively, can use the `URLLib` library to easily download documents.
```python
from urllib import request
url = "http://example.org"
raw = request.urlopen(url).read().decode('utf8')
print(raw) # html code of the page
```

### Basic Parsing
#### Accessing Data
Need something to parse, can acquire data either from existing text files or from web.

#### Reading online files.
```python
import nltk
from urllib import request
url = "http://www.gutenberg.org/files/84/84-0.txt"
content = request.urlopen(url).read().decode('utf', errors='ignore')
```
We set encoding to `utf-8` (variable 1-4 bytes unicode) as almost every webpage is transmitted as `utf-8`. Import to set errors to ignore that so that entire process does not stop if python encounters one character it cannot parse. Might mean end up with some blank characters in `content`, risk almost always take when dealing with data (obviously rectify this, statistical analysis etc).

#### Reading local files
The `open()`function returns a file object, and is most commonly used with two positional arguments, and one keyword argument. A file object is an object exposing a file-oriented API to an underlying resource. Function signature: `open(filename, mode, encoding=None)`. The mode argument is optional, `r` will be assumed if its omitted.
It is good practice to use the `with` keyword when dealing with file objects. File is properly closed after its suite finishes even if exception is raised, more succinct that `try-finally` blocks too. 
```python
with open('document.txt', 'r', encoding='utf-8') as f:
	for line in f:
		print(sorted(line))
# Can check the file has been automatically closed.
f.closed() 
>>> TRUE
```
Can manually close file object using `f.close()`


#### Working with databases
Python integrates native access to `SQLLite databases`, though also free to use other databases such as `MongoDB`, `PostgreSQL` etc.
```python
import sqlite3
connection = sqlite3.connect('database.db')

cursor = connection.cursor

cursor.execute (’’’CREATE TABLE Corpus 
( documentId text , documentContent text , documentTopic text )’’’) 

cursor.execute ("INSERT INTO Corpus VALUES ( ’ doc1 ’,’This is an example document for the corpus ’, ’example ’)") 
connection.commit () 

connection.close ()
```
A database cursor is a control structure that enables traversal over the records in a database. Cursors facilitate subsequence processing in conjunction with the traversal, such as retrieval, addition, and removal of database records. Akin to iterator, but can also identify rows, execute statements. Essentially just an abstraction..

### The preprocessing pipeline
#### HTML
if downloaded HTML files, can use `BeautifulSoup` to remove the tags and extract the text only. Assuming have the text containing html tags in an `str = htmlString` the code would look like this:
```python
from bs4 import BeautifulSoup
content = BeautifulSoup(htmlString, 'html.parser').get_text()
```

### Tokenisation, stopword removal, and casing normalisation
1. Tokenisation
	This is the process of breaking text into smaller units, called tokens. Similar process to lexing in compiler frontends without the inclusion of lexical categorisation. Allows for more straightforward processing of text further down NLP pipeline e.g., counting word frequencies, parsing, or vectorisation. 
	NLTK provides a tokenisation function to automatically tokenise textual input. 
	`word_tokenize(text, language='english', preserve_line=False)
2. Stopword Removal
	Refers to filtering out common words that do not add much meaning to textual analysis. Reduces noise in data, and allows NLP pipeline to focus on meaningful characteristics in the text that carry the semantic weight. Any group of words may be used as a list of stopwords.
	NLTK provides a predefines `list` of stopwords
	`nltk.download('stopwords')
3. Casing normalisation aka case folding
	Means converting all text to a consistent case, typically lowercase.
	This prevents treating the same words as different tokens, and simplifies comparison and matching and equality of words. 
	Can just use the `lower()` function to make words lowercase.
```python
import nltk
nltk.download('stopwords')

from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize

text = "Goth Trad playing at the White Hotel don't miss out iz right."
text_tokens = word_tokenize(text)
tokens_without_sw = [word.lower() for word in text_tokens if word not in stopwords.words()]
print(tokens_without_sw)
filtered_sentence = (" ").join(tokens_without_sw)
```

#### Stemming v Lemmatisation
"One cat jumps, two cats jump"
Words are modified based on usage, such variations in those words are called **inflections**. Inflections on verbs are called conjugation, and inflections on nouns are called declensions. Some languages have a lot of declensions, such as Russian, while some only have a few like english. Declensions give us extra info about the meaning of a word through its grammatical role, in exchange for a slight bit more complexity. Such inflections are detrimental to the performance of NLP pipelines. It greatly increases the number of potential tokens the pipeline may encounter, and there is the issue of semantic extrapolation given an inflection. 


Two types of methods used to reduce the number of different tokens are **stemming** and **lemmatisation**. They operate differently but achieve similar things: removing inflections from words. 


##### Stemming
The goal of stemming is to recover the stem of a word, which is the common part of all inflections of that word. This is then used to group words with the same root, such as *walking* and *walked* becoming walk, to improve efficiency and accuracy. The stem produced need not be identical to the morphological root of the word e.g., the resulting stem for *arguing* is *argu*; this is acceptable as the goal is just to group words, not create linguistically perfect roots. There are several types of stemming algorithms which differ in respect to performance and accuracy.

A simple stemmer looks up the inflected form in a lookup table, simple fast, handles exceptions. However all inflected forms must be explicitly listed in the table: new or unfamiliar words are not handled, and the table may be large, especially for highly inflected languages like Turkish.

Suffix stripping algorithms, which are rule-based, attempt to find the root form of a word by crudely removing the suffix. Prefix stripping may also be implemented. Doing both is called affix stemming. 

Stochastic algorithms involve using probability to identify the root form of a word. Learn of a table of the root form to inflected form relations to develop a probailistic model.

Either brute-force and exhaustively search, or use implicit reasoning to stem a word.
```python
from nltk.stem.porter import PorterStemmer
from nltk.stem.snowball import SnowballStemmer

p_stemmer = PorterStemmer()
sb_stemmer = SnowballStemmer('english')
sentence = "This is a test sentence, and I am hoping it doesn't get chopped up too much!"

for token in word_tokenize(lower(sentence)):
	print(p_stemmer.stem(token))
	print(sb_stemmer.stem(token))
	print('---')
```
All stemmers not 100% accurate, these use a set of heuristics to guess the most likely stem of a word and as such have different levels of aggressiveness when cutting down inflections. 


##### Lemmatisation
Lemmatisation is the process of grouping inflected forms of word so that they can be analysed as a single item, identified by the word's lemma(dictionary form, morphological root). This is the key difference from stemming, focuses on the morphological root to give more meaningful results. A trivial way to accomplish this is via dictionary lookup. Words well for straightforward inflected forms, but a rule-based system will be needed for cases such as in languages with long compound words. 

This process involves first determining the part of speech(category of words that share similar grammatical properties e.g., noun, verb, adjective, article, conjunction, pronoun) of a word, and applying different normalisation rules for each part of speech. This approach is conditional upon obtaining the correct lexical category of a given word. Identifying the wrong category or being unable to produce the right category limits the added benefit of this approach over crude suffix stripping algorithms. 

```python
from nltk.stem.wordnet import WordNetLemmatizer
from nltk.tokenize import word_tokenize

nltk.download('punkt')
nltk.download('wordnet')

lemmatiser = WordNetLemmatizer()
sentence = "This is a test sentence, and I am hoping it doesn't get chopped up too much!"

for token in word_tokenize(lower(sentence)):
	print(lemmatiser.lemmatize(token))
```
![[Pasted image 20251013020320.png]]
Notice that the lemmatiser does very poorly on verbs. The reason for that is it needs **POS** as a second argument of the lemmatize() function. `lemmatize(word: str, pos: str, n)`. Default assumption is noun. NLTK allows us to determine the part of speech of a token! This requires downloading an additional resource, the `average_perceptron_tagger`. 
```python
from nltk.stem.wordnet import WordNetLemmatiser
from nltk.tokenize import word_tokenize
nltk.download('wordnet')
nltk.download('averaged_perceptron_tagger')
nltk.download('universal_tagset')

lemmatiser = WordNetLemmatizer()
sentence = "This is a test sentence, I hope it doesn't get chopped up too much!"

post = nltk.pos_tag(word_tokenize(lower(sentence)), tagset=
'universal')
```
Yields the following result:
```python
[( ’This ’, ’DET ’) , (’is ’, ’VERB ’) , (’a’, ’DET ’) , (’test ’, ’NOUN ’) , (’sentence ’, ’ NOUN ’) , (’,’, ’.’) , (’and ’, ’CONJ ’) , (’I’, ’PRON ’) , (’am ’, ’VERB ’) , (’hoping ’ , ’VERB ’) , (’it ’, ’PRON ’) , (’does ’, ’VERB ’) , ("n’t", ’ADV ’) , (’get ’, ’VERB ’) , (’chopped ’, ’VERB ’) , (’up ’, ’PRT ’) , (’too ’, ’ADV ’) , (’much ’, ’ADV ’) , (’.’, ’ .’) ]
```
All tokens are now tagged with their parts of speech. However, the tags that the lemmatiser wants are different to the POS tags provided by the POS tagger. We need to map these POS tags to the POS tags `WordNetLemmatizer` takes as input (a for adjectives, n for nouns, v for verbs, r for adverbs etc) and then let the default behaviour happen for the other tags.

```python
posmap = {'ADV' : 'r', 'ADV ': 'r', 'NOUN' : 'n', 'VERB' : 'v'}

for token in post:
	(word, tag) = token
	print(lemmatiser.lemmatize(word, posmap.get(pos, 'n')) 
```

### I/O loop

```python
import nltk

def process(user_input):
	tokens = nltk.word_tokenize(user_input)

def main()
	while True:
		user_input = input("You: ")
		if user_input.lower() == 'exit':
			print("Goodbye")
			break
		processed_input = process(user_input)
		print(f"Bot: you said {processed_input}")
		
if __name__ == "__main__":
	main()
```


