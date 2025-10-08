![[Pasted image 20250119205653.png]]
- P v Q, Primitive
- P -> Q, implication, primitive
- true, true, primitive
- not p, negation, P->False
- <-> logical equivalence


![[Pasted image 20250119205031.png]]
if disjunction appears on the right, classically provable from left->right. 
X I
C I
I X
I I 
Effectively demo
![[Pasted image 20250119182619.png]]
i) Parent x y ^ Female x
ii) ∃ z : People, Parent z y ^ Mother x z
iii) ∀y: People, ∃ x : People, Mother x y
iiii) ∀y: People, ∃ m1 m2: People, Mother m1 y ^ Mother m2 y -> m1 = m2

def Father (x y : People) : Prop := Parent x y ^ Male x
def Grandfather (x y : People) : Prop := ∃ x : People, Parent z y ^ Father x z
def only_one_father : Prop :=  ∀y : People, There exists f1 f2 : People, Father f1 y ^ Father f2 y -> f1 = f2
def every_parent : Prop := ∀y : People, ∃ x : People, Father x y
def no_self_parent : Prop := ¬∃ y : People, Parent y y

def Aunt
def Cousin
def half-sibling
def Sibling (x y : People) ∃ z : People, Parent z y ^ Parent z x ^ x \= y
def Uncle (x y : People) : Prop := ∃ z : People, Parent z y ^ Male x ^ Sibling x z
def Ancestor (x y : People) : Prop := Parent x y v E z : People, Ancestor x z ^ Parent z y
def exactly_one_no_parents : Prop := E! x : People, ∀y : People, ¬Parent y x
X IS THE SUBJECT 


![[Pasted image 20250119205835.png]]
i) P->(Q->R)
ii) The readings are not equivalent: CONSTRUCT COUNTER EXAMPLE USING FALSES
(false->false) = t -> false = false
false->(false->false)
false -> true = true


![[Pasted image 20250119210821.png]]
- Apply h1
- existsi a
- cases h with a PP
- refl
- rewrite h <-, rewrite h
![[Pasted image 20250119215345.png]]
i) right
ii) no, counterexample, emptyset, for all x (of which there is none, so true) and whatever q is, but in the second one, the proposition is bound to the range of the quantifier, making it so that the predicate and q must be true for all x, of which there is none. So if q is false, the left would be false, but the right would be true. 

![[Pasted image 20250119215526.png]]
- P1 = P
- P2 = N
- P3 = P
- P4 = P 
- P5 = N
![[Pasted image 20250119215720.png]]
n: N
|- PP n 

cases n with n',
n: N
|- PP 0

n': N
|- PP succ(n')


induction n with n' ih, 
n: N
|- PP 0

n': N
ih: PP n'
|- PP succ(n')

Cases performs simple case analysis on inductive types and breaks down an assumption, providing subgoals relating to the constructors of that type. Induction performs proof by induction on inductive types, therefore providing an induction hypothesis in the form of ih, specifying the assumption that PP holds for n', whilst retaining the same goal of proving this for n'+1/succ(n').
![[Pasted image 20250119215727.png]]
![[Pasted image 20250119220402.png]]
Q1: P
Q2 : N
Q3 : P
Q4 : N
Q5: P









