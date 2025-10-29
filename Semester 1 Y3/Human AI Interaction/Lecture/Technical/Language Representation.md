Before representation, raw text undergoes several pre-processing steps to make it consistent and structured. Typical steps include those discussed last lecture.

After pre-processing, we have clean, structured tokens, ready for representation.

Computers however cannot interpret raw text - they need numeric vectors.
A good representation should
- Capture **semantics** - words with similar meaning have similar vectors
- Preserve **syntactic** information - word order, relationships/dependencies
- Handle **context** - meaning changes according to context.

## Bag-of-Words (BoW) Representation
Use a **vector space model** to represent text, such that the vector space has $V$ dimensions, where $V$ is the vocabulary size. A document is represented as vector in that space where $d_i = [w_1, w_2,..., w_v]$ where $w_j$ is the weight of word $j$ in document $i$. ![[Pasted image 20251013214025.png]]

The Bag-of-Words model transforms a text corpus into a **document-term matrix** $X \in R^{D\times V}$ where:
- $D$ is the number of documents
- $V$ is the vocabulary size
- $x_{ij}$ is the frequency of a word $j$ in a document $i$ 
$$
X =
\begin{bmatrix}
x_{11} & x_{12} & \cdots & x_{1V} \\
x_{21} & x_{22} & \cdots & x_{2V} \\
\vdots & \vdots & \ddots & \vdots \\
x_{D1} & x_{D2} & \cdots & x_{DV}
\end{bmatrix}
$$

#### Distance in Vector space
- How far apart two vectors are, low value $\to$ more similar
- Euclidean distance
	Measures the straight-line distance between two vectors in $V$-dimensional space.
	$dist_{Euc}\left( A^{\to}B^{\to} = \sqrt{ \sum_{i=1}^{V}(A_{i}B_{i})  }  \right)$
- Manhattan distance measures absolute difference between word weights.
#### Similarity in Vector Space
- How close/alike two vectors are, high-value $\to$ more similar
- Cosine Similiarity
	Measures the angle between vectors, not their magnitude. Two docs are similar if point in same direction, even if one is longer.
	$cosine_{sim}(A^{\to}B^{\to}) = \frac {A^{\to} \cdot B^{\to}} {|| A^{\to}|| \ \ ||B^{\to}||}$
	Measures from -1 to 1, 1 is identical direction, 0 is no shared terms -1 is opposite direction.

#### Weighting functions
Given a document $d$, each dimension of that vector is computed from the frequency of the corresponding terms in that document. Using raw frequency to weight each term has problems. Large documents end up distant from short documents, even if they are similar. We apply **weighting functions** to avoid this.

The main functions are:
- **Binary weighting** - $weight(t,d) = \Bigg\{ 1 \ \text{if t} \in d, 0 \ otherwise$ 
	![[Pasted image 20251013215912.png]]
- **Log weighting** = $weight(t,d) = log(1+freq(t, d))$
	![[Pasted image 20251013215933.png]]
- **TF-IDF weighting** = $weight(t, d, n, N) = log(1+freq(t,d)) \times log(\frac{N}{n}$ )
	Where:
	 $N$ = number of documents
	 $n$ = number of documents containing that term
	 $d$ = the document
	 $t$ = term
	![[Pasted image 20251013220053.png]]
Now we can represent any document in a fixed-size vector, we can put appropriate weights to each term. Document vectors can be fed into machine learning algorithms e.g., classifiers such as SVMs, logistic regression, clustering algorithms like K-Means.

### One-hot encoding
A one-hot is group of bits among which the legal combinations of values are only those with a single high bit and all the others low. Simplest form of variable length representation. 

$L \times V$ dimensions where $L$ = length of input and $V$ = size of vocabulary

Example:
	$d_1$ "The red cat"
	$d_2$ - "The red dog"
	$d_3$ - "Cat, red cat and dog"
	$V = |\{the, red, cat, dog, and\}|$
	![[Pasted image 20251014001739.png]]

Assumption: all words are equally unrelated, no semantics, and large **sparse matrices.** Issue when reach millions of dimensions. These vectors are orthogonal and do not encode semantics.
## Dense Vector Representations
A dense vector representation is a general term for representing data as a low-dimensional, real-valued vector (almost all elements non-zero). The goal is to encode meaningiful similarity. Similar items have vectors that are close together in this high dimensional vector space.
### Word embeddings
Word embeddings are a specific type of dense vector representation - applied to words in NLP. They represent each word as a dense vector learned from data, such that semantic relationships are captured.

Let $v(x)$ be the embedding vector of word $x$:
**Similarity:** $Similarity(v(dog), v(cat))>Simiarlity(v(dog), v(car))$
**Analogy:** $v(king) - v(man) + v(woman) ≈ v(queen)$

The properties depend on how the embeddings were computed.

The mathematical intuition for word embeddings is via co-occurrence.
By defining a context window of size $n$ for a target word $w_t$, a model can learn which words tend to appear near each other, and construct a co-occurrence matrix $M$. Embedding algorithms can then factorise this into dense vectors.
	Example:
	"The cat sat on the mat."
	With a context window of size 2:
		Context for "sat" = {"The", "cat", "on", "the"}
		Context for "mat" = {"on", "the"}
Although there are numerous ways to compute embeddings other than this, the context window is rather universal. **GloVe** combines co-occurrence matrices with local context window learning.
![[Pasted image 20251014003808.png]]

#### Word2Vec
A shallow neural network that learns word vectors by predicting context (given window size $n$) from target (or vice versa.)

Two architectures:
1. **CBOW** (Continuous Bag of Words)
	Predicts a target word given its context
	$P(w_t|context) = P(w_t|w_{t-2},w_{t-1}, w_{t+1}, w_{t+2})$ 
2. **Skip-Gram**
	Predicts surrounding words given a target word
	$P(context|w_t)$ 
	For a context window of size 2, create training pairs $(target, context)$, given the target word $x$, the word $y$ appears in its context. Simple shallow neural network taking a one-hot vector of target word (relative to?), hidden layer, and output layer softmaxing over all words to predict context.
	Each word has two vector representations
	1. Input Vector
	2. Output Vector
![[Pasted image 20251014004302.png]]

![[Pasted image 20251014004327.png]]

## Representation and similarity
### Components of a Basic Search Engine
Search engine fundamentally consists of
- **Corpus** : Collection of documents represented in shared feature space.
- **Relevance Function** : A function that measures how relevant a document is to a query, returning a score between 0 and 1
	- **Input** = query
	- **Output** = relevance score
- **User interface** : Allows users to enter queries
- **Ranking interface** : Orders documents by decreasing relevance
### Search as similarity
Search can be viewed as a similarity computation between queries and documents.
$$Relevance(query, document) = Similarity(query, document)$$
Various models approximate this similarity:
- **Boolean models** - Exact matching of keywords
- **Fuzzy models** - Partial matches allowed
- **Vector Space Models** - Represent text as high-dimensional vectors; measure cosine similarity
- **Probabilistic models** - Estimate likelihood of relevance.
### Classification as Similarity: The K-Nearest Neighbours classifier.
Given a labelled corpus and a new document $d:$
1. Find the $K$ most *similar* documents to $d$
2. Assign the majority label among these neighbours
Bag-of-Words representations are often used for measuring similarities.
Choosing $K$ is critical to success.
Labels new instances by comparing them to existing examples.  