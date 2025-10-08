## Functor Definition and Example
```haskell
class Functor f where
	fmap :: (a->b) -> f a -> f b
```

```haskell
map :: (a->b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map xs
```

```haskell
instance Functor [] where 
	fmap = map
```

Maps a pure function taking a single argument, and returning a single argument, over the parameterised type of list and all its elements.

```haskell
sqr :: [Int] -> [Int]
sqr = fmap (^2)
```

Indicates intent, know that function is applied uniformly to all values in the list, and can chain calls to fmap as obeys the functor laws, saves us from writing explicit recursion, abstracting out a common pattern. 
## Applicative Definition and Examples
```haskell
class (Functor f) => Applicative f where
	pure :: a -> f a
	<*> :: f(a->b) -> f a -> f b
```

```haskell
instance Applicative [] where
	pure a = [a]
	xs <*> fs = [f x | f <- fs, x <- xs]
```

```haskell
pure (+1) <*> [1, 2]
pure (+) <*> [1, 2] <*> [1, 2]
```
The list applicative applies a list of functions, i.e., functions within the context of the list to another list. This allows functions which take multiple arguments to be applied via partial application and parallelised.
More structure than a functor, with more freedom.
## Monad definition and Examples
```haskell
class (Applicative f) => Monad m where
	return :: a -> m a
	(>>=) :: m a -> (a -> m b) -> m b
```

```haskell
instance Monad [] where
	return = pure
	xs >>= f = concat (map f xs)
```

```haskell
do x <- xs
do y <- ys
guard (y > 10)
return (x + y)


xs >>= \x -> ys >>= \y -> if y > 10(return x+y) otherwise []
```
Represents the modelling of **non-deterministic computations**: xs [a] can be thought of as r**epresetting a computation that can produce any of the elements of xs** as possible results. 

When running bind, and applying a function to a list, we're essentially saying for each **x from xs, run f x,** which **may itself produce multiple outcomes**, and then **flatten those outcomes.** 

![[Pasted image 20250528145631.png]]
Flattens automatically, leads to declarative code, avoid explicit recursion as this is abstracted away from you. 




evenPairs' :: [Int] -> [Int] -> [(Int, Int)]
evenPairs' xs ys = do
  x <- xs
  y <- ys
  guard ((x + y) `mod` 2 == 0)
  return (x, y)


![[Pasted image 20250528145900.png]]







