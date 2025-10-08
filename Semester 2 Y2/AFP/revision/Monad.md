> A ==**monad**== ==subsumes Applicatives,== but ==adds the ability== to ==choose subsequent computations==, ==based on previously computed results==. It ==captures sequential==, ==**dependent** computations==, and ==applies functions== that ==return a wrapped value==, ==to a wrapped value==, r==ather than just permitted application== of ==pure functions to a context.==
> 
## Monad Typeclass 
```haskell
class (Applicative m) monad m where
	return :: a -> m a
	(>>=) :: m a -> (a -> m b) -> m b
```

`return` ==acts the same as pure==, simply ==lifts a value into the monadic context.==

`(>>=)` ==Takes a monadic value== `m a`, and ==a function that takes a plain `a`== and ==returns a new monadic value== by ==applying the function,== producing `m b`

==Every **monad**== is also an ==**applicative**== and a ==**functor**,== and must ==satisfy the **monad typeclass**==, and the ==**monad laws**== for the ==**monad to be sane.**== 


## Monad Laws, Generics, Benefits 
### Generics
==Using the constraint== of the ==monad typeclass,== we are ==free to define== functions which ==work for any monadic type.==

For example, we may wish to ==generalise map for list==, to all monadic types, like so:

```haskell
mapM :: Monad m => (a -> m b) -> [a] -> m[b]
mapM f [] = []
mapM f (x:xs) = do
	y <- f x
	ys <- mapM xs
	return (y:ys)
```

Generalising concat to monadic context(s).
```haskell
concat :: [[a]} ->[a]]
concat xss = [x | xs <- xss, x <- xs]
```

Flattens nested m(m a) into an m a, which can satisfy type constraints in some sitatuions and leads to more idiomatic code.
```haskell
--Works for arbuitrary monad m, for nested monadic values
join Monad m => m(m a) -> m a
join mmx = do mx <- mmx
		       x <- mx
			   return x
```

### Laws
#### Left Identity - ==Identity of function application==
If you take a value,and place it in monadic context via return, and then immediately bind it to a function f, it is the same as applying f directly
```haskell
**return a >>= f = f a**
```
#### Right Identity - Identity of monadic type
==If you bind a monadic value to return, it doesn't change the monadic value, simply unpacks the monadic value and repacks it, thus identity.==
```haskell
mx >>= return = mx
```
![[Pasted image 20250511001940.png]]
#### Associativity of bind
==The order in which binds are chained, in a sequence of dependent monadic computations, do not matter.==
==That is:==
```haskell
(mx >>= f) >>= g = mx >>=(\x->(f x >>= g))
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
because $f :: a \to m \ b$, not type $m \ b$ purely, so type error, must write RHS like above.]

### Effectful programming
Know that $a \to b$ as pure language, simply receives member of type $a$ and transforms into a member of type $b$ with no other side effects.  ==In the context of a monad, $m$ is injected into the result of a pure function,== which ==describes an additional effect.== I.e., the application itself has an effect, not just that the arguments themselves are effectful.

Each monad represents different effects

**For example**:
$a -> Maybe \ b$ represents exceptions, and handles automatic propagation of failure, or represents success
$a->[b]$ represents non-determinism, can have many results, flattened via the list monad, or can have none, the appearance of the empty list is used to discard a partial set of results in a similar manner to the Maybe Monad
$a -> ST \ b$ represents internally threading some state, which is used by stateful computations to determine resultant state, but hides this effect from the programmer. 


### Reasons why laws and monads are useful
1. S==upports pure programming with effects==
	- Haskell is a pure programming language, monadic machinery permits side effects whilst preserving the pure functional semantics of the language.
2. ==The use of monads, and therefore effects, is immediately obvious in types== e.g., $a->Maybe b$ represents this operation may fail, and the lack thereof indicates no effects.
3. ==The satisfaction of monad laws:==
	- ==Guarantee that monads behave sanely and predictably, e.g., left identity, can freely introduce or eliminate returns in refactorings without changing behaviour, and right identity can be used to flatten unnecessary layers of binding. Associativity of binding means can restructure computations to give way to more idiomatic code.==
4. ==Abstraction and reuse, e.g., join, mapM, respects monad interface and laws, can define general functions, regardless of concrete monadic context.==


