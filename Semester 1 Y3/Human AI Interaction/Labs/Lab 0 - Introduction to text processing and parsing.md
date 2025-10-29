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


