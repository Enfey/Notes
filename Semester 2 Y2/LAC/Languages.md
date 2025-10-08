### Language
> A language is a set of words.

### Word
> Sequence, or string, of symbols.

ϵ denotes the empty word, i.e., a sequence of length 0.

### Symbol
Anything that is a member of an alphabet $\Sigma$

### Language definition
Mathematically:
Given an alphabet $\Sigma$ we define the set $\Sigma*$ as the set of all possible words over $\Sigma$. The empty word ϵ ∈ $\Sigma*$ and given a symbol x ∈ $\Sigma$ and a word w ∈ $\Sigma*$ we can form a new word xw ∈ $\Sigma*$.
With $\Sigma$ = {0, 1}, typical elements of $\Sigma*$ are 0010, 00000000,ϵ
$\Sigma*$ by definition is nonempty given the empty word. For any non empty alphabet, $\Sigma*$ is an infinite set, from which we can generate a new element whenever one is needed, but its elements are finite. 
So the formal definition of a language is: L ⊆ $\Sigma*$

### Concatenation of words
An important operation on $\Sigma*$ is concatenation. Given u, v ∈ $\Sigma*$ we can construct a new word uv ∈ $\Sigma*$ simply by concatenating the two words. This is defined by primitive recursion. 
$$ϵv = v$$
 $$(xu)v = x(uv)$$Properties of concatenation:
 - **Exponetiation** - denotes concatenation of a word with itself e.g., $u^2$ = $uu$. $u^0$ = ϵ, unit
 - **Unit**/**identity** - $ϵu = u = uϵ$
 - **Associativity** - $u(vw)$ = $(uv)w$

### Concatenation of languages
Given languages M and N, their concatenation MN is a new language which is the set of all possible concatenations of words from M and N.

$$MN = \{uv | u \in M \wedge v \in N\}$$
E.g., 
	$M$ = $\{ϵ,a,aa\}$
	$N$ = $\{b,c\}$
	$MN = \{b, c, ab, ac, aab, aac\}$

Properties of language concatenation: 
- **Associativity** - $L(MN)$ = $(LM)N $
- **Zero language** - $L\emptyset=\emptyset=\emptyset$L = FUCKING EMPTY LAD
- **Unit language** - $L\{ϵ\} = L = \{ϵ\}L$
- **Distributivity over Union**
	$L(M \cup N) = LM \cup LN$
	$(L \cup M) N = LN \cup MN$
- **Non-distributive over Intersection**
	$L\{ϵ, a\}, M=\{ϵ\}, N = \{a\}$
	$L(M\cap N) = L\emptyset$
	$LM \cap LN = \{ϵ, a\} \cap \{a, aa\} = \{a\}$
### Exponent notation and kleene star
Exponent notation is used to denote iterated language concatenation:
$L^1 = L, L^2 = LL, L^0 = \{ϵ\}$

Kleene star: represents the set of all possible concatenations of words from L, including the empty word. Formally: 

$$L^* = \bigcup_{n=0}^{\infty} L^n$$
$ϵ ∈ L^∗$ for any language $L$, including L = $\emptyset$

Difference between $\Sigma^*$ and $L^*$ is that while the result is a set of words, the types of the arguments differ - one argument is initially a set of words which is a subset of $\Sigma^*$, $L \Subset \Sigma^*$ , while the other is a set of symbols.

