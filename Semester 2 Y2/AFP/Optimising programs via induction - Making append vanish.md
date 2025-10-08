

# Making Append Vanish : Fast Reverse
The library function append is defined in Haskell like so : 
```haskell
(++) :: [a] -> [a] -> [a]
[] ++ ys = ys
(x:xs) ++ ys = x : (xs ++ ys)
```

The library function reverse is define in terms of append, like so: 
```haskell
reverse :: [a] -> [a]
reverse [] = []
reverse (x:xs) = (reverse xs) ++ [x]
```

To ask ourselves why this reverse is slow, we must first consult how many evaluation steps a given call of append makes, and then how many given steps a call of reverse makes for an arbitrary list can be calculated.

## How many steps does xs ++ ys take? 
**Example**
`[1,2] ++ [3]`
= `1 : ([2] ++ [3])
= `1 : (2 : ([] ++ [3]))
= `1 : (2 : ([3]))
3 evaluation steps
For an arbitrary `xs ++ ys`, there are `length xs` + 1 evaluation steps. 
1 for base case.
One evaluation step for each list element in `xs` due to pattern matching on head and recursive case.

## How many evaluation steps does an arbitrary reverse take? reverse xs
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
OR, simplifying
$$\frac{n^2+3n+2}{2}$$
### Gauss's Formula
![[Pasted image 20250524010929.png]]
E.g., n = 5, do (5 x 6) / 2 to get sum for all i up to n.

## Fast Reverse
`reverse' xs ys = (reverse xs) ++ ys`
Want to define a function, `reverse'` which has the **same effect** as reverse, but eliminates the use of append. Want to solve the equation above. We want to construct this `reverse'` via ***constructive induction**.*


### Constructive Induction for `reverse'`
***Base Case***
`reverse' [] ys`
	= `(reverse []) ++ ys` - by applying the specification that reverse' must satisfy.
	= `[] ++ ys` - apply reverse
	= `ys` 
Using equational reasoning and induction, and without having defined the base case for `reverse'`, we have calculated the base case which is as follows:
`reverse' [] ys = ys`

***Inductive Case***
`reverse' (x:xs) ys`
= `reverse(x:xs) ++ ys` -- apply specification
= `(reverse xs ++ [x]) ++ ys` -- apply reverse
= `reverse xs ++ ([x] ++ ys)` -- associativity of append 
= `reverse xs ++ (x:ys)` -- apply append 
(stuck here, cannot apply reverse, know nothing about xs, can't do ++as don't know if xs is empty, must use induction hypothesis, which matches, as assumed that `reverse' xs ys = (reverse xs) ++ ys`, just tacked on extra x at front)
= `reverse' xs (x:ys)`

Thus via constructive induction, obtained the definition of `reverse'` : 

```reverse' (x:xs) ys = reverse' xs (x:ys)```

### Definition of reverse'
```haskell
reverse' :: [a] -> [a] -> [a]
reverse' [] ys = ys
reverse' (x:xs) ys = reverse' xs (x:ys)

reverse :: [a] -> [a]
reverse xs = reverse' xs []
```
![[Pasted image 20250524013730.png]]
![[Pasted image 20250524013834.png]]
Took 5 steps, thus n + 2 where n = `length xs` 
Use the `[]` from call to wrapper reverse as an accumulator which accumulates the result for us.


# Making Append Vanish : Fast Flatten
Previously, obtained definition of more efficient reverse via ***constructive induction***. Can also eliminate calls to append which are purely convenient, for more efficient operations, for a tree flattening function.

## Initial Flatten Function Definition
```haskell
data Tree = Leaf int | Node Tree Tree

flatten :: Tree -> [Int]
flatten (Leaf n) = [n]
flatten (Node L R) = flatten L ++ flatten R
```

In order to derive a more efficient flatten, use a trick. Define a more general function, that combines the behaviour of both flattening and  append.


### Fast Flatten Equational specification
```flatten' t ns = flatten t ++ ns```
Specifies how we would like fast flatten to behave, want its behaviour to be equivalent, without referring to original flatten function or append function.

HOW DOES HE PICK WHICH TO DO? is it just that he wants to eliminate the append, and so attaches its behaviour to a recursive call instead?
### Constructive Induction to derive Fast Flatten
***Base Case***
Perform induction on `t`
```flatten' (Leaf n) ns```
= `flatten (Leaf n) ++ ns` -- using specification
= `[n] ++ ns` -- apply flatten
= `n : ([] ++ ns)` --apply append
= `n : ns` -- apply append, done.

`flatten' (Leaf n) ns = n : ns`

***Inductive Case***
`flatten' (Node l r) ns` 
= `flatten (Node l r) ++ ns` -- Via specification
= `flatten l ++ flatten r ++ ns` -- apply flatten
= `flatten l ++ (flatten r ++ ns)` -- associativity of append
=` flatten l ++ (flatten' r ns)` -- via specification
= `flatten' l (flatten' r ns)` -- via specification


`flatten' (Node l r) ns = flatten' l(flatten' r ns)`


### Fast Flatten full definition
```haskell
flatten' :: Tree -> [Int] -> [Int]
flatten' (Leaf n) = n : ns
flatten' (Node l r) = flatten' l (flatten' r ns)

flatten :: Tree -> [Int]
flatten t = flatten' t []
```