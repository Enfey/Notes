Example: 
```haskell
data Expr = Val Int | Div Expr Expr

--Example representation
e :: Expr
e = Div (Val 6)(Val 3)

--Evaluator, BUT doesn't deal with division by 0
eval :: Expr -> Int
eval (Val n) = n
eval (Div x y) = eval x `div` eval y

safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv (x y) = Just(x `div` y)

eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = case eval x of
					Nothing -> Nothing
					Just n -> case eval y of
						 Nothing -> Nothing
						 Just m -> safediv n m

--Quite messy and exhaustive, try to use applicatives?


eval :: Expr -> Maybe Int
eval (Val n) = pure n
eval (Div x y) = pure safediv <*> eval x <*> eval y
```
Results in a type error via use of safediv. Not a pure function, so when brought into context of applicative and applied to effectful arguments, fails. Needs type Int->Int->Int, not Int->Int->Maybe Int
When apply pure safediv < * > eval x, types don't match, applicatives expect: 
	Maybe(Int->Int) not Maybe(Int->Maybe Int), which is what the first application results in, leading to a type error as there are nested maybe results which cannot be utilised further, the values need unwrapping to be used and then wrapping again at the end of the operation.
Here we see a common pattern - applicative style fails, and exhaustive pattern matching and forwarding of results based on whether a computation succeeds or not.

## The monadic pattern
```haskell
case ___(mx) of
	Nothing -> Nothing
	Just x -> ___(f) x 
```
Where mx is a monadic value, and f is a function. 
```haskell
case mx of
	Nothing -> Nothing
	Just x -> f x 
```
Can abstract the mx and f, unknown variables into a definition. 
```haskell
mx >>= f = case mx of 
			Nothing -> Nothing
			Just x -> f x
```
Bind:
```haskell
(>>=) :: Maybe a -> (a->Maybe b) -> Maybe b
Nothing >>= f = Nothing 
(Just x) >>= f = f x
```
Take Maybe a, a function that tells us what to do with a once unwrapped, and returns a Maybe.
```haskell
eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = eval x >>= \n -> eval y >>= \m -> safediv n m
```
Initially use bind operator: Arguments
1. Eval x = Just n
2. \n -> eval y >>= \m -> safediv 
Then use bind operator in this function again
3. eval y = Just n
4. \m -> safediv n m
First bind operator takes eval, and is predicated on anonymous function, second one takes the second anonymouse function which just performs safediv on the unwrapped arguments.
![[Pasted image 20250303062049.png]]
This is suitable for applying impure functions to effectful arguments, often where the return type

### Typical use of binding operator for maybe
Have some monadic computation, $m1$ 
```haskell
m1 >>= \x1 ->
m2 >>= \x2 ->
    .
    .
    .
mn >>= \xn ->
f x1 x2 ... xn -- returns effectful type, over pure arguments
```

This pattern appears frequently, the `do` notation allows us to do away with the lambdas.
```haskell
do x1 <- m1
   x2 <- m2
      .
      .
      .
   xn <- mn
f x1 x2 .. xn = --monadic type.


eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = do n <- eval x
					m <- eval y
				safediv m n
```


# Monad definition
The class of applicative functors which support a bind operator are called monads. 
```haskell
class Applicative m => Monad m where
	(>>=) :: M a -> (a -> M b) -> M b
	return :: a -> m a

	return = pure -- default definition.

```

## Maybe Monad
```haskell
instance Monad Maybe where
	--(>>=) :: Maybe a -> (a -> Maybe b) -> Maybe b
	Nothing >>= f = Nothing
	Maybe x >>= f = f x
	
```

## List monad
```haskell
instance Monad [] where 
	-- (>>=) :: [a] -> (a -> [b]) -> [b]
	xs >>= f = concat (map f xs)
	--would be [[b]] without concat
```

```haskell
--Monadic pairs
> pairs [1,2] [3,4]
[(1,2), (1,3), (2,3), (2,4)]

pairs :: [a] -> [b] -> [(a,b)]
pairs xs ys = do x <- xs
			  do y <- ys
			  return (x,y)	  

--Equivalent
pairs :: [a] -> [b] -> [(a,b)]
pairs xs ys = xs >>= \x -> ys >>= \y -> return(x,y)	  
```
![[Pasted image 20250307151726.png]]


## State monad
```haskell
type State = ... -- could have any type of state we want

type ST a = State -> (a, State) -- state transformer, a is result type


--if wanted to have a function

eg :: Char -> ST Int

--is equivalent to curried function

eg :: Char -> State -> (Int, State)


-- want to make ST an instance of monad class, but can't, as is 'type' not data


data ST a = S(State -> a, State)


-- can also use newtype, which is for datatypes with only one constructor

newtype ST a = S(State -> (a, State))

-- also nee
instance Monad S where
	-- (>>=) ST a -> (a -> ST b) -> ST b
	-- return :: a -> ST a
	return a = S(\s->(a,s))
	ST >>= f = S(\s->
				let (x,s') = app ST s
				in app(f x) s')

-- Next step is to implement an F to understand this. 

app :: ST a -> State -> (a, State)
app (S st) s = st s --unwraps function from state transformer and applies it to a given state

fresh :: ST int
fresh = S(\n->(n, n+1))
```



