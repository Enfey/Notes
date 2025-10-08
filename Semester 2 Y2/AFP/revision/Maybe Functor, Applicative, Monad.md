## Functor Definition and Examples
```haskell
instance Functor Maybe where
	fmap f Nothing = Nothing
	fmap f Just a = Just (f a)
```
Functor maybe type. Intuition, maps a function over the underlying value in maybe, and returns a result in the context of the maybe type.
If there is nothing to transform via the pure function, i.e., nothing, then Nothing is simply returned. This allows us to apply a pure function to the parametersied maybe type. 
## Applicative Definition and Examples
```haskell
class (Functor f) => Applicative f where
	pure :: a -> f a
	(<*>) :: f(a->b)-> f a -> f b
```

```haskell
instance Applicative Maybe where
	pure x = Just x
	(Just f) <*> Nothing = Nothing
	(Just f) <*> Just x = Just (f x)
```
Applicative for maybe type. Intuition, allows us to apply multi-argument functions to multiple maybe arguments whilst allowing us to apply a function wrapped in the context of the maybe type to a value wrapped in the context of the maybe type. If have nothing, nothing to transform. Nothing propagates through, same as with monad, as such sequences of computations can be constructed, but they cannot wholly depend on values or guards from earlier computations. 

## Examples
```haskell
> pure (+) <*> Just 1 <*> Just 2
> Just 3
```
Lifts + into monadic context, partially applies to 1 to get Just(+1) and then applies this to get Just(2+1), which evaluates to Just 3.
## Monad definition and Examples, AND MOTIVATION.
```haskell
class (Applicative f) => Monad m where
	return :: a -> m a
	(>>=) :: m a -> (a -> m b) -> m b
```

```haskell
instance Monad Maybe where
	return = pure
	Nothing >>= f = Nothing
	Just x >>= f = f x
```
Monad for maybe type. Intuition, allows us to apply an impure function, that is, one that returns into the type of maybe, to a value wrapped in the maybe context, returning an m b. Abstracts the common pattern - that is the applicative style fails, and exhaustive pattern matching and forwarding of results based on whether a computation succeeds or not. 




### Examples:

## Example 1
```haskell
safediv :: Int -> Int -> Maybe Int
safediv _ 0 = Nothing
safediv (x y) = Just(x `div` y)

eval :: Expr -> Maybe Int
eval (Val n) = Just n
eval (Div x y) = eval x >>= \n -> eval y >>= \m -> safediv n m


--OR

eval (Div x y) = do 
	n <- eval x
	m <- eval y
	safediv n m
```
Wouldnt work for applicative due to safediv, if a nothing was received at any point from eval, would just chain through, and become nothing overall due to bind definition. 


Anything that chains togeher maybes, and a final function that returns a maybe.

```
case f a0 of
  Nothing  -> Nothing
  Just x1  -> case g x1 of
                Nothing -> Nothing
                Just x2 -> case h x2 of
                             Nothing -> Nothing
                             Just x3 -> Just (x1, x2, x3)
```

Anything with nested cases, checking what result is, then doing next bit can be cleanly rewritten as
```haskell
do x <- f a0
do y <- g x
do z <- h y
return (x, y, z)
```

Nothing propagated through

## Example 2

-- | Safe integer square root: Nothing if input < 0
safeSqrt :: Int -> Maybe Int
safeSqrt n
  | n < 0     = Nothing
  | otherwise = Just (floor (sqrt (fromIntegral n)))

-- | Compute square root of (a ÷ b), failing cleanly if b=0 or a/b < 0
computeRoot :: Int -> Int -> Maybe Int
computeRoot a b = do
  quotient <- safeDiv a b
  safeSqrt quotien
  

Maybe essay - monad definition, what it represents, example program, what programmer sees, encapsulates exceptions, propagates through failure via definition of bind operator. 
