## Regular
A language $L$ is said to be regular if it satisfies the following properties
1. There exists a DFA or NFA that recognises L, and rejects all strings not in L
2. There exists a regular expression which can express L
3. The language is closed under several operations:
	- Language Union: $L_1 \cup L_2$ 
	- Language Concatenation, $L_1L_2$ 
	- Kleene Star, $L^*$ 
	- Insersection, Difference also closed
Some languages cannot be recognised by finite automata as they require memory beyond a fixed finite amount.
	Ie these automaton have finite number of states that do not change based on input length, and only 'remember' info implictly through its states.
A simple example of a language that cannot be recognised using only finite memory/states is the language $$ L = \{0^n1^n \ | \ n \in N\}$$ Language of words that starts with a number of 0's followed by the **same** number of 1s.

Why exactly can $L$ not be recognised by a computer with finite memory.
Suppose 32MB of memory; that is 32 $\times$ 1024 $\times$ 1024 $\times$ 8 = 268435456 bits. This would correspond to a DFA with $2^{268435456}$ states maximum (e.g., if 3 bits, 2^3 = 8 states max). Because this above must work for arbitrarily large values of N, once an input string exceeds the necessary states, it cannot determine if the number of 0s is equal to the number of 0s seen through states. An unbounded amount of memory is needed to recognise $L$ (in theory).
### Pumping lemma
> *Given a regular language $L$, then there is a number $p \in N$ such that all words $w \in L$ that are longer than p(|w| $\ge$ p) can be split into three words w = xyz s.t.*
> 	1. $y \neq \epsilon$ 
> 	2. $|xy| \le p$ 
> 	3. $\forall k \in N . xy^kz \in L$ 

#### Applying the pumping lemma
> **Theorem**
> 	*The language $L = \{0^n1^n \ | \ n \in N\}$ is **not** regular

**Proof**
Assume L would be regular, show this leads to contradiction. 
By using pumping lemma, there is an n such that we can split each word that is longer than n such that the properties given by the pumping lemma hold. 
Consider $0^n1^n$, this is longer than $p$, $2p \ge$ $p$ 
$xyz$ = $0^n1^n$ 
$|xy| \le p$ , x and y are entirely within the first half given $n \in N \ and \ p \in N$,
and y therefore contains at least one 0. 
$xy^0z \in L$, but this would result in **at least** one fewer 0 than 1s.
$xy^2$z $\in L$, result in at least 2 or more 0s than 1s. 

Contradicts. 



#### Applying the pumping lemma
$\{a^nb^mc^{m+n} \ | \ m, n \in N\}$

**Proof**
PICK ONE WORD/STRING THAT EXCEEDS $p \in N$ and PROVE IT FOR THAT



Consider $a^pb^pc^{2p}$, this is a string longer than $p$, $4p$ $\ge p$, where m = p and n = p, m,n,p $\in$ $N$  

$xyz = a^pb^pc^{2p}$
$|xy| \le p$ so x and y are entirely a's, meaning y must be at least one a, thus $y \neq \epsilon$
$xy^2z \in L$ , but this would result in additional a's at least 1 more, and this would break the regularity given that the count of c's remains the same while the number of a's increases, breaking the balance required by $c^{m+n}$. Thus contradict condition 3 of the pumping lemma.
e.g., would take 
aabbbccccc to aaaabbbccccc.
would need $c^{2p} + n$ 


#### Applying the pumping lemma
$L_2 = \{w \in \Sigma^* \ | \ num_a(w) = 2 \times num_b(w) \wedge num_b(w) = 2 \times num_c(w)\}$ 
Obviously irregular 
E.g., $abcaaba \in L_2$ 

Consider $xyz = num_a(w) = 2 \times num_b(w) \wedge num_b(w) = 2 \times num_c(w)$ where $num_c = p$
Thus $num_a = 4p, num_b = 2p$
$7p \ge p$
$|xy| \le p$, since string starts with a^4p, xy must be some subset of a's, and are entirely a's thus $y \neq \epsilon$ 
$xy^kz \in L$, pick 2. (1 would give back 1 and satisfy)
$xy^2z \in L$, but this would result in additional a's, at least 2 more given that y is at least one a, resulting in a^{4p}a

To sketch the composition of all words w at this point:
	$x = a^k$ 
	$y = a^m$
	$z = a^{4p-k-m}b^{2p}c^{p}$
Thus for k = 0:
$xz = a^ka^{4p-k}b^2pc^p$, thus holds
For k = 1:
$xyz = a^ka^ma^{4p-k-m}b^2pc^p$ , thus holds
For k = 2:
$xy^2z = a^ka^{2m}a^{4p-k-m}b^{2p}c^p$ does not hold, given that this is equivalent to:
	a^{4p+mb^2pc^p} and m > 0, and this string/word does not satisfy this condition as a^{4p+m} and not a^{4p}
















big jar of pickled ginger