## Monads IV : Generics, Laws, and Benefits
### Generic functions
Can define functions which work for any monad.
```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map xs
```
Can define map to bring list 

```haskell
mapM :: Monad m => (a -> m b) -> [a] -> m[b]
mapM f [] = return []
mapM f (x:xs) = do y <- f x
			       ys <- mapM xs
			       return(y:ys)
```

``` haskell
conv :: Char -> Maybe Int
conv c | isDigit c = Just (digitToInt c)
	   | otherwise = Nothing
```

```haskell
convl :: [Char] -> Maybe [Int]
convl cs = mapM conv cs

e.g.,
> mapM conv "1234"
Just [1,2,3,4]
> mapM conv "123a"
Nothing
```

Generalising concat to monadic context(s).
```haskell
concat :: [[a]} ->[a]]
concat xss = [x | xs <- xss, x <- xs]
```

```haskell
--Works for arbuitrary monad m, for nested monadic values
join Monad m => m(m a) -> m a
join mmx = do mx <- mmx
		       x <- mx
			   return x
```
![[Pasted image 20250510224907.png]]





### Monad Laws
### Left identity
If you take a value a, place it in monadic context via return, and then immediately bind it to a function f, its the same as applying f directly.
```haskell
return a >>= f  =  f a
```
### Right identity
If you bind a monadic value $mx$ to $return$, it doesn't change the monadic value, simply unpacking the value and then repacking it. Thus identity.
```haskell
mx >>= return  =  mx
```
![[Pasted image 20250511001940.png]]
### Associativity
The order in which you chain binds shouldn't matter.
```haskell
(mx >>= f) >>= g  =  mx >>= (\x -> (f x >>= g))
```
For example:
```haskell
mx = Just 3

f x = if x > 0 then Just (x * 2) else Nothing
g y = if y < 10 then Just (y + 1) else Nothing

--LHS
-- Step 1: mx >>= f
Just 3 >>= f
=> f 3
=> Just (3 * 2) = Just 6

-- Step 2: result >>= g
Just 6 >>= g
=> g 6
=> Just (6 + 1) = Just 7


--RHS
Just 3 >>= (\x -> f x >>= g)
=> (\x -> f x >>= g) 3
=> f 3 >>= g
=> Just 6 >>= g
=> g 6
=> Just 7
```

```haskell
(mx >>= f) >>= g
≠
mx >>= (f >>= g) 
```
because $f :: a \to m \ b$, not type $m \ b$ purely, so type error, must write RHS like above.

### Effectful programming
Know that $a \to b$, as pure language, simply receives a member of type $a$, performs computation on that $a$ and returns it as a member of arbitrary type $b$.
```haskell
a -> b
a -> m b
```
In the context of monad $m$ is injected into the result of this function, which describes an additional effect. 

#### Examples
$a \to Maybe \ b$ : Effect = Exceptions, failure or one success.
$a \to ST \ b$ : Effect = Internally threading state
$a \to [b]$ : Effect = Non-determinism, computation could return more than one result, 0, 1 or many results.
$a \to IO$ : Effect = Input/Output


#### Reasons why the laws are useful and monads are useful
1. Supports pure programming with effects - fundamentally Haskell is a pure programming language, but need to have some effects to be a useful language, and monadic machinery does this while preserving pure functional semantics.
2. The use of monads, and therefore effects, is obvious in types. E.g., $a \to [b]$ instantly indicates non-determinism, and the lack thereof indicates the program has no effects.
3. Enabling abstraction and reuse - because any type that implements the monad interface obeys the same laws and will thus behave predictably, generic libraries and functions can be written that work uniformly regardless of the monadic context.
4. Provide general interface to structure computation.
5. Laws guarantee that monadic code behave predictably, e.g., via left identity, can freely introduce or eliminate returns in refctorings without changing behaviour, and via right identity, can use to flatten unnecessary layers of binding, and via associativity, can restructure order of computation knowing that it will produce the same output, given the same input. 



NEED TO BE ABLE TO DO THIS IN EXAM, TRACK THROUGH TYPES AND MAKE TYPE INSTANCE OF MONAD TYPECLASS

![[Pasted image 20250511010019.png]]
![[Pasted image 20250511010107.png]]
Mapping f over the contents of add via the bind operator, and then wrapping back up into add constructor to satisfy the Expr b type.

![[Pasted image 20250511010618.png]]
