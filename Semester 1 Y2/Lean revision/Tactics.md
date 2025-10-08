## Assume
Most commonly used: Variety of logical systems
==Introduces== a ==named assumption==, and ==changes== the ==goal== respectively. 
#### When to use w examples
``` lean
example (P Q : Prop) : P → Q → P :=
begin
  assume P : P,
  assume Q : Q,
  exact P,
end
```
##### Final goal state
![[Pasted image 20250119002509.png]]
#### Caveats
- ==Assuming== something that is '==not==' ==results in false== in the goal
- ==Assuming an inequality==, l==eaves the equality as a hypothesis==, and ==false as the goal==

## Apply
Most commonly used: Variety of logical systems
==Matches== the ==goal== with the ==conclusion== of a ==provided assumption== or ==lemma== to ==simplify the goal==/==generate subgoals== for its premises. 

WHEN APPLYING TO HYPOTHESIS OF FORM P->Q->R, CREATES MULTIPLE GOALS E.G., APPLY PQR RESULTS IN A GOAL WITH P, AND A GOAL WITH Q.
#### When to use w examples
Using lemma
```
example (b r : bool) (h : b = tt → r = true) : b = tt → r = true :=
begin
  apply h,
end
```
Proposition assumptions
```
theorem swap : (P → Q → R) → (Q → P → R) :=
begin
  assume f q p,
  apply f,
  exact p,
  exact q,
end
```
Goal state after assumption
![[Pasted image 20250119003032.png]]
Predicate assumptions
```
example (P Q : ℕ → Prop) (h : ∀ x, P(x) → Q(x)) (n : ℕ) (hP : P(n)) : Q(n) :=
begin
  apply h,
  exact hP,
end
```
==APPLY TO NOT STATEMENTS WHEN HAVE FALSE IN GOAL==
### Constructor
Most commonly used: Inductive types
==Constructs== a ==term== for an ==inductive type== by ==splitting the goal== into ==subgoals== ==corresponding== to the constructo==r arguments.==
#### When to use w examples
`And`
```
example (P Q : Prop) (hP : P) (hQ : Q) : P ∧ Q :=
begin
  constructor,
  exact hP,
  exact hQ,
end
```
When there is ```iff``` in the goal
```
theorem curry : P ∧ Q → R ↔ P → Q → R :=
begin
	constructor,
end
```
becomes
![[Pasted image 20241203160719.png]]

## Left/Right
Most commonly used:
==Chooses left or right branch of disjunction== goal to prove, ==changing the goal.==
#### When to use w examples
```
example : Q → P ∨ Q :=
begin
  assume q,
  right,
  exact q,
end

example : P → P ∨ Q :=
begin
  assume p,
  left,
  exact p,
end
```
#### Caveats

## Cases
==Performs case analysis== on ==inductive types== (types which have values specified from constructors). ==Generates subgoals== for ==each constructor== of that type.
Most commonly used: ==On assumptions only.==
CASES ON OR, 2 DIFF GOALS, DIFF ASSUMPTION IN EACH.
CASES ON AND, 1 GOAL, HAVE BOTH ASSUMPTIONS. 
#### When to use w examples
Booleans
```
example : ∀ x : bool, x=tt ∨ x=ff :=
begin
  assume x,
  cases x,
  right,
  refl,
  left,
  refl,
end
```
After cases, does false first.
![[Pasted image 20250119004044.png]]
Or
```
theorem case_lem : (P → R) → (Q → R) → P ∨ Q → R :=
begin
  assume p2r q2r pq,
  cases pq with p q,
  apply p2r,
  exact p,
  apply q2r,
  exact q,
end
```
And
```
theorem comAnd : P ∧ Q → Q ∧ P :=
begin
  assume pq,
  cases pq with p q,
    constructor,
    exact q,
    exact p
end
```
After cases
![[Pasted image 20250119004235.png]]

False
```
theorem efq : false → P :=
begin
  assume pigs_can_fly,
  cases pigs_can_fly,
end
```
Existential quantifier as assumption
```
	theorem curry_pred : (∃ x : A, PP x) → R  ↔ (∀ x : A , PP x → R)  :=
	begin
	  constructor,
	  assume ppr a p,
	  apply ppr,
	  existsi a,
	  exact p,
	  assume ppr pp,
	  cases pp with a p,
	  apply ppr,
	  exact p,
	end
```
Before cases
![[Pasted image 20250119004601.png]]
after cases
![[Pasted image 20250119004622.png]]
Nat
```
theorem mult_rneutr : ∀ n : ℕ, n * 1 = n :=
begin
	assume n,
	cases n with n',
end
```
![[Pasted image 20250119005336.png]]
![[Pasted image 20250119005419.png]]


List
Cons and nil case.

