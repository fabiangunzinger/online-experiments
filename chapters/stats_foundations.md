# Statistics foundations

## Combination vs Permutation

**Permutation (Order Matters)**

If we have $n$ items and we want to pick $r$ in a specific order, the formula is:  

$$
P(n, r) = \frac{n!}{(n - r)!}
$$
Example:
Arranging 3 letters (A, B, C) in 2 positions.  
- Possible orders: AB, AC, BA, BC, CA, CB → 6 ways  
- Formula:  
  $$
  P(3,2) = \frac{3!}{(3 - 2)!} = \frac{3!}{1!} = \frac{3 \times 2 \times 1}{1} = 6
  $$

**Combination (Order Doesn't Matter)**

If we have $n$ items and we want to pick $r$, but order does not matter, the formula is:

$$
C(n, r) = \frac{n!}{r!(n - r)!}
$$

Example:

Choosing 2 letters from (A, B, C), where order does not matter.  
- Possible groups: {A, B}, {A, C}, {B, C} → 3 ways  
- Formula:  
  $$
  C(3,2) = \frac{3!}{2!(3 - 2)!} = \frac{3!}{(2! \times 1!)} = \frac{3 \times 2 \times 1}{(2 \times 1) \times 1} = 3
  $$

**Key Difference:**
- **Permutation:** ABC and BAC are different  
- **Combination:** ABC and BAC are the same  

**Shortcut:**  
$$
P(n, r) = C(n, r) \times r!
$$  
(Since for every combination, there are $r!$ ways to arrange it.)
