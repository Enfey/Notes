## Implications
Implication is right associative that is
(x → (y → z)) = ((x → y) → z) fails, amongst other groupings. 
This is because the meaning of the two expressions differs. For example:

- x→(y→z) means "if x holds, then if y holds, z holds."
- (x→y)→ z means "if x->y holds as a proposition, then z holds."
- false counterxample.
## Quantifiers
How do we read ∀ x : A,PP x ∧ Q? 
(i) Is it (∀ x : A,PP x) ∧ Q or ∀ x : A,(PP x ∧ Q)?
the right, the quantifier binds tightly to everything that follows unless parentheses explicitly limit its scope. 