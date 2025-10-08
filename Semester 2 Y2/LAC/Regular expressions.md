To recaptilate, given an alphabet $\Sigma$, a language is a set of words $L \Subset \Sigma^*$, described languages so far using set theory (comprehensions or explicit enumeration) and through finite automata, which can describe infinite languages and directly give mechanicaly way to determine language membership. From an automaton, not usually immediately obvious what language of it is, and given a high-level description of language, often not obvious if it is possible to describe language using finite automation. 

Regular expressions are concise and direct way to describe languages, and can be mechanically translated into a finite automaton, and vice versa. Being interconvertible, meaning they describe the same class of language.
## Regular expression definition
Given alphabet $\Sigma$, the syntax of regular expressions over $\Sigma$ is defined inductively as follows:
1. **$\emptyset$ is a regular expression** - it a regular expression that does not accept any input
2. **$\epsilon$ is a regular expression** - it a regular expression representing the empty string/string of length 0
3. $x \in \Sigma$, $x$ is a regular expression** - matches just that character.
4. If E and F are regular expressions, then E+F is a regular expression - a string matched if it is matched by either E or F, corresponds to UNION.
5. If E and F are regular expressions, then EF is a regular expression - CONCATENATION ![[Pasted image 20250302214927.png]]
6. **If E is a regular expression, then E* is a regular expression** - matches 0 or more occurrences of E.
7. **IF E is a regular expression, then (E) is a regular expression** 
Kleene star binds stronger than juxtapositioning/concatenation and (+.) E.g., ab* is read as a(b*)
Juxtapositioning/concatenation binds stronger than (+). ab + cd read as (ab) + (cd).

Some examples of regular expressions:
- ϵ  
- hallo
- hallo + hello
- h(a + e)llo 
- $a^*b^∗$ 
- (ϵ + b)(ab) ∗ (ϵ + a)
### The meaning of regular expressions
Language concatenation:
$$MN = \{uv | u \in M \wedge v \in N\}$$
Kleene star operation:
$$L^* = \bigcup_{n=0}^{\infty} L^n$$
To each regular expression E over $\Sigma$ we assign a language L(E) $\Subset$ $\Sigma^*$ as its meaning. This is what the regular expression signifies, a language representation.

1. L($\emptyset$) - $\emptyset$
2. L($\epsilon$) = {$\epsilon$}
3. L($x$) = $x$
4. L(E+F) = L(E) $\cup$ L(F) - union
5. L(EF) = L(E)L(F) - concatenation
6. L(E*) = L(E)*
7. L((E)) = L(E)

Seeing the examples from before, we can now analyse the language denoted in each case.
- L(ϵ) = {ϵ} via 2
- ![[Pasted image 20250302220815.png]]
- L(hallo+hello) = {hallo $\cup$ hello} = {hallo, hello}
- L(h(a+e)llo) = {a $\cup$ e} = {a,e}
	- L(h(a+e)llo) = L(h)L(a+e)L(llo)
		- $\{uvw | u ∈ L(h) ∧ v ∈ L(a + e) ∧ w ∈ L(llo)\}$
		- $\{uvw | u ∈ \{h\} ∧ v ∈ \{a, e\} ∧ w ∈ \{llo\}\}$
		- $\{hallo, hello\}$
	- Obtained via 4 and 5.
- $a^*b^*$
	- $L(a^*) = L(a)^*$ = $\{a\}^*$ = ![[Pasted image 20250302222044.png]]
	- $L(b^*) = L(b)^* = \{b\}^*$ = above
	- $L(a)^*L(b)^*$
		- $\{uv \ | \ u ∈ L(a^∗ ) ∧ v ∈ L(b^∗ )\}$
		- $\{uv \ | \ u ∈ \{a^m \ | \ m ∈ N\} ∧ v ∈ \{b^n \ | \ n ∈ N\}\}$
		- $\{a^nb^m \ | \ m,n \in N\}$
	This language represents all strings where any number of aaa's (including zero) appear before any number of bbb's (including zero). However, **no bbb's appear before aaa's**
	This is because of the concatenation, which denotes ordering.
