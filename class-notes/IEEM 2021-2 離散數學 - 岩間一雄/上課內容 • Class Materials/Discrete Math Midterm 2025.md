1. Step-by-Step Solution for Problem 1

**Question:** Obtain \(3^{33} \bmod 7\).

This problem tests your knowledge of **Chapter 4: Number Theory**. You can solve it using two different methods: a fast shortcut theorem or a manual pattern method.

Method A: Using Fermat's Little Theorem (The Professional Shortcut)

1. **Check the conditions:** Fermat's Little Theorem states that if p is a prime number and a is an integer not divisible by p, then:  
    \(a^{p-1}\equiv 1\mathinner{\;\left(\mod \,p\right)}\)
2. **Apply your numbers:** Here, our base a = 3 and our modulus prime p = 7. Therefore:  
    \(3^{7-1}\equiv 3^{6}\equiv 1\mathinner{\;\left(\mod \,7\right)}\)
3. **Break down the large exponent (33):** Divide 33 by 6 to see how many full cycles fit inside it.  
    \(33=6\times 5+3\)
4. **Rewrite the expression:**  
    \(3^{33}=(3^{6})^{5}\times 3^{3}\)
5. **Substitute the modulo shortcut:** Since \(3^6 \equiv 1\), substitute 1 into the equation:  
    \(3^{33}\equiv (1)^{5}\times 3^{3}\mathinner{\;\left(\mod \,7\right)}\)  
    \(3^{33}\equiv 1\times 27\mathinner{\;\left(\mod \,7\right)}\)
6. **Find the final remainder:** Divide 27 by 7. Since 7 × 3 = 21, the remainder is 27 - 21 = 6.  
    **Answer:** **6**

Method B: Finding a Manual Pattern (If you forget the theorem)

If you don't remember the theorem, just look for a repeating pattern in successive powers of 3 modulo 7:

- \(3^1 \bmod 7 = \mathbf{3}\)
- \(3^2 \bmod 7 = 9 \bmod 7 = \mathbf{2}\)
- \(3^3 \bmod 7 = (3^2 \times 3) \bmod 7 = (2 \times 3) \bmod 7 = \mathbf{6}\)
- \(3^4 \bmod 7 = (3^3 \times 3) \bmod 7 = (6 \times 3) \bmod 7 = 18 \bmod 7 = \mathbf{4}\)
- \(3^5 \bmod 7 = (3^4 \times 3) \bmod 7 = (4 \times 3) \bmod 7 = 12 \bmod 7 = \mathbf{2}\)
- \(3^6 \bmod 7 = (3^5 \times 3) \bmod 7 = (2 \times 3) \bmod 7 = 6 \bmod 7 = \mathbf{4}\) ... Wait, let's recalculate carefully:
    - \(3^1 = 3 \equiv \mathbf{3}\)
    - \(3^2 = 9 \equiv \mathbf{2}\)
    - \(3^3 = 6 \equiv \mathbf{6}\)
    - \(3^4 = 18 \equiv \mathbf{4}\)
    - \(3^5 = 12 \equiv \mathbf{5}\)
    - \(3^6 = 15 \equiv 15 \bmod 7 = \mathbf{1}\) (Pattern resets!)

The pattern length is 6 (3, 2, 6, 4, 5, 1). Because \(33 \bmod 6 = 3\), the remainder matches the 3rd item in the sequence.  
**Answer:** **6**

---

2. Step-by-Step Solution for Problem 2

**Question:** Let \(x = (a \bmod b) \bmod c\) and \(y = a \bmod c\).  
(1) Obtain specific values for a, b, c ≥ 2 (all different and not too small) such that x = y.  
(2) Obtain a necessary and sufficient condition on b ≥ 2 and c ≥ 2, such that x = y for any positive integer a.

This problem tests your conceptual understanding of how the remainder operation behaves under nested layers (**Chapter 4**).

Part (1): Finding a working example where x = y

Let's pick values where the inner mod operation doesn't disrupt anything.

- Let's pick b = 10 and c = 5. Notice that c divides cleanly into b.
- Let's pick a random large integer for a, say a = 12.

Let's test if x = y:

1. **Calculate x:**
    - Step 1: \((a \bmod b) = 12 \bmod 10 = 2\)
    - Step 2: \(x = 2 \bmod c = 2 \bmod 5 = \mathbf{2}\)
2. **Calculate y:**
    - \(y = a \bmod c = 12 \bmod 5 = \mathbf{2}\)

Since 2 = 2, our example works.  
**Answer (1):** **a = 12, b = 10, c = 5** (or any set where b is a multiple of c and a > b).

Part (2): Finding the condition for x = y to _always_ be true

For \((a \bmod b) \bmod c = a \bmod c\) to hold true for _every single possible choice_ of a, the boundaries of the remainder blocks must align perfectly.

