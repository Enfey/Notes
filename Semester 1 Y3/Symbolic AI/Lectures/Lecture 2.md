## First-Order Logic
- To formalise knowledge and reasoning/inference, we need a formal language to describe knowledge
- Exist many formal languages to encode knowledge

### Main Characteristics of FOL
- Declarative Language
- Designed to express knowledge
- Precise: each fact can only be true or false (unlike in fuzzy logic)
- Defines which strings are valid sentences(syntax) and what it means for them to be true(semantics)
- Unlike natural languages e.g., English, FOL is completely abstract; it does not have the inherent ability to describe real-world phenomena
	- Uses abstract symbols e.g., x, Dog2 etc
	- The mapping of these symbols to real world phenomena is up the user
		- $Dog(fido) \vee Cat(fido)$ 
		- $Dog(x) \implies Animal(x)$ 
		- $\exists.Dog(x) \wedge Lovely(x)$ #

### Types
Two 'data types'
1. Boolean
2. Domain of discourse(Domain)
Operations such as $\wedge, \vee, \neg$ work with Booleans
Domain elements are used in quantifiers $\forall, exists$ and equality $=$ , can be passed as parameters.


### Alphabet
#### Logical Symbols
- Quantifiers $\forall$ and $\exists$ for domain elements
- Logical connectives $\vee, \wedge, \neg, \implies, \iff$ 
- Equality $=$ to compare domain elements
- Punctuation '(',')' and other brackets for readability: '{','}'
- Variables for domain elements, usually lower case letters, optionally provided with indices, when referring to a specific instance. 
#### Non Logical Symbols
- Predicate Symbols - 'functions' that return booleans; usually begin with capital letters e.g., $P(x)$
- Function Symbols: functions that return domain elements, usually begin with lower-case letters e.g., $f(x)$ 

#### Logical connectives and their precedence
![[Pasted image 20251007180958.png]]

We use the following precedence rules:
- Apply all the functions, predicates and '='
- Apply $\neg$ to as little as possible
	- $\neg A \wedge B$ means $(\neg A) \wedge B$ 
- Then apply the quantifiers to as little as possible
	- $\forall x.A(x) \wedge B(x) \wedge C$ means $(\forall x.A(x) \wedge B(x)) \wedge C$ 
- Then apply $\wedge$ to as little as possible
	- $A \implies B\wedge \neg C \wedge D$ means $A \implies(B \wedge (\neg C) \wedge D)$
- Then apply $\vee$ to as little as possible
- Then apply $\implies$ to as little as possible
- Then apply $\iff$ to as little as possible


## De morgans laws and their generalisations
![[Pasted image 20251007181624.png]]
Laws: Bind each negation to individual terms given negation over a series of terms, and then flip the connective, given either conjunction or disjunction. 

This extends to quantifiers, simply change the quantifier, and bind the negation to the predicate function. 


#### Arity of function and predicate symbols
- Each predicate and function symbol has *arity* i.e., number of parameters
- Arity can be 0, 1, 2
- Predicates with arity 0 are just propositional variables; either true or false
- Functions with arity 0, i.e., nullary functions are called constants, fundamental term that stand for domain elements e.g., Cat, 74 rather than the result of applying a function. 

#### Example of FOL formula
![[Pasted image 20251007182335.png]]

#### Well-Formed Formulas
Not every sequence of alphabet symbols are a valid expression:
	$\neg, \wedge x \forall$ has no meaning
- Valid formulas are called **Well-Formed Formulas**
- **WFFs** are defined inductively via **terms** and **formulas**
	- **Term** is an expression that 'returns' a domain element
	- **Formula** is an expression that 'returns' Boolean

#### Formation rules
- Formation rules inductively define terms and formulas
- A **Term** is either:
	- A variable
	- Any expression in the form of $f(t_1, t_2,...t_n)$ where $t_j$ is a term and $f$ is a function symbol of arity $n$ 