## Have
Most commonly used:
==Introduces== an ==intermediary result== that can be ==used later== in the proof, that ==must also be proven== ==given== the ==current goal state.== 
#### When to use w examples
```
example : (P → Q ∧ R) → P → Q :=
begin
assume pqr p,
have qr : Q ∧ R,
apply pqr,
exact p,
cases qr with q r,
exact q,
end
```
#### Caveats

## Raa
Most commonly used: Classical logic

#### Definition
![[Pasted image 20241203172953.png]]

#### When to use w examples
IDK

## Em
Most commonly used: Classical logic
==Applies== the ==law== of ==excluded middle== to a ==proposition==, that is, either a ==proposition is true==, or its ==negation is true.==

==_principle of indirect proof_== or in latin _reduction ad absurdo_ (reduction to the absurd). This ==principle tells you== that ==to prove== `P` it is s==ufficient to show that `¬ P` is impossible==.
#### When to use w examples
``` 
theorem raa : ¬ ¬ P → P :=
begin
assume nnp,
cases (em P) with p np, 
exact p,
have f : false,
apply nnp,
exact np,
cases f,
end
``` 
==EM== ==SPAWNS THE P== OUT OF ==THIN AIR== AS AN ==ASSUMPTION== FOR ==ONE GOAL==, AND ==NP FOR THE OTHER==. SEE BELOW FOR AFTER CASES EMP P WITH P NP.
![[Pasted image 20250119020734.png]]


## Existsi
Most commonly used: Predicate logic
==Provides a witness== to prove an ==existential statement==. ==NEED THAT TYPE ASSUMED==, THEN ==PROVIDE IT AS A WITNESS== TO ==EXISTENTIAL IN THE GOAL.==
#### When to use w examples
```
example : (∃ x : A, PP x) → (∀ y : A, PP y → QQ y) → ∃ z : A , QQ z :=
begin
  assume p pq,
  cases p with a pa, -- cause it existential assumption
  existsi a,
  apply pq,
  exact pa,
end
```
![[Pasted image 20250119010659.png]]
IF GOAL IS LIKE THIS, CANT ASSUME, BOUND TO Z. must provide witness
## Rewrite
==Replaces occurrences== of a term ==using an equality==, uses an assumed equality to rewrite the goal.

#### When to use w examples
##### Rewrite
```
example:  ∀ x y : A, x=y → PP y → PP x :=
begin
  assume x y eq p,
  rewrite eq,
  exact p,
end
```
##### Rewrite <-
```
example:  ∀ x y : A, x=y → PP x → PP y :=
begin
  assume x y eq p,
  rewrite← eq,
  exact p,
end
```

## Refl
==Solves goals== where ==LHS== ==equals== the ==right hand side==, by r==eflexivity of equality.==
#### When to use w examples
Pretty obvious

## Symmetry 
==Swaps the sides of an equality in the goal==, also ==works for iff==.
#### When to use w examples
#### Caveats

## Transitivity
Introduces an intermediate step to prove transitive relations like equality or inequality. 
#### When to use w examples
#### Caveats

## dsimp
==Simplifies expressions== by ==unfolding definitions.== 
#### When to use w examples
bool
```
def band : bool → bool → bool
| tt b := b
| ff b := ff

def bnot : bool -> bool 
| tt := ff
| ff := tt

def bor : bool -> bool -> bool 
| tt b := tt
| ff b := b

theorem distr_b : ∀ x y z : bool,
  x && (y || z) = x && y || x && z :=
begin
  assume x y z,
  cases x,
  dsimp [band],
  dsimp [bor],
  refl,
  dsimp [band],
  refl,
end
```
NAT




## Contradiction 
Looks for contradiction among the hypotheses of the current goal. Only ==tt = ff== and ==succ x== = 0 e.g., 0 = 1
Most commonly used:
#### When to use w examples
#### Caveats

## Induction
==Performs proof by induction for inductive types==. Ie, P==rove that the statement is true== for the ==initial value of 𝑛==, 0.  ==Assume the statement is true for some n==', the ==induction hypothesis==, and ==via this== ==prove== that if the ==statement is true for k==, the ==statement is true for k+1.==
#### When to use w examples
Nat
```
lemma add_succ_lem : ∀ m n : ℕ, succ m + n = succ (m + n) :=
begin
  assume m n,
  induction n with n' ih,
  refl,
  apply congr_arg succ,
  exact ih,
end
```


## Change
Same as dsimp, used to simplify goal though exclusively.
```
def is_tt : bool → Prop
| ff := false
| tt := true
```

```
theorem cons : tt ≠ ff :=
begin
  assume h,
  change is_tt ff,
  rewrite ← h,
  trivial,
end
```
Change results in: 
![[Pasted image 20250119022450.png]]

## Injection
![[Pasted image 20250119021339.png]]
Used to extract equalities from an inductive type when have a hypothesis of the form f a = f b and f is injective.
#### When to use w examples
```
example :  ∀ m n : nat, succ m = succ n → m = n :=
begin
  assume m n h,
  injection h,
end
```



