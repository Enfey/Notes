## Pure
A language is called pure if the programs are mathematical functions. They take all their input, and transform it into output, and this is their sole intention. There are no side effects like in impure programs, where things such as I/O, global state and exceptions.

However, if want to write real programs, have to interact with outside world. Need benefits of 2 approaches. 

### Abstracting programming patterns
```haskell
inc :: [Int] -> [Int]
inc [] = []
inc (n:ns) = n + 1 : inc ns

sqr :: [Int] -> [Int]
sqr [] = []
sqr (n:ns) = n^2 : sqr ns
```
The only difference in these 2 function definitions is the function applied to each list item. Pattern of applying function to all items in a ***list*** can be abstracted into a higher-order function.

```haskell
map :: (a -> b) -> [a] -> [b]
map f [] = []
map f (x:xs) = f x : map f xs
```
Can now define such functions in a much more succinct manner.

```haskell
inc = map (+1)
sqr = map(^2)
```

### Generalising further
The mapping of a function over each element of a data structure isn't specific to the type of lists - this can be generalised further to a range of parameterised types.

```haskell
class Functor f where 
	fmap :: (a -> b) -> f a -> f b

```
This generalises the idea of mapping to any parameterised type/structure f: which is an instance of the functor class if it implements the fmap function. 

For lists to be a functor, this would simply require: 
```haskell
instance Functor [] where 
-- fmap :: (a -> b) -> [a] -> [b]
fmap = map
-- empty list overloaded here, really means list applied to parameterised type a and b
```

Maybe functor 
```haskell
data Maybe a = Nothing | Just a 

instance Functor Maybe where 
-- fmap (a -> b) -> Maybe a -> Maybe b
fmap f Nothing = Nothing 
fmap f (just a) = Just (f a)
```

Tree functor
```haskell
data Tree a = Leaf a | Node (Tree a) (Tree a)

instance Functor Tree where
-- fmap (a -> b) -> Tree a -> Tree b
fmap f (Leaf x) = Leaf (f x)
fmap f (Node L R)  = Node (fmap f L) (fmap f R)


fmap even (Node(Leaf 1)(Leaf 2))
-- Node (Leaf False) (Leaf True)
```

### Why use functors?
1. Able to use the same function name, fmap, for functions that are essentially the same, creating a universally recognised programming pattern for parameterised types that indicates intention immediately.
2. Functors give us the opportunity to define generic functions that work for any functorial type.
	```haskell
	inc :: Functor f => f int -> f int
	inc = fmap (+1)
	```

A functor will only work sanely if the following functor laws hold:
```haskell
fmap id = fmap
fmap (g . f) = fmap g . fmap f
```


### Why not use functors
While allowing us to apply functions to parameterised types which wrap concrete values in some context, their definition does not, by default permit functions which 'take more than 1 argument.'

We could not do for example 
```haskell
class Functor f where 
	fmap :: (a -> b) -> f a -> f b

data Maybe a = Nothing | Just a 

instance Functor Maybe where 
-- fmap (a -> b) -> Maybe a -> Maybe b
fmap f Nothing = Nothing 
fmap f (Just a) = Just (f a)

> Just 1 + Just 2
-- See below
```
due to the return type of fmap for just values, the partially applied function returned from `(+) Just 1` will no longer be a pure function, it would be wrapped in the maybe type, lifted into the context of the functor. `Just ((+) Just 1)` and so cannot be applied to Just 2, due to type mismatch. To be able to take multiple arguments, functors would need to have multiple fmaps, fmap0...fmapn. This is where the use of [[Applicatives]] is of use. 