- A formula is any expression in the following forms:
	- $P(t_1, t_2, ... t_n)$ where $t_j$ is a term and $P$ is a predicate symbol of arity $n$ 
	- $\neg \phi$ where $\phi$ is a formula
	- $t_1 = t_2$, where $t_1$ and $t_2$ are terms
	- $\phi \wedge \psi,  \phi \vee \psi, \phi \implies \psi, \phi \iff \psi$  where $\psi$ and $\phi$ are formulas
	- $\forall x.\phi$ and $\exists x.\phi$ where $x$ is a variable and $\phi$ is a formula


#### Variable Scope
- Variables in FOL can be free or bound
	- Bound variables are defined by quantifiers
	- Free variables are defined outside the formula (their values are given)
- The *scope* of a variable is the part of the formula where it can be used. 
- If the name of a variable is reused, it refers to the innermost definition.
- Variables defined by a quantifier are only accessible within the scope of that quantifier
	- ![[Pasted image 20251007183741.png]]

#### Sentences
Sentences are special cases of formulae:
$$\text{A WFF that does not have free variables is called a sentence.}$$
To convert a formula with free variables into a sentence, must substitute values for every free variable in that formula.


#### Semantics
Consider the following sentence
$$ \forall x . PM(x) \to MP(x) \wedge \text{Popular(x)}$$ What does this mean?

- Cannot hope to evaluate this formula: we don't know what the non logical symbols mean and how they behave. 
- Semantics is concerned with the mathematical meaning of sentences - but not the mapping to real world.
- **Logical interpretation** defines the semantics of FOL
	- It specifies how non-logical symbols behave.

#### Interpretation
- Interpretation $\mathcal{J}$ is a pair $\langle \mathcal{D}, \mathcal{I} \rangle$ where
	- $\mathcal{D}$ is a non-empty set called the **domain of discourse** - represents all objects we are talking about in our logical system
	- $\mathcal{I}$ gives a mapping of every non-logical symbol - that is constants, functions, and predicates. 

	$\mathcal{I}$ is defined as follows:
	- For a functional symbol $f$ of arity $n$ 
		- $\mathcal{I}[f] \ : \ \mathcal{D} \times \mathcal{D} \times \mathcal{D} \times ... \times \mathcal{D} \to \mathcal{D}$ 
		- ![[Pasted image 20251007190202.png]]
		- Essentially a function from $n$ domain elements to $1$ domain element.
	- For a predicate symbol of arity $n$ then
		- $\mathcal{I}[P] \ : \ \mathcal{D} \times \mathcal{D} \times \mathcal{D} \times ... \times \mathcal{D} \to {True, False}$ 
		- ![[Pasted image 20251007191019.png]]
		- Tells us true or false for each element in the domain. 

#### Evaluate given interpretation
![[Pasted image 20251007191134.png]]


### Satisfaction
Having interpretation $\mathcal{J}$, we can determine if a sentence $\alpha$ is satisfied by $\mathcal{J}$. 
	$$\mathcal{J} \models \alpha$$
If $\alpha$ is a formula with free variables, then satisifability then depends on the variable assignment to those variables which do not have predefined semantics:$$\mathcal{J}, u \models \alpha$$
Can use crossed out entails to say that a formula is not satisfied i.e., it is false under interpretation $\mathcal{J}$ (and variable assignment $u$ )

Same notation can be used for a set $S$ of sentences/formulae. 


