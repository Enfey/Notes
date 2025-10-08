 Applicatives are a stronger abstraction than a functor, which allow us to generalise the idea of mapping/function application to functions that take more than one argument to parameterised types. 

Without applicatives, a number of fmaps would be required: 
```haskell
fmap0 :: a -> f a
fmap1 :: (a -> b) -> f a -> f b
fmap2 :: (a -> b -> c) -> f a -> f b
fmap3 :: (a -> b -> c -> d) -> f a -> f b -> f c -> fd
```

### Definition
```haskell
class (Functor f) => Applicative f where
    pure  :: a -> f a
    (<*>) :: f (a -> b) -> f a -> f b
```
All applicatives are functors and must also be a functor before being an applicative.

`Pure` - lifts a value into the applicative context - this can be a function
`(<*>)` - the star operator is a generalised form of function application - takes a parameterised type which contains functions of type a to b and a parameterised type which contains arguments of type a, and returns a parameterised type which contains arguments of type b.

	Type is interchangeable for data structure here.
Only apply the function if it resides within the applicative function. 

```haskell
fmap0 :: f -> f a
fmap0 = pure
```

```haskell
fmap1 :: (a -> b) -> f a -> f b
fmap1 g x = pure g <*> x --lifts into applicative context, and then applies star operator.

-- g = a -> b
-- x = f a
-- lhs results in f 
```

Writing programs in this way is known as an applicative style of programming, in which programs are applied to effectful arguments. An effectful argument is an argument not solely consisting of a concrete value, but containing some additional context; a secondary effect of the argument. 

#### Maybe applicative
```haskell
instance Applicative Maybe where 
	pure x = Just x
	Nothing <*> mx = Nothing
	(Just f) <*> (Just x) = Just (f x)

> pure (+) <*> Just 1 <*> Just 2
> Just 3
```


### List applicative
```haskell
instance Applicative [] where
	--pure :: a -> [a]
	pure x = [x]
	-- (<*>) :: [a->b] -> [a] -> [b]
	fs <*> xs = [f x | f <- gs, x <- xs]
```
fmap applies single pure function to all elements of a list, applicative takes functions in a context, ie in a list, and applies all functions to another context, producing a result.
```haskell 
>[(*2), (+1)] <*> [1, 2, 3]  
-- results in [(*2) 1, (*2) 2, (*2) 3, (+1) 1, (+1) 2, (+1) 3]
-- which is [2, 4, 6, 2, 3, 4]
```
Get example for why multi-arg functions work with list applicative at some point.