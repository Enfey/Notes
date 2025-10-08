> A ==**functor** represents a type== that can be ==*mapped* over.==

Means we have ==some context `f a`== and want to ==apply a pure function to it.==

## Pure
==A language is called **pure**== if the ==programs are constructed using mathematical functions==. ==Take all input==, ==produce output==, this is sole intention, ==zero side effects==. But ==not useful== for **real programs.**


## Functor
**A ==functor== allows us to ==apply a pure function== `a->b` to a ==context `f a`== and ==return the result== ==wrapped in the context==, i.e., `f b`, ==not altering the context whatsoever.==** **It ==represents a parameterised type==, that can have its contents mapped over, without altering the context.**

### Abstracting programming patterns
```haskell
inc :: [Int] -> [Int]
inc [] = []
inc (n:ns) = n + 1 : inc ns

sqr :: [Int] -> [Int]
sqr [] = []
sqr (n:ns) = n^2 : sqr ns
```
==Only difference here==, is the ==function applied to each item==. The ==context is removed via pattern matching==, but there is a ==clear pattern of applying function to all items in a context,== and this can be abstracted to a ==***functor for lists***==. [[List functor, applicative, monad]].

==Generalising this pattern== gives rise the the ==***functor*** class==, which ==generalises this idea of mapping something over the discrete elements== of a ==parameterised type==/structure.

```haskell
class Functor f where 
	fmap :: (a -> b) -> f a -> f b
```


## Why use Functors 
1. ==Able to use the same function name `fmap`, for functions that are essentially the same. Creates universally recognised programming pattern for parameterised types that immediately indicates intent.==
2. ==Functors give us the opportunity to define functions that work with any functorial type, by adding a typeclass restriction in the type declaration of the function. For example==
	``` haskell
	inc :: Functor f => f int -> f int
	inc = fmap (+1)

	digInt :: Functor f => f char -> f int
	digInt = fmap(digitToInt)
    ```
## Why not use Functors
While a functor ==allows us== to ==apply **pure functions**== to ==parameterised types==, without altering the context, the type signature of a functor ==forbids us from being able to apply curried functions,== that is, functions that take more than one argument. ==Application of such a function, would return a partially applied function==, that is ==now in the context of the functorial type==, ==rather than being pure==, and so cannot be applied to further arguments, thus failing. 

A stronger abstraction, like the [[Applicative]] is necessary to do this