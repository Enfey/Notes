## Functor Definition and Examples
```haskell
ST a = S(State -> (a, State))

app :: ST a -> State -> (a, State)
app (S st) s = st s

instance Functor ST where
	-- (a->b) -> ST a -> ST b
	fmap f (S st) = S(\s -> let (x, s') = app st s in (f x, 
	s') )
```

Applies stateful computation to the current state, returning a type of a and then applies the function, and wraps this back up in a stateful computation, without having to rewrite plumbing. Functor is only useful if you only need to take one stateful computation and apply a pure transformation/function to its final result - it is too lightweight to chain multiple stateful steps.
## Applicative Definition and Examples
```haskell
ST a = S(State -> (a, State))

app :: ST a -> State -> (a, State)
app (S st) s = st s

class (Functor f) => Applicative f where
	pure :: f a
	(<*>) :: f(a->b) -> f a -> f b

instance Applicative ST where
	-- :: f a
	pure a = S(\s->(a, s))
	-- ST(a->b) -> ST a -> ST b
		(S stf) <*> (S stx) = S(\s0 -> let (f, s1) = app stf s0,   
		let (x, s2) = app st s' in (f x, s2)
``` 

We run the stateful computation on the initial state to yield the inner function and the new state, and then run the additional stateful computation on the updated state. We then apply the function, and wrap this up the the state transformer, threading the state in the applicative throughout these computations. There is no dependent sequencing here, except on the state. Allows you to apply pure functions that are wrapped in a stateful computation in sequence, whilst threading the thread transparently, whilst also being potentially able to parallelise the computaations. 
```
pure f
  <*> st1
  <*> st2
  <*> st3
```
Can now chain these together - limited initially by the type of the functor typeclass, can now thread independent stateful computations together. 

can't adapt depending on result of one action, must define upfront, or embed further parameterised types e.g., propagating nothing, making code more complex. 


## Monad definition and Examples
```haskell
ST a = S(State -> (a, State))

app :: ST a -> State -> (a, State)
app (S st) s = st s

class (Applicative f) => Monad m where
	return :: m a
	(>>=) :: m a -> (a -> m b) -> m b

instance Monad ST where
	return = pure
	(S St) >>= f = S(\s-> let (x, s') = app st s, in app (f x) s'
```

The ST monad is an expressive way to chain stateful computations such that a future stateful computation can depend on the result of an earlier one. 

### Examples
#### Example 1
![[Pasted image 20250528180056.png]]

#### Example 2


#### Example 3