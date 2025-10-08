> Applicatives are a ==stronger abstraction== that a functor - they allow us to ==apply functions== that ==themselves== are ==within the contex==t of a ==parameterised type==, and ==apply functions== that ==aren't just unary== to ==parameterised types.==

==Without the use of applicatives,== a ==number of fmaps==, ==technically infinite==, would be ==neede==d to ==bypass the constraints== of the f==unctorial return==/acceptance type.

```haskell
fmap0 :: a -> f a
fmap1 :: (a -> b) -> f a -> f b
fmap2 :: (a -> b -> c) -> f a -> f b
fmap3 :: (a -> b -> c -> d) -> f a -> f b -> f c -> fd
```

The cleaner way to achieve this result is to ==adjust the type signature,== leaving us with a ==more powerful abstraction.== 


## Applicative Typeclass
```haskell
class (functor f) f => Applicative A where
	pure :: a -> A a
	<*> ::  A (a->b) -> A a -> A b
```
==All applicatives are functors, and must be applicatives before being functors.==

`Pure` ==lifts a value into the applicative context, simply satisfying the parameterised type that we wish to make an instance of the applicative typeclass.==

`<*>` ==is a generalised form of function application - it takes  a function wrapped in the parameterised type, with type `a->b`, it takes a parameterised type which contains arguments of type `a`, and returns a parameterised type which contains arguments of type `b`.==


Writing programs using these operatos is known as an ==applicative style== of programming, in which functions are lifted into the context of an applicative, and applied to effectful arguments, ==permitted a reasonable degree of effectful programming==.

An effectful argument is an argument not solely consisting of a concrete value, but wrapped in some additional context. ==By permitting partial application, and multi-arg functions, applicatives give the first real look at writing pure programs that have side effects.== 


## Why use Applicatives
- ==Sequence independent effects== - unlike monads, the shape of effects/which computations run is determined statically, and is not dependent on earlier results, whereas a Monad allows you to choose which computation to run next based on previous value.
- ==Parallelisable== - each arguments effect is independent from the next in any given chain of generalised function applications, thus, applicative structures and style of code can be evaluated in parallel.
- ==Can define functions, similar to functor, that work for any type that satisfies the applicative typeclass.== 
	```haskell
	plus :: Applicative f => f int -> f int -> f int
	plus x y = pure (+) <*> x <*> y
	```
## Why not use applicatives
- When you want to be able to skip or change the shape, of effects, according to earlier results. ==When code is less predictable== than a ==pure program may suggest==, then a ==monad may be better off== being used than an applicative, which is also an instance of the applicative typeclass. 
- When we wish to ==apply a function== that ==returns a wrapped value,== ==rather than a pure one== which is wrapped in context of the applicative e.g., safediv, takes 2 ints, returns a maybe, is not pure, and returns in the context of the Maybe Monad. 