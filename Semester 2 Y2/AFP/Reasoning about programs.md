1. How to do **proofs** about programs
2. How to improve program **efficiency**
3. Case study : **compiler correctness**

## Equational reasoning
Method of proving the equivalence of 2 expressions or programs by principle of substituion, where equals can be subsituted for equals.

![[Pasted image 20250511011213.png]]
**Commutativity** - order of operations doesn't matter.
**Associativity** - way numbers are grouped doesn't affect result
**Distributivity (left and right)** - can be distributed over.

$(x+a)(x+b)$ 
= $(x+a) \ x + (x+a) \ b$ - left distributivity property
= $x^2 + xa + (x+a)b$ - right distributivity
= $x^2 + xa+xb+ab$ - right distributivity
=$x^2+(a+b)x + ab$ -left distributivity
Implicitly used associativity of addition while performing equational reasoning steps.


## Reasoning about Haskell 
```haskell
double :: Int -> Int
double x = x + x
```
Can view this function definition as a property to be used in equational reasoning.

Can use in L->R direction by simply applying function. Given any integer expression x, any $double \ x$ can freely be replaced by $x+x$.
Can also run functions backwards. Given any integer expression x, the expression $x+x$ can be freely replaced by $double \ x$. 

```haskell
isZero :: Int -> Bool
isZero 0 = True
isZero n = False
```
![[Pasted image 20250511012935.png]]

Cannot simply apply the second equation without restriction - reasoning with multiple equations requires some strictness to apply in R->L and L->R directions. 
![[Pasted image 20250511013112.png]]
Would permit both directions, making the implicit assumption that $n \neq 0$ visible

### Induction
![[Pasted image 20250511014449.png]]
Where $p$ is some property, if prove for $zero$, then use induction hypothesis for prove for any natural number $n$.

```haskell
data Nat = Zero | Succ Nat

add :: Nat -> Nat -> Nat
add zero m = m
add (Succ n) m = Succ (add n m)
```

Show that:
```haskell
add n zero = n
```
P(n) = add n Zero = n (right identity)

Base case: $P(Zero)$
Show $add \ zero \ zero = zero$
	add Zero Zero 
	= applying add first case
	= Zero

Inductive case: $P(n) \to P(Succ \ n)$ 
Show: $add \ n \ zero = n \implies add(Succ \ n) \ Zero = Succ \ n$ 
first bit is induction hypothesis
usually start with largest subexpression

add (Succ n) Zero
= apply add
   Succ (add n Zero)
= apply induction hypothesis
   Succ(n)

### Further Example:
Show that add is associative
add x (add y z) = add ( add x y ) z 

3 natural numbers need to decide which to perform induction on. Refer to definition of add, which is defined by recursion on its first parameter.
x in recursion position twice
y in recursion position once
z in recursion position not at all
so try induction on x first

P(x) = add x (add y z) = add ( add x y ) z 
Base case: add zero(add y z) = add(add zero y) z

add Zero (add y z)
= add y z, applying add
= add y z, unapplying add, below
= add(add Zero y) z

Inductive case: add x (add y z) = add ( add x y ) z $\implies$ add (Succ x) (add y z) = add (add (Succ x) y) z

 add (Succ x) (add y z) 
 = Succ (add x (add y z)), apply add
 = Succ (add (add x y)z), apply ih, unapplying from bottom
 = add(Succ (add x y))z
 = add (add (Succ x) y) z
 (watch video/try for self, makes sense)


### Induction principle - Lists
Performing induction principle on lists, for each constructor of its type. 
![[Pasted image 20250511025829.png]]
1. Base case: Show property P holds for empty list.
2. Inductive step: Assume as an inductive step that P(xs) holds for an arbitrary list xs, and then show that P(x:xs) holds for any additional head element x.
3. From those, may conclude that holds for every list xs.

##### Example:
```haskell
rev :: [a] -> [a]
rev [] = []
rev (x:xs) = rev xs ++ x --cons expects right arg to be list
```

Show that, via structural induction over lists:
```haskell
rev(rev xs) = xs
```

**Base case:**
rev(rev [ ]) = [ ]
= rev [ ], apply rev
= [ ], apply rev

**Inductive case:** 
rev (rev xs) = xs => rev(rev(x:xs)) = x:xs

rev(rev(x:xs)) = x:xs
= rev(rev xs ++ [x]), apply rev
= Stuck! need distributivity of rev over append lemma
![[Pasted image 20250511031744.png]]
![[Pasted image 20250511032044.png]]

![[Pasted image 20250511031950.png]]


