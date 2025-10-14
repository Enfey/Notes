One of most basic forms of artificial intelligence that people interact with on daily basis are search engines.
Lab objective - **build search engine.** 

## Prerequisites
`pip install numpy`
`pip install scipy`
Then, in python script import:
```python
import numpy
import scipy
```

Python has handy set of functions to save and load objects via  and a newer way of doings things with **joblib**, either works.
```python
from joblib import dump, load
dump(someobject, 'filename.joblib')
loaded_object = load('filename.joblib')
```
Important to know building an application that makes use of computationally processes, such as a machine learning model or an index, saves recomputation!

## Bag of Words
### Building a bag-of-words-model
Let us assume that documents are multi-sets of $n-grams$ . Let us take for example the first sentence of Mary Shelley's Frankenstein. 
$$\text{You will rejoice to hear that no disaster has accompanied the commencement of an enterprise which you have regarded with such evil forebodings.}$$

After tokenising and removing stop words, resulting set of unigram tokens is:
`[’you ’, ’rejoice ’, ’hear ’, ’disaster ’, ’ accompanied ’, ’ commencement ’, ’enterprise ’, ’regarded ’, ’evil ’, ’ forebodings ’]`

After removing punctuation and stemming each term, the **BoW** representation would be a list of those tokens with their frequency in the document
![[Pasted image 20251014010712.png]]

Look at another sentence from same novel:
$$\text{Six years have passed since I resolved on my present undertaking. I can, even now, remember the hour from which I dedicated myself to this great enterprise.}$$ 
Can stem these tokens, add to bag of words model computed for first passage: 
![[Pasted image 20251014010838.png]]
See that this matrix is very sparse - but representing documents as vectors allows us to have a common basis on which to compare them. 
Words used to build **BoW** model = $V$ = $Vocabulary$

### Term weighting
**BoW** model extremely powerful, and barring deep learning methods, the standard way of analysing textual documents. Runs into issue encoding documents of different lengths with regard to the weightings of each vector. Longer documents are skewed with regard to their weighting in accordance with their size. Leads to error when computing the similarity between documents.

Term-weighting methods come into play here to squash weights.

#### Binary weighting
Each term $t$ is either present or absent from document $d$. Puts large and small documents on same footing. 
$weight(t,d) = \Bigg\{ 1 \ \text{if t} \in d, 0 \ otherwise$ 
#### Log-frequency weighting
Each term $t$ is weighted by a function of the raw frequency in the document $d$. This solves the main drawback of frequency weighting by log-scaling term frequencies, which means that even a very frequent term will never be more than a few times as important as a less frequent one.
$weight(t,d) = log(1+freq(t, d))$

#### TF-IDF 
Each term is weighted using its log frequency multiplied by the inverse document frequency of that term.
$weight(t, d, n, N) = log(1+freq(t,d)) \times log(\frac{N}{n}$ )
	Where:
	 $N$ = number of documents
	 $n$ = number of documents containing that term
	 $d$ = the document
	 $t$ = term
The inverse document frequency is a handy way to ensure that a term that is present in almost every single document will have a low weight.

## Pre-processing data
```python
from urllib import request

doc_urls = {
	"Russia": "http://www.gutenberg.org/cache/epub/13437/pg13437.txt",
	"France": 
"http :// www.gutenberg.org /cache/epub/10577/ pg10577.txt ", 
	"England": 
"http://www.gutenberg.org/cache/epub/10135/pg10135 .txt",
	"USA": "http://www.gutenberg.org/cache/epub/10947/pg10947.txt",
	"Spain": "http://www.gutenberg.org/cache/epub/9987/pg9987.txt",
	"Scandinavia": "http://www.gutenberg.org/cache/epub/5336/pg5336.txt ",
	"Iceland":
"http://www.gutenberg.org/cache/epub/5603/pg5603.txt"
}

documents = {}

for country in doc_urls.keys():
	content = request.urlopen(doc_urls.get(country)).read.decode('utf8', errors='ignore')
	documents[country] = content
```

Scikit-Learn gives us access to its own functions for tokenising, filtering etc. Could work around them using the NLTK functions as they achieve the same things, but Sci-Kit Learn is easier to work with often.

Simplest function `CountVectorizer` which takes care of tokenising, filtering, and counting terms in documents.


```python
from urllib import request
from nltk.corpus import stopwords
from sklearn.feature_extraction.text import CountVectorizer

doc_urls = {
	"Russia": "http://www.gutenberg.org/cache/epub/13437/pg13437.txt",
	"France": 
"http :// www.gutenberg.org /cache/epub/10577/ pg10577.txt ", 
	"England": 
"http://www.gutenberg.org/cache/epub/10135/pg10135 .txt",
	"USA": "http://www.gutenberg.org/cache/epub/10947/pg10947.txt",
	"Spain": "http://www.gutenberg.org/cache/epub/9987/pg9987.txt",
	"Scandinavia": "http://www.gutenberg.org/cache/epub/5336/pg5336.txt ",
	"Iceland":
"http://www.gutenberg.org/cache/epub/5603/pg5603.txt"
}

documents = {}

for country in doc_urls.keys():
	content = request.urlopen(doc_urls.get(country)).read.decode('utf8', errors='ignore')
	documents[country] = content
	
all_text = documents.values()
count_vect = CountVectorizer(stopwords = stopwords.words('english'))

X_train_counts = count_vect.fit.transform(all_text)

print(X_train_counts.shape)
```
Returns a term-document matrix. Returns `(7, 24128)`. 24128 terms over 7 documents, approx 24k dimensional space.

The `CountVectorizer` can be instantiated with parameters to modify the vocabulary:
- `max_df`: maximum document frequency. Words which appear in more than `max_df` documents will not be considered. Can also provide floating point between 0 and 1, in which case filters based on percentage rather than raw number.
- `min_df`: minimum document frequency. Words which appear in fewer than `min_df` documents will not be considered. Can also provide float here.
- `max_features`: maximum number of terms in the vocabulary (cuts words with lowest frequency)
- `vocabulary`: if want to pre-specify the vocabulary want to use
- `binary`: treats frequency of terms in term-document matrix as binary.
If wanted to remove the top 10% of documents in terms of document frequency as well as all terms present in fewer than 5 documents, could do so by using the following command: 

`count_vect = CountVectorizer (stop_words = stopwords.words ('english'), min_df =5, max_df =0.9)`

Finally the ngram_range parameter lets you change the tokenisation to an