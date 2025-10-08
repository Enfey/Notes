## Equational reasoning/Reasoning about haskell
We view function definitions as a property that can be used to reason about haskell.
A function can be used in $L \to R$ direction by simply applying the function.

For example, for any integer expression $x$, any $double \ x$ can be replaced by $x + x$. Can also run functions backwards e.g., replace occurrence of $x+x$ with function definition. Reasoning with multiple equations often requires strictness to apply in $R \to L$ and $L \to R$ directions, that is, for the reasoning to be sane. 

![[Pasted image 20250511012935.png]]
![[Pasted image 20250511013112.png]]

We can use equational reasoning with inductive proofs over the recursive types specified in Haskell.


## Induction Proofs
$$\frac{P(Zero)  \ \ \ \ \ \forall{n.}[P(n) \implies P(Succ n)]}{\forall{P(n)}}$$ If prove property for zero, then assume it holds for arbitrary n, if can prove for successor of arbitrary n, then we can say that the property holds for all natural numbers. 

### General principle of induction
==Structural induction over algebraic data types, is the fundamental proof vehicle== in Haskell for ==showing== that a ==property $P(x)$== holds for some recursively defined type $T$ where $x :: T$ .

To prove $P(x)$ $\forall{x::T}$ it suffices to show:
	1. **Base cases:** For each non-recursive constructor of $T$, prove $P(nonrecursiveconstructor \ args)$
	2. **Inductive Cases** For each recursive constructor of $T$, assume as induction hypothesis, that $P$ holds for each recursive sub-value, then prove $P(recursiveconstructor args)$ 

This is necessary in Haskell. Instead of examining every possible infinite value of $T$, it is enough to check the finitely many ways to construct a term of type $T$, according to induction.

### Induction On Nat/Numbers
`data Nat = Zero | Succ Nat`
Algebraic data type representing the peano numbers
Induction on this type would suffice by showing that the property holds for the non-recursive constructor, that is, `Zero` and then proving the inductive case. That is, we adopt an induction hypothesis, assuming that $P$ holds for each recursive sub-value, that is nat, and then we must prove the property holds for $Succ Nat$. 

We leverage equational reasoning in our inductive proofs like so: 
```haskell
add :: Nat -> Nat -> Nat 
add Zero y = y 
add (Succ x) y = Succ (add x y)
```

#### Prove `add x Zero = x`
***Base case***
`add zero zero = zero`
`zero`, applying add

***Inductive case***
**ih** = `add x zero` = x
`add (Succ x) zero`
`Succ (add x zero)`, applying add
`Succ (x)`, ih
`(Succ x)`


#### Prove `add (add x y) z = add x (add y z)`
***base case***
add zero (add y z), start with rhs
add y z, apply add
add y z, unapply from bottom
add (add zero y) z

***inductive case***
**ih** add(add x y) z = add x (add y z)
proving add (Succ x) (add y z) = add(add Succ x y) z

add (Succ x) (add y z) 
Succ (add x add y z)
Succ (add x (add y z))
Succ (add (add x y) z)

, unapply add, unapply add, boom
= add(add Succ x y) z


### Induction on Lists

len :: List a -> Int 
len Nil = 0 
len (Cons x xs) = 1 + len xs

len (xs ++ ys) = len xs + len ys
***base case***
len(nil ++ ys) = len nil + len ys
len(nil ++ ys)
len(ys), apply append/unapply len nil
len nil + len ys

**inductive case**
len(Cons x(xs) ++ ys) = len Cons x(xs) + len ys
ih = len (xs ++ ys) = len xs + len ys

len(Cons x(xs) ++ ys) 
1 + len(xs ++ ys), applying len
1+len xs + len ys, ih/unapplying len from bottom
 len Cons x(xs) + len ys



### Induction on Trees
leafCount :: Tree a -> Int
leafCount (Leaf _)   = 1
leafCount (Node l r) = leafCount l + leafCount r

nodeCount :: Tree a -> Int
nodeCount (Leaf _)   = 0
nodeCount (Node l r) = 1 + nodeCount l + nodeCount r

Prove 
leafCount t = nodeCount t +1.

***Base case***
leafCount Leaf = nodeCount Leaf + 1
leafCount Leaf
1, applying leafCount
nodeCount Leaf + 1, equivalent


