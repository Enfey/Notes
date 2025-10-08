
# Language
A language is a set of words

## Word
A word is a sequence of symbols, where a symbol can be anything, and $\epsilon$ denotes the empty word, with length 0.#

### Symbol
Anything that is a member of a defined set, the alphabet $\Sigma$ 


### Lang Definition
Given alphabet $\Sigma$, define set $\Sigma^*$ as the set of all possible words over $\Sigma$. Given $\epsilon \in \Sigma^*$ and given $x \in \Sigma$ and a word $w \in \Sigma^*$, we can form a new word, xw. Kleene star over language, infinite set, can generate elements whenever needed, and nonempty given the empty word. So lang $L \in \Sigma^*$ 


### Concatentation of words
Given u, v ∈ $\Sigma*$ can construct a new word uv by concatenating the two words.
Defined via recursion
$$ϵv = v$$

 $$(xu)v = x(uv)$$
 Keep taking first symbol of x, append it to front, until empty word, then place v. 
 Empty word is identity, this is associative, can exponent a word $v$ with itself $v^2$ denotes $vv$ 


### Concatenation of languages
Given languages M and N, their concatenation MN is a new language which is the set of all possible concatenations of words from M and N.
$$MN = \{uv | u \in M \wedge v \in N\}$$
E.g., 
	$M$ = $\{ϵ,a,aa\}$
	$N$ = $\{b,c\}$
	$MN = \{b, c, ab, ac, aab, aac\}$

#### Properties of language concatenation
- Associativity - L(MN) = (LM)N
- Zero language = L$\emptyset$ = $\emptyset$ and vice versa
- Unit languag = $L\{\epsilon\}$ = $L$ and vice versa
- Distributivity over union
	- $L(M \cup N)$  = $LM \cup LN$
	- $(L \cup M) N$  = $LN \cup MN$
		- UNION WORKS LIKE MULTIPLICATION
- Non-distributive over intersection 
	- IDK


### Exponent notation and kleene star
Exponent notation is used to denote iterated language concatenation, ie all concatenations of words in language, with itself, $L^0 = \{\epsilon\}, L^1 = L, L^2 = LL$ 

Kleene star: represents set of all possible concatenations of words from L, including the empty word. 

Difference between $\Sigma^*$ and $L^*$, language concatenation results in set of words which are a subset of kleene star over alphabet, but kleene star over alphabet yields the infinite set of **all** possible words e.g.,

$\Sigma$ = {a,b,c}
$\Sigma^*$ = {\epsilon, a, b, c, aa, bb, cc, ab, ac, ba, bc, ca, cb...}
L $\Subset$ $\Sigma^*$ 
L = {a, b, c, aa}
L = {a, b, c, aa}
L^2 = {aa, ab, ac, aaa, bb, bc, cc, caa, aab, aac, aaaa }, just equals  L^1L^1 
$L^*$ = infinite language concatenation, imagine this simply extending, so does this further.


# Questions
Let the alphabet Σ = {3, 5, 7, 9}, and let the language L = {w | w ∈ Σ ∗ , 1 ≤ |w| ≤ 2}. (If w is a word, |w| denotes the length of that word. If X is a finite set, like an alphabet or finite language, |X| denotes the number of elements in that set, its cardinality.) Answer the following questions:

1. Describe L in plain english
	The language L is a set of words such that the words recognised are a subset of the kleene star over the alphabet, where the length of the word is at least 1, and at most 2, and thus does not recognise the empty word with length 0, and does not recognise sequences of symbols with length exceeding 2, but accepts everyhing else. 
2. L = {3,5,7,9,33,35,37,39,53,55,57,59,73,75,77,79,93,95,97,99}
3. In general, for an arbitrary alphabet Σ1 and 0 ≤ m ≤ n, how many words are there in the language L1 = {w | w ∈ Σ ∗ 1 , m ≤ |w| ≤ n}? That is, write down an expression for |L1|.
	Length of alphabet = k
	| w |
	$m$ $\le w \le n$ 
	For each word length $i, \  w_i$ The number of possible words of length $i$ over the alphabet is $k^i$, as each character can independently be one of $k$ choices.
		![[Pasted image 20250520150239.png]]
		
4. Σ = {a, b, c} and let L1 = {ϵ, b, ac} and L2 = {a, b, ca}, enumerate the following
	41. L_3 = $L1 \cup L2$ = {a, b, ca, ba, bb, bca, aca, acb, acca}
	42. L_4 = $L1{\epsilon}(L2 \cap L1)$ = {b}
	43. L_5 = $\emptyset$ 
5. Let the alphabet Σ = {a, b, c} and let L1 = {ϵ, b, bb} and L2 = {a, ab, abc} be two languages over Σ. Enumerate the words in the following languages:
	1. $L3 = L1 \cap L2$  = $\emptyset$ 
	2. $(L2\{\epsilon\}L1)\cap \Sigma^*$ = $\{a, ab, abb, ab, abbb, abc, abcb, abcbb\}$ 
	3. $L5 = L3\emptyset \cap L4$ = $\emptyset$ 
6. Σ = {a, b, c}. Enumerate the words in L = {w | w ∈ {ϵ, a, b, bc}^∗ , |w| ≤ 3}
	L = {\epsilon, a, b, bc, aa, ab, abc, ba, bb, bbc, bca, bcb, aaa, bbb, aba, abb, bba, bab, aab }


