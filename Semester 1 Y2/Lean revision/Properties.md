## Equality
### Reflexive
∀ x : A, x=x
==any element x== of ==type A== is ==equal to itself==. 
### Symmetric
∀ x y : A, x=y → y=x
==if x = y== then ==y = x==, showing that ==equality is bidirectional==
###  Transitive
∀ x y z : A, x=y → y=z → x=z
==if x = y and y = z then x = z.==
## Commutative
∀ x y : A, op x y = op y x
==An operation== op e.g., addition is ==commutative== if the ==order of operands== does ==not matter.== 
- Addition
- ==Multiplication==
- ==Conjunction==
- ==Disjunction==
## Associative
∀ x y z : A, op (op x y) z = op x (op y z)
An ==operation== op is ==associative== if the ==grouping== of operands does ==not affect the result==. Addition is associative.
- ==Addition==
- ==Multiplication==
- ==Conjunction==
- ==Disjunction==
## Idempotent
An ==operation== op is ==idempotent== if ==applying== it to the ==same element repeatedly== produces the ==same result.== 
- ==Conjunction==
- ==Disjunction==
## Distributive
Both ==left== and ==right distributivity== hold for ==multiplication over addition ==in the ==integers==, ==real numbers==, and ==natural numbers==:
#### Left Distributive
∀ l m n : ℕ , (m + n) * l = m * l + n * l 
Distributivity in algebra states the interaction between multiplication and addition, defining how multiplication 'distributes' over addition. 
ie that multiplying l by the sum of m+n is the same as multiplying l by m and n separately. 

#### Right Distributive
∀ l m n : ℕ , l * (m + n) = l * m + l * n 