- ![[Pasted image 20250302222653.png]]

### Algebraic laws
The meaning of regular expressions allows us not only to find out the meaning of specific regular expressions, but lets us prove useful laws about regular expressions in general. Let us prove the following distributive law, where the = is semantic, saying say these denote the same language.
$$E(F+G) = EF + EG$$
We start with L(E(F+G)) and show this is equal to L(EF + EG).
L(E(F+G))
= L(E)L(F+G) (via r.e. concat semantics)
= L(E)(L(F)$\cup$L(G))
= $\{w_1,w_2 \ | \ w_1 \in L(E) \land \  w_2 \in (L(F) \cup L(G))\}$
= $\{w_1,w_2 \ | \ w_1 \in L(E) \land \  \{w_2 \in {L(F) \vee  L(G)}\}$
= ![[Pasted image 20250303043812.png]]
Maybe look at this another time.
Other algebraic laws
.![[Pasted image 20250303043920.png]]
### Translating regular expressions into NFAs 
A regular expression $E$ can be translated into an equivalent NFA N(E) such that $L(N(E)) = L(E)$
We refer to this as the graphical construction. 
The proof is by induction on the syntax of regular expressions:
1. $N(\emptyset)$: 
	![[Pasted image 20250303044417.png]]
	Rejects everything
	Thus $L(N(\emptyset))$ = $\emptyset$ =$L(\emptyset)$
2. $N(\epsilon)$
	![[Pasted image 20250303044427.png]]
	Accepts the empty word, 1 final state, but rejects everything else:
	Thus $L(N(\epsilon)) = {\epsilon} = L(\epsilon)$
3. $N(x)$
	 ![[Pasted image 20250303044445.png]]
	This automaton only accepts the word/character $x$.
	Thus $L(N(x)) = {x} = L(x)$
4. N(E+F):
	![[Pasted image 20250303044554.png]]
	Assuming already have NFAs. Merge diagrams for N(E) and N(F) into one.
	Thus, if either NFA accepts a word, the combined NFA accepts this word. 
	Have to ensure the states of constituent NFAs do not get confused with each other, have to use *disjoint union*.
	$$ A \sqcup B = \{(0,a) \ | \ a \in A\} \cup \{(1,b) \ | \ b \in B\}$$
	Tags each element of union with an index to show from which of two sets it originated, to distinctly mark the elements of the constituent sets. The transition function of the combined NFA also has to be defined to work on the tagged states.
	 ![[Pasted image 20250303045516.png]]
	 where: 
		 $Q_{E+F} = Q_E \sqcup Q_F$
		 $\delta_{E+F}((0,q),x) = \{(0,q') \ | \ q' \in \delta_E(q,x)\}$
		  $\delta_{E+F}((1,q),x) = \{(1,q') \ | \ q' \in \delta_F(q,x)\}$
		  $S_{E+F} = S_E \sqcup S_F$
		  $F_{E+F} = F_E \sqcup F_F$
		  Ensures transitions remain independents, as input and resulting states are tagged. 
		  ![[Pasted image 20250303051322.png]]
		  ![[Pasted image 20250303051328.png]]
	Remains to prove that L(N(E+F)) = L(E+F)
		= L(N(E)) $\cup$ L(N(F))
		= L(E) $\cup$  L(F) assume the translation is correct for the subexpressions
		= L(E+F) via semantic rule 4 of regular expressions
5. N(EF) :
	Recall that L(EF) = L(E)L(F). Thus, a word w $\in L(EF) \ iff \ w$ can be divided into two words u and v, w = uv, such that u $\in$ L(E) and $v \in L(F)$. 
	So if have two NFAs recognising same language as regular expression, can construct an NFA recongising words w = uv $\in L(EF)$ if join the NFAs in sequence in such a way that machine moves from $N(E)$ to an initial state of N(F) on the last symbol of $u \in L(E)$. 

MAYBE DO THESE LATER
