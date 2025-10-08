State that if you negate a disjunction or conjunction this is equivalent to the negation of their components replaced by conjunction and vice versa. 
that is: 
¬ (P ∨ Q) ↔ ¬ P ∧ ¬ Q
in lean 
```
theorem dm1 : ¬ (P ∨ Q) ↔ ¬ P ∧ ¬ Q :=
begin
  constructor,
  assume npq,
  constructor,
  assume p,
  apply npq,
  left,
  exact p,
  assume q,
  apply npq,
  right,
  exact q,
  assume npnq pq,
  cases npnq with np nq,
  cases pq with p q,
  apply np,
  exact p,
  apply nq,
  exact q,
end
```
¬ (P ∧ Q) ↔ ¬ P ∨ ¬ Q
```
theorem dm2 : ¬ (P ∧ Q) ↔ ¬ P ∨ ¬ Q :=
begin
 constructor,
 assume npq,
 left,
 assume p,
 apply npq,
 constructor,
 exact p,
 sorry,
 assume npnq pq,
 cases pq with p q,
 cases npnq with np nq,
 apply np,
 exact p,
 apply nq,
 exact q,
end
```
NEED CLASSICAL FOR THIS?