***Inductive case***
leafCount (Node l r) = nodeCount (Node l r)
ih = leafCount l = nodeCount l + 1
ih = leafCount r = nodeCount r + 1

leafCount (Node l r)
leafCount l + leafCount r, applying leafCount
nodeCount l + 1 + nodeCount r + 1, ih/unapplying nodeCount
nodeCount (Node l r) + 1







## Constructive Induction
Using the general principle of induction and equational reasoning in Haskell, we can define functions, via following the structure of an algebraic data type. The function is constructed in accordance with the constructors of the ADT. 
This is often used to optimise existing functions. We identify the expensive pattern in a naive definition e.g., repeated concatenation, recomputing results, and systematically derive a more efficient version, by specifying the behaviour the new function should generalise over and be equivalent to.

For example, take reverse:

```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = reverse xs ++ x
```

To ask ourselves why this is slow, we must first consult how many evaluation steps an arbitrary call to ++ takes, and then calculate it for reverse

**For ++:**
**Example**
`[1,2] ++ [3]`
= `1 : ([2] ++ [3])
= `1 : (2 : ([] ++ [3]))
= `1 : (2 : ([3]))
3 evaluation steps
For an arbitrary `xs ++ ys`, there are `length xs` + 1 evaluation steps. 
1 for base case.


**For Reverse:**
**Example**
`reverse [1,2]`
= `(reverse [2]) ++ [1]`
= `(reverse []) ++ [2] ++ [1]
= `[] ++ [2] ++ [1]
3 evaluation steps. 
But did not factor in cost of append.
	`[] ++ [2] takes 1 step`
	`[2] ++ [1] takes 2 steps`
Can draw conclusion.
Conclusion is in general, reverse of `xs` takes
	$1+2+...+(n+1)$ steps, where $n$ = `length xs`
		(first bit is appends, $n+1$ is cost of reverse without appends factored in)
Due to **Gauss's** Formula, the evaluation steps can now be calculated as follows.
$$\frac{(n+1)(n+2)}{2}$$
![[Pasted image 20250524010929.png]]

## Fast reverse
`reverse' xs ys = (reverse xs) ++ ys`
Want to define a function, `reverse'` which has the **same effect** as reverse, but eliminates the use of append. Want to solve the equation above. We want to construct this `reverse'` via ***constructive induction**.*

***Base case***
`reverse' [] ys = (reverse []) ++ ys`
`reverse [] ys`
`(reverse[]) ++ ys`, specification
[]++ys, applying reverse
ys
Via constructive induction, derived base case definition for our new recursive function

***Inductive case***
`reverse' (x:xs) ys = (reverse (x:xs)) ++ ys`
**IH**: `reverse' xs ys = (reverse xs) ++ ys`

`reverse' (x:xs) ys`
`(reverse (x:xs)) ++ ys`
reverse xs ++ [x] ++ ys
reverse xs ++ (x ++ ys), assoc of append
reverse xs ++ (x:ys), apply append
reverse' xs x:ys, via constructive induction



```haskell
data Tree = Leaf int | Node Tree Tree

flatten :: Tree -> [Int]
flatten (Leaf n) = [n]
flatten (Node L R) = flatten L ++ flatten R

flatten' t ns = flatten t ++ ns

flatten' (Leaf n) ns = flatten (Leaf n) ++ ns
flatten' (Leaf n) ns
flatten (Leaf n) ++ ns
[n] ++ ns
n : ns

flatten' (Node L R) ns = flatten (Node L R) ++ ns
ih = flatten' L ns = flatten L ++ ns
ih = flatten' R ns = flatten R ++ ns

flatten' (Node L R) ns = flatten (Node L R) ++ ns
flatten' (Node L R) ns
flatten (Node L R) ++ ns
flatten L ++ flatten R
flatten' L ns ++ flatten' R ns
flatten' L ns (flatten' R ns)


```

Always include accumulator


### Fast Flatten full definition
```haskell
flatten' :: Tree -> [Int] -> [Int]
flatten' (Leaf n) = n : ns
flatten' (Node l r) = flatten' l (flatten' r ns)

flatten :: Tree -> [Int]
flatten t = flatten' t []