The necessary and sufficient condition is that **b must be a multiple of c** (written mathematically as **\(c \mid b\)** or **b = k ⋅ c** for some integer k).

**Justification/Proof Outline:**

- **Sufficiency (Why it works if b is a multiple of c):** By definition, any integer a can be written as a = q ⋅ b + r, where \(r = a \bmod b\). If b = k ⋅ c, then substituting it gives \(a = q \cdot (k \cdot c) + r = (q \cdot k) \cdot c + r\). Taking everything modulo c cancels out the first term entirely, meaning \(a \bmod c = r \bmod c\). This perfectly matches our definition of x.
- **Necessity (Why it fails if b is not a multiple of c):** If b is not a multiple of c, we can easily construct a counterexample. Let a = b.
    - \(x = (b \bmod b) \bmod c = 0 \bmod c = 0\).
    - \(y = b \bmod c \neq 0\) (since b is not a multiple of c).
    - Thus, x ≠ y, proving the condition is strictly required.

---

3. Step-by-Step Solution for Problem 3

**Question:** Prove, by Math Induction, that any integer (≥ 2) is a prime or a product of primes.

This requires a **Strong Mathematical Induction** proof template from **Chapter 5**. Standard induction fails here because knowing that k is a product of primes doesn't tell us anything useful about k+1.

- **Step 1: State the Proposition.**  
    Let P(n) be the statement: "The integer n is a prime or a product of primes." We want to show P(n) is true for all integers n ≥ 2.
- **Step 2: Base Case (n = 2).**  
    For n = 2, 2 is a prime number. Therefore, P(2) is true.
- **Step 3: Inductive Hypothesis.**  
    Assume that \(P(2), P(3), \dots, P(k)\) are all true for some integer k ≥ 2. That means every integer from 2 up to k is either a prime or can be written as a product of primes.
- **Step 4: Inductive Step (Prove P(k+1) is true).**  
    We need to evaluate the next integer, k+1. There are exactly two logical possibilities for the number k+1:
    - **Case 1: k+1 is a prime number.**  
        If it is prime, then it satisfies the statement immediately. Thus, P(k+1) is true.
    - **Case 2: k+1 is a composite number.**  
        By definition, a composite number can be factored into two smaller integers, let's call them a and b, such that:  
        \(k+1=a\times b\quad \text{where\ }2\le a\le k\text{\ and\ }2\le b\le k\)  
        Because both a and b are less than or equal to k, **they are covered by our Inductive Hypothesis**. This means a and b are both either prime or a product of primes.  
        When you multiply two sets of prime factors together (a × b), the resulting number (k+1) is automatically a product of primes. Thus, P(k+1) is true.
- **Conclusion:** By the principle of strong mathematical induction, P(n) is true for all integers n ≥ 2. \(\blacksquare \)

---

Would you like to continue walking through the remaining two problems (**Question 4 on Logic** and **Question 5 on Graph Theory/Geometry**)? Let me know if you want to tackle those next!

Ready to create a study guide?

Use Canvas to save, edit, and share your guide

Get started

yes continue

4. Step-by-Step Solution for Problem 4

**Question:** We have three pigeons, \(a, b,\) and \(c\), and two pigeonholes \(1\) and \(2\). If each pigeonhole can accept at most one pigeon, then we obviously cannot put all the pigeons into the holes. Prove this by using logic.

This question tests your ability to take an intuitively obvious concept and formalize it using **Chapter 1: Propositional Logic** and a **Proof by Contradiction**.

Step 1: Define the Propositions (Variables)

Let's represent the placement of each pigeon as logical variables. Let \(P_{ij}\) mean **"Pigeon \(i\) is placed in hole \(j\)"**:

- For pigeon \(a\): \(P_{a1}\) (in hole 1), \(P_{a2}\) (in hole 2)
- For pigeon \(b\): \(P_{b1}\) (in hole 1), \(P_{b2}\) (in hole 2)
- For pigeon \(c\): \(P_{c1}\) (in hole 1), \(P_{c2}\) (in hole 2)

Step 2: Translate the Given Premises into Logic Formulas

1. **Premise 1: Every pigeon must be placed in at least one hole.**
    - \(a\) is housed: \((P_{a1} \lor P_{a2})\)
    - \(b\) is housed: \((P_{b1} \lor P_{b2})\)
    - \(c\) is housed: \((P_{c1} \lor P_{c2})\)
    - _Total Setup Clause (\(S\)):_ \((P_{a1} \lor P_{a2}) \land (P_{b1} \lor P_{b2}) \land (P_{c1} \lor P_{c2})\)