### Rules of interpretation
- Let $P$ be a predicate symbol of arity $n$, $t_i$ be a term, $d_i$  = $\mathcal{J}[t_i]$ or $d_i = u[t_i]$ for free variables, $\alpha$ and $\beta$ be formulas, and $x$ a variable.
- Then the rules of interpretation are as follows:
	- - $\mathfrak{J},\mu \models P(t_1,t_2,\dots,t_{n})\iff \left< d_1,d_2,\dots,d_{n} \right>\in \mathcal{I}[P]$
		- That is, plug in the actual objects for $t_1,...t_n$ and see if that tuple satisfies $P$ in $\mathfrak{J}$.
		- If $P(x,y)$ means x is less than y then
		- $\mathfrak{J}, \mu \models P(2,3)$ is true $\iff$ $(2,3) \in \mathfrak{J}[P]$
	- $\mathfrak{J},\mu \models(t_{1}=t_{2})\iff d_{1}\text{ is the same object as } d_{2}$
		- True if both terms evaluate to the same object
	- $\mathfrak{J},\mu \models ¬\alpha \iff \mathfrak{J},\mu \not\models\alpha$ 
	- $\mathfrak{J},\mu \models(\alpha \wedge \beta)\iff \mathfrak{J},\mu \models\alpha$ or $\mathfrak{J},\mu \models\beta$ 
	- $\mathfrak{J},\mu \models(\alpha \vee\beta) \iff \mathfrak{J},\mu \models\alpha$ or $\mathfrak{J},\mu \models\beta$ 
	- $\mathfrak{J},\mu \models\exists x.\alpha \iff \text{for some }d\in \mathcal{D}\text{ holds }\mathfrak{J},\mu \models\alpha \text{, where }\mu[x]=d$
	- $\mathfrak{J},\mu \models\forall x.\alpha \iff \text{for all }d\in \mathcal{D}\text{ holds }\mathfrak{J},\mu \models\alpha \text{, where }\mu[x]=d$ 
	- 
#### Classification of sentences
- Sentence $\alpha$ is 
	**Unsatisfiable** if no interpretation satisfies $\alpha$ 
	**Satisfiable** if at least one interpretation satisfies $\alpha$
	**Not valid** if at least one one interpretation does not satisfy $\alpha$ 
	**Valid** if every interpretation satisfies $\alpha$ 
We can allow free variables e.g., a forumla is satisfiable if there exists an interpretation $\mathfrak{J}$ and a variable assignment $\mu$ such that $\mathfrak{J}, \mu \models \alpha$ 

#### Logical entailment
- Satisfaction of a sentence generally depends on interpretation - i.e., whether a formula is true depends on how we interpret the symbols - truth is relative to interpretation.
- Sometimes want to express relationships between sentences that hold no matter the interpretation - hold in all possible interpretations. So instead of looking at one interpretation, we consider all.
	Let:
		$S$  = a set of sentences
		$\alpha$ = a single sentence
		We say that $S \models \alpha$, $S$ logically entails $\alpha$ $\iff$ for every interpretation $\mathfrak{J}$, if $\mathfrak{J} \models S$ then $\mathfrak{J} \models \alpha$ as well.
		Thus $\alpha$ is a logical consequence of $S$ - it cannot be false when $S$ is true.
#### Logical validity
- A sentence $\alpha$ is logically valid if it is satisfied by every interpretation. 
- An equivalent definition: a formula is valid if it is a logical consequence of an empty set: $$S \models \alpha \ \text{where} \ S = \emptyset$$ $$\text{we write} \models \ if \ \alpha \ \text{is valid and} \neg\models \alpha \text{for not valid}$$
#### Unsatisfiability
- Sentence $a$ is unsatisfiable if there is no $\mathfrak{J}$ such that $\mathfrak{J} \models \alpha$ 
- E.g. $α = α'∧ ¬α'$is unsatisfiable
- Unsatisfiability can be expressed using entailment
	- $S \models FALSE$, false is a consequence of $S$ 



#### Practice Questions
##### Exercise 1
![[Pasted image 20251008110245.png]]
##### Exercise 2
1. $f(x1) ∧ g(x2)$
	Not a WFF - functions return domain elements, not booleans, thus cannotn use logical connectives. 
2. $A ∨ (B ∧ ∃x1.C(x1))$ 
	WFF
3. $B(A(x1))$ 
	Not a WFF, A(x_1) is a formula whereas the parameter of B is a term.
4. $∃A.(f(x1) = x2) ∧ A$ 
	Not a WFF, quantifier requires variable not a predicate.
5. $[∀x1∃x2.A(x1, x2) ∧ ¬B(x2)] ∨ [∃x1∃x2∃x3.(f(x1) = g(x1, x2, x3)) ∧ (A(x1) = B(x2))]$ Not a WFF, equality connective at end cannot be used between predicates.
##### Exercise 3





![[Pasted image 20251008105455.png]]
