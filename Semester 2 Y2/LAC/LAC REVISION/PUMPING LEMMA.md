# Regular
A language $L$ is said to be regular, if, satisfies following properties
1. There exists DFA or NFA that recognises $L$ and rejects all strings not in $L$ 
2. There exists regular expression which can express $L$
3. The language is closed under several operations
	- Language Union: $L1 \cup L2$
	- Language Concatenation: $L1L2$ 
	- Kleene star, $L^*$
	- Instersection, Difference.
Some languages, cannot be recognised, by finite automaton. They require memory, beyond fixed finite amount to be recognised. These automaton, finite number of states, do not change based on input length, info remembering implicity via states locally, and then forgotten. No state shared, and the amount of states = finite.

$$ L = \{0^n1^n \ | \ n \in N\}$$ Why can this not be recognised? Suppose 32MB of memory = 32 x 1024 x 1024 x 8 = 268435456 bits. DFA with $2^{268435456}$ state. Above must work for arbitrarily large values of n. So if this memory ever exceeded, (where $n \in N$ so it definitely will be) this will not be able to determine if the number of 0s is equal to the number of 1s seen through states. Need an unbounded amount of memory to recognise L. 

Whether a language can be recognised or not is not always obvious at first glance.

However, we do not use the pumping lemma to prove regularity. All regular languags satisfy the pumping lemma, but there are some non-regular languages that also satisfy the pumping lemma, so passing it doesn't prove anything, must build a DFA, NFA or regular expression instead. 

# Pumping Lemma
> *Given a regular language $L$, then there is a number $p \in N$ such that all words $w \in L$, that are larger than $p, (|w| \ge p)$ can be split into 3 words, $w = xyz$ such that
> 	1. $y \neq \epsilon$
> 	2. $|xy| < p$
> 	3. $\forall{k} \in N . xy^kz \in L$

As for all, use a proof by contradiction.
Essentially, b


## Application of pumping lemma
> **Theorem**
> 	*The language $L = \{0^n1^n \ | \ n \in N\}$ is **not** regular

**Proof**
Assume $L$ would be regular. Leads to contradiction.
By using pumping lemma, there is a p such that we can split words larger than p into 3 words such that the properties of the pumping lemma hold.
Consider $0^n1^n$, longer than $p$, $p \in N$, $2p \ge p$
|xy| $\le p$, x and y can only contain zeros, thus $y \neq \epsilon$ also.
xy^kz would no longer hold, do for 0, y^0, because y was at least one zero, now missing 1, or could also add one by doing y^2, and does not hold for all, and thus fails. 



# QUESTIONS
![[Pasted image 20250521133345.png]]
L2, not regular, use pumping lemma to prove this.

Where $\#a, and \ \#b,\in \Sigma^* \to N$ 

p \in N

$L2 =  \{w | \#_a(w) = \#_b(w)\}$ 

Select word $\#_a(w) = \#_b(w)$, this is longer than $p$, 2p $\ge$ p
|xy| $\le p$  = x and y formed entirely of a's, thus $y \neq \epsilon$ 
$\forall{k} \in N, xy^kz \in L$ 
xy^2z where
x = a^m
y = a^n
z = a^p-n-m b^p

xy^2z = a^m a^2n a^p-n-m b^p
Leading to an increase in the number of a's, whilst failing to account for this change in the number of b's. So by raising the exponent, there will be too many a's, at least one a given that $y \neq \epsilon$, and the count of a's will not be equivalent to the count of b's. 



