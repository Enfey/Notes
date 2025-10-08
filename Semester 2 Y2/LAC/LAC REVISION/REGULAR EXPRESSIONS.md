$L \Subset \Sigma^*$, for regular languages, use automata, which can describe infinite language, and give mechanical way to determine language membership for a given $w$. Not usually immediate, via automaton, what language of it is, and given high-level description of lang, often not obvious if it is possible to describe language using finite automaton.

Regular expressions, concise, direct way to describe languages, and can be converted into finite automaton mechanically(so describe same class of language.)

# Regular expression definition
Given alphabet $\Sigma$, syntax of regular expressions over $\Sigma$ is defined inductively as follows: 
1. $\emptyset$ **is a regular expression** - does not accept any input
2. $\epsilon$ is a regular expression - represents empty string/string of length 0
3. $x \in \Sigma,\ x$ is a **regular expression** - matches that symbol
4. If $E$ and $F$ are **regular expressions**, then $E+F$ is a regular expression - string matched if it is matched by either $E$ or $F$, CORRESPONDS TO UNION.
5. If $E$ and $F$ are **regular expressions**, then EF is a regular expression, CORRESPONDS TO CONCATENATION
	L(EF) = $\{xy | x \in L(E), y \in L(F)\}$ SURE!
6. If $E$ is a **regular expression**, then $E^*$ is a regular expression, matching 0 or more occurrences of $E$.
7. If $E$ is a regular expression, then $(E)$ is a regular expression


EXAMPLES:
- ϵ = 
- hallo = hallo
- hallo + hello = hallohello
- h(a + e)llo = hallo, hello
- $a^*b^∗$ = ab, aab, aa, bb, aabb

# The meaning of regular expressions
LANGUAGE CONCATENATION

$$MN = \{uv \ | \ u \in M \wedge v \in N\}$$
KLEENE STAR ON LANGUAGES.
$$L^* = \bigcup_{n=0}^{\infty} L^n$$
To assign regex over $\Sigma$, we assign language $L(E) \Subset \Sigma^*$ as its meaning. Regex signifies a language representation, essentially. 
Define 7 definitions of language representations over the 7 possible regular expressions.

1. L($\emptyset$) - $\emptyset$
2. L($\epsilon$) = {$\epsilon$}
3. L($x$) = $x$
4. L(E+F) = L(E) $\cup$ L(F) - **union**
5. L(EF) = L(E)L(F) - **concatenation**
6. L(E*) = L(E)*
7. L((E)) = L(E)

Can now analyse language denoted in each case of regular expressions:
EXAMPLES:
- ϵ = 
- hallo = hallo
- hallo + hello = hallohello
- h(a + e)llo = hallo, hello
- $a^*b^∗$ = ab, aab, aa, bb, aabb


-  L(ϵ) = {ϵ} via 2
- L(hallo)
	- L(h) = h, via 3
	- L(a) = a, via 3
		- Hence by 5
			- L(hallo) = L(h)L(a)L(l)L(l)(o)
				- $\{uv | u \in L \wedge v \in L'\}$
					- L and L' refer to the sequence of languages seen above.
- $L(hallo+hello) = \{hallo \cup hello\} = \{hallo, hello\}$, via 4
	- No point computing L(hallo), obvious via 3 and 5.
- $L(h(a+e)llo) = {a \cup e} = {a, e}, via 4$
	- $\{uvw | u \in L(h) \wedge v \in L(a+e) \wedge w \in L(llo)\}$
	- COMPUTE EACH SUB LANGUAGE, THEN DO CONCATENATION OR WHATEVER
- $L(a^*b^*)$ 
	- Compute each sublanguage:
		- L(a*) = L(a)* = {a}*
		- L(b*) = L(b)* = {b}*
			- $\{uv \ | \ u ∈ L(a^∗ ) ∧ v ∈ L(b^∗ )\}$
			- $\{uv \ | \ u ∈ \{a^m \ | \ m ∈ N\} ∧ v ∈ \{b^n \ | \ n ∈ N\}\}$
			- $\{a^nb^m \ | \ m,n \in N\}$
		- DOing it like this, purely semantic.
		- Lang where any number of a's, incl 0, appear before b. 
- $(\epsilon+b)(ab)^*(\epsilon+a)$
	- $L(\epsilon+b)$ = $\{\epsilon\} \cup \{b\}$ = ${\epsilon, b}$
	- L(ab*) = L(ab)* = $\{u \ | \ u \in \{ab^m | m \in N\}\}$ 
	- $L(\epsilon+a)$ = $\{\epsilon\} \cup \{a\}$ = ${\epsilon, a}$
		- L($(\epsilon+b)(ab)^*(\epsilon+a)$) = 
			- $\{uvw \ | \ u \in \{\epsilon, b\}, \wedge v \in {ab^m | m \in N\} \wedge w \in \{\epsilon, a\} }$ 
	- JUST LOOK AT THIS, UR THE GOAT




# Algebraic Laws
The meaning of regular expressions allows us to prove useful laws about regular expressions in general. Prove following the distributive law, = is semantic, saying denote the same language. 

$$E(F+G) = EF + EG$$
IDK



# Translating regular expressions into NFAs

1. $N(\emptyset)$ 
	Rejects everything
	Thus $L(N(\emptyset))$ = $\emptyset$ =$L(\emptyset)$
	![[Pasted image 20250303044417.png]]
2. $N(\epsilon)$
	Accepts empty word, 1 final state, rejects everything else. 
	$L(N(\epsilon)) = {\epsilon} = L(\epsilon)$
	![[Pasted image 20250303044427.png]]
3. $N(x)$
	This automaton only accepts the word/character $x$.
	Thus $L(N(x)) = {x} = L(x)$
	![[Pasted image 20250303044445.png]]
4. $N(E+F):$
	Assuming already have NFAs for E and F, merge N(E) and N(F) into one. UNION. 
	Thus is either NFA accepts a word, combined NFA accepts this word. 
	Have to ensure the states of constituent NFAs do not get confused, have to use **DISJOINT UNION**. 
		$$ A \sqcup B = \{(0,a) \ | \ a \in A\} \cup \{(1,b) \ | \ b \in B\}$$
		Tags each element of union with an index to show from which set originated to distinctly mark states. TAG WITH 0, OR 1.
		![[Pasted image 20250303045516.png]]
		$Q_{E+F} = Q_E \sqcup Q_F$
		$\delta_{E+F}((0,q),x) = \{(0,q') \ | \ q' \in \delta_E(q,x)\}$
		$\delta_{E+F}((1,q),x) = \{(1,q') \ | \ q' \in \delta_F(q,x)\}$ 
		$S_{E+F} = S_E \sqcup S_F$
		$F_{E+F} = F_E \sqcup F_F$
		  ![[Pasted image 20250303051322.png]]
		  Have 2 separate NFA definitions, just tag everything in them, transition function now called on state and tag, and returns a tagged state, using the transition function already defined for that state in respective NFA.
		  ![[Pasted image 20250303051328.png]]
		  






# Questions
Give regular expressions for the following languages over $\Sigma = \{a,b,c\}$ 
1. Words where all a's occur before any b's
	$a^*c^*a^*b^*c^*$ 
2. Words that do not contain the symbol c.
	(a + b)*
3. Words that contain an odd number of symbols
	((a+b+c)(a+b+c))* (a+b+c)
4. Words that end with the sequence aa
	(a + b + c)* aa
5. All words containing exactly 2 a's
	(b+c)^* a (b+c)^* a(b+c)^*
6. All words that contain at least one b
	(a+c)^* b (a+c)^* 
7. All words where the number of b's is even
	((a+c)^* b (a+c)^* b)* (a+c)*
8. All words that start and end with the same symbol
	((a+b+c)(a+b+c)* ) * ???
9. All words that contain exactly 1 a
	(b+c)* a (b+c)*
10. All words that contain at least 2 b's
	(a+c)^* b (a+c)^* b (a+b+c)^*
11. All words that contain at most 2 c's
	(a+b)* c (a+b)* c (a+b)*
12. All words such that all b's appear before all c's
	(a+b)^*(a+c)^*
13. All words that contain exactly one b and one c, and any number of a's
	((a)* b (a*) c (a*))+((a)* c (a*) b (a*))
14. All words such that the number of a's plus the number of b's is odd
	(c* a  c* a)* (a) + (c* b  c* b)* (b) + (c* a  c* b)* (b) + (c* b  c* a)* (a)
15. Words that contain substring abc at least once
	(a+b+c)* abc (a+b+c)*
16. Words that contain at least one a, but no more than two
	(b+c)* a (b+c)* a + (b+c)* a (b+c)* 
17. Words that start and end with different symbols
	b(a+b+c)* c + b(a+b+c)* a + a(a+b+c)* c + a(a+b+c)* b +
	c(a+b+c)* b + c(a+b+c)* a
18. All words of even length
	((a+b+c)(a+b+c))* + ($\epsilon$) 

