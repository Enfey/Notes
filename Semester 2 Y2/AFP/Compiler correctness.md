## Source language
```haskell
data Expr = Val Int | Add Expr Expr
```
Very simple type expressing integer addition.

### Semantics
Assign meaning to expression beyond just that expression

```haskell
eval :: Expr -> Int
eval (Val v) = v
eval Add(e e') = eval e + eval e'
```

Simple evaluation gives us our semantics.


### Target language
Targeting a stack-based virtual machine.

```haskell
type Stack = [Int]
type Code = [Op]
data Op = PUSH Int | ADD  
```

cons and pattern match on head to have list behave like stack. 

#### Semantics
```haskell
exec :: Code -> Stack -> Stack
exec [] s = s
exec (Push n : cs) s = exec c (n : s)
exec (ADD : c) (i : i' : s) = exec c (i+i' : s) 
```


### Compiler
```haskell
comp :: Expr -> Code
comp (Val v) = [PUSH v]
comp (Add e e') = comp e ++ comp e' ++ [ADD] 
```
leave e on top of stack
then e' on top of stack
then add, so works.

![[Pasted image 20250527221411.png]]

![[Pasted image 20250527222451.png]]
Commutative diagram denoting relation between types and functions. 
### Compiler correctness
What does it actually mean for our compiler to be correct?
If we execute the result of compiling any expression, then we expect to get back the evaluated value of that expression, according to the exec function. 

**Essentially:**
```haskell
exec (comp e) [] = [eval e]
```

Generalising from earlier example.
But run into issue, using the empty list for an induction proof, cannot prove that these 2 are equivalent inductively, must generalise the empty stack to an arbitrary stack.


```haskell
exec (comp e) s = (eval e) : s
```


Want to prove this is true for an arbitrary expression e, so proceed by induction on e.

***Base Case***
`exec (comp (Val n)) s`
`exec [PUSH n] s`, apply comp
`n : s`, apply exec
= `eval (Val n) : s`


***Inductive Case***