2. **Premise 2: Each pigeonhole can accept at most one pigeon.**  
    This means no two distinct pigeons can share a hole.
    - For Hole 1: \(\neg(P_{a1} \land P_{b1}) \land \neg(P_{a1} \land P_{c1}) \land \neg(P_{b1} \land P_{c1})\)
    - For Hole 2: \(\neg(P_{a2} \land P_{b2}) \land \neg(P_{a2} \land P_{c2}) \land \neg(P_{b2} \land P_{c2})\)
    - _Total Capacity Clause (\(C\)):_ Both condition sets above must hold true simultaneously.

Step 3: Perform the Proof by Contradiction

Assume that it **is** possible to house all three pigeons under these constraints.

1. From Premise 1, all three pigeons are assigned holes. Since there are 3 pigeons and only 2 holes, we can count the distribution using logical cases.
2. Look at the assignments for Hole 1. How many pigeons can be in it?
    - If **two or more** variables from \(\{P_{a1}, P_{b1}, P_{c1}\}\) are true, Premise 2 (Capacity Clause for Hole 1) is immediately violated, yielding a contradiction (\(\text{True} \land \text{False} \equiv \text{False}\)).
    - Therefore, **at most one** pigeon can be in Hole 1.
3. Apply the exact same logic to Hole 2: **at most one** pigeon can be in Hole 2.
4. If Hole 1 has at most 1 pigeon, and Hole 2 has at most 1 pigeon, the maximum number of true variables across all assignments is \(1 + 1 = 2\).
5. However, Premise 1 requires that at least 3 distinct variables (one for each unique pigeon letter) must be true to house all pigeons.
6. This yields a direct logical contradiction: we require at least 3 true variables, but our constraints limit us to a maximum of 2 true variables (\(3 \le 2 \equiv \text{False}\)).

**Conclusion:** Our initial assumption is false. It is logically impossible to place all three pigeons into the two holes under these rules. \(\blacksquare \)

---

5. Step-by-Step Solution for Problem 5

**Question:** Prove that for any \(n \ge 1\) and any \(1 \le k \le n\), there is a \(3n\)-node polygon for which \(k\)-guards are necessary and sufficient.

This question tests **Chapter 10: Graphs** and geometric modeling. It is based on the **Art Gallery Theorem**, which states that any simple polygon with \(V\) vertices can be fully monitored by at most \(\lfloor V/3 \rfloor\) guards. Here, our total number of vertices is \(V = 3n\), so the maximum upper bound of guards required is \(\lfloor 3n/3 \rfloor = n\).

The problem asks you to constructively prove that you can build a custom shape for **any** value of \(k\) between \(1\) and \(n\) where _exactly_ \(k\) guards are required.

Step 1: Understand the Visual Clues in the Exam

Look closely at the drawings provided at the bottom of your exam sheet:

- The first diagram shows a standard triangle structure or "comb prong".
- The second diagram shows a transformation step (indicated by the \(\implies \) arrow) where a sharp prong is collapsed flat into a dull, visible wall.

Step 2: Construct the Base Object (The "Comb" Component)

To force a shape to require a guard, we create a sharp, isolated triangular alcove (a "prong").

- A single prong consists of 3 vertices forming a deep pocket.
- If a guard is placed outside this pocket, they cannot look deep down inside it because the doorway corners block their line of sight.
- Therefore, to watch a sharp prong, **1 guard must be dedicated to it**.
- If we chain \(n\) of these sharp prongs together, we get a standard **Chvátal Comb** with \(3n\) vertices. This base shape absolutely requires \(n\) guards.

Step 3: Construct the Shape for a Specific \(k\)

To prove the theorem for _any_ arbitrary target number of guards \(k\) (where \(1 \le k \le n\)):

1. **Build \(k\) Sharp Prongs:** Construct \(k\) distinct, deep, sharp prongs along the top perimeter of your polygon. Since each sharp prong is hidden from the others, each one strictly demands its own unique guard. This accounts for \(k\) guards.
2. **Flatten the Remaining \((n - k)\) Prongs:** You still need to ensure your total vertex count equals exactly \(3n\). For the remaining parts of the polygon, instead of making sharp pockets, you use the visual transformation shown in the exam's hint: flatten the points out into broad, obtuse triangles along a flat, open wall.
3. **Show Sufficiency (Why \(k\) guards can see everything):** The guards standing in the \(k\) sharp pockets have an unobstructed line of sight out into the main hallway of the gallery. Because the remaining \((n-k)\) sections are flattened out along this open wall, those same \(k\) guards can easily look across and see all of those flat vertices without needing any extra help.
4. **Show Necessity (Why fewer than \(k\) guards won't work):** Because there are still \(k\) distinct, isolated sharp pockets completely hidden from each other, hiring \(k-1\) guards will always leave at least one pocket completely unmonitored.

Thus, you have constructed a valid \(3n\)-node polygon where exactly \(k\) guards are both necessary and sufficient. \(\blacksquare \)