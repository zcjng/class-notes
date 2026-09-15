---
tags:
  - discrete
---

# Lecture Notes: Propositional SAT & Predicate Logic

![[IMG_20260915_110009.jpg]]
## 1. Boolean Satisfiability (SAT) via Sudoku
This lecture demonstrates **problem reduction to SAT** by converting a mini $4 \times 4$ Sudoku puzzle into Boolean logic rules so a computer solver can calculate the solution.

### A. Variable Mapping
Computers cannot interpret an empty grid, so every cell is assigned a specific base variable by row and column:
* **Row 1:** $x_1, x_2, x_3, x_4$
* **Row 2:** $y_1, y_2, y_3, y_4$
* **Row 3:** $z_1, z_2, z_3, z_4$
* **Row 4:** $w_1, w_2, w_3, w_4$

![[IMG_20260915_110020.jpg]]

Each cell is then split into **4 sub-variables** indicating its numerical value. For cell $x_1$:
* $x_{11} = \text{True/False}$ ("Cell $x_1$ contains a **1**")
* $x_{12} = \text{True/False}$ ("Cell $x_1$ contains a **2**")
* $x_{13} = \text{True/False}$ ("Cell $x_1$ contains a **3**")
* $x_{14} = \text{True/False}$ ("Cell $x_1$ contains a **4**")

### B. Defining the Constraints (Conjunctive Normal Form)
A modern SAT solver requires formulas to be structured in blocks of **OR ($\lor$)** statements joined together by **AND ($\land$)**. 

![[IMG_20260915_110027.jpg]]
#### 1. Unique Value Rule: $\text{Unique}(x_1)$
Every cell must contain *exactly one* number. This is constructed via two rules:
* **At least one number:** $(x_{11} \lor x_{12} \lor x_{13} \lor x_{14})$
* **At most one number:** Elements cannot collide. By De Morgan's Law, "NOT (1 AND 2)" becomes $(\overline{x_{11}} \lor \overline{x_{12}})$. The complete rule chains every pair combination:
  $$\text{Unique}(x_1) = (x_{11} \lor x_{12} \lor \lor \dots) \land (\overline{x_{11}} \lor \overline{x_{12}}) \land (\overline{x_{11}} \lor \overline{x_{13}}) \dots \land (\overline{x_{13}} \lor \overline{x_{14}})$$

#### 2. Layout Restrictions ($\text{row}_n$, $\text{col}_n$, $\text{sgrid}_n$)
* **$\text{row}_n$ / $\text{col}_n$:** Ensures numbers do not repeat in a line. For row 1, cell $x_1$ and $x_2$ cannot both be 1, written as $(\overline{x_{11}} \lor \overline{x_{21}})$.
* **$\text{sgrid}_n$:** Ensures numbers do not repeat inside the mini $2 \times 2$ boxes. For the top-left subgrid, cells $x_1, x_2, y_1, y_2$ cannot share a digit.

![[IMG_20260915_110152.jpg]]
### C. The Master Equation & Starting Clues
The final master formula $F$ is a massive chain of all rules joined by **AND ($\land$)**. Pre-filled hints from the puzzle board are appended directly onto the end to lock them in place:
$$F = \text{Unique}(x_1) \land \dots \land \text{row}_1 \land \dots \land \text{sgrid}_1 \dots \land (x_{22} \land y_{23} \land w_{12} \land z_{33})$$

> [!important] How the Master Checkpoint Evaluates
> Every single bracketed clause must equal **1 (True)** simultaneously. If there is even a single duplicate value or a broken starting clue anywhere in the puzzle, that specific pair bracket drops to **0**, causing the entire formula to crash to **0**. 

---

## 2. Predicate Logic & Quantifiers ($\mathbb{N} = \{0, 1, 2, 3, \dots\}$)
When scaling logic up to infinite domains (like all natural numbers $\mathbb{N}$), we utilize **quantifiers** to make structural claims.

![[IMG_20260915_110119.jpg]]
### A. The Existential Quantifier ($\exists$)
* **Meaning:** "There exists at least one element..."
* **Statement:** $\exists x \in \mathbb{N} \text{ s.t. } x^2 - 3x + 5 = 20$
* **Logical Behavior:** Acts like an infinite chain of **OR ($\lor$)** operators: $Q(0) \lor Q(1) \lor Q(2) \dots$

### B. The Universal Quantifier ($\forall$)
* **Meaning:** "For all / for every element..."
* **Statement:** $\forall x \in \mathbb{N}, x^2 + 2x + 1 = (x+1)^2$
* **Logical Behavior:** Acts like an infinite chain of **AND ($\land$)** operators: $P(0) \land P(1) \land P(2) \dots$

![[IMG_20260915_110121.jpg]]
### C. Negation and De Morgan's Law
To disprove an existential statement, you apply De Morgan's Law across the entire set:
$$\neg(\exists x \in \mathbb{N}, Q(x)) \equiv \forall x \in \mathbb{N}, \overline{Q(x)}$$
*Flipping "There is an $x$ where $Q$ is true" results in stating "**For all $x$**, $Q$ is **NOT** true."*

#### Proof by Exhaustion Example:

![[IMG_20260915_110126.jpg]]

To prove $\exists x \in \mathbb{N} \text{ s.t. } x^2 - 3x + 5 = 20$ is **False**, your professor proved its negation is True:
1. **Low Bounds:** $x=0,1,2,3$ outputs numbers well below 20 (e.g., $P(1)=3, P(3)=5$).
2. **High Bounds ($x \ge 4$):** Factoring to $x(x-3)+5$ shows strict growth that completely skips over the target value: $P(4)=9, P(5)=15, P(6)=23$.

___

## 3. Nested Quantifiers & Ordering ($\forall y \exists x$ vs $\exists x \forall y$)

![[IMG_20260915_110245.jpg]]

> [!danger] Crucial Distinction
> The positioning of quantifiers dictates dependency. You cannot swap them freely:
> $$\forall y \in \mathbb{N} (\exists x \in \mathbb{N}, Q(x,y)) \neq \exists x \in \mathbb{N} (\forall y \in \mathbb{N}, Q(x,y))$$
### Case 1: $\forall y \exists x$ (True Statement)
* **Interpretation:** "For any number $y$ you give me, I can calculate a specific, moving partner $x$ to satisfy the rule."
* **Equation:** $x^2 = y^2 + 4y + 4$
* **Algebraic Factoring:** 
  The right side simplifies to a perfect square: $y^2 + 4y + 4 = (y+2)^2$. 
  By taking the square root, we get a reliable **partner recipe**:
  $$x = y + 2$$
* Since every input $y$ can cleanly produce a whole number partner exactly two steps ahead of it ($y=1 \rightarrow x=3$; $y=10 \rightarrow x=12$), the rule holds.

### Case 2: $\exists x \forall y$ (False Statement)
* **Interpretation:** "There is one singular 'magic number' $x$ that instantly solves the equation for every value of $y$ at the same time."
* **Why it fails:** If you fix $x$ as a constant before knowing $y$, it cannot adapt. For example, if you guess $x=5$, the equation only balances if $y=3$. If tested against a different input like $y=0$, the system collapses ($25 \neq 4$).

# Direct Proof: Square of an Odd Number

![[IMG_20260915_112342.jpg]]
An entry-level demonstration of a **direct proof** confirming that squaring any odd integer always yields another odd integer.

## The Logical Framework
* **Theorem:** If $n$ is odd, then $n^2$ is odd.
* **Notation:** $A \rightarrow B$ (Where $A$ is the condition "$n$ is odd", and $B$ is the consequence "$n^2$ is odd").

## Algebraic Proof Steps
1. **Define the Target:** Let $n$ be any odd integer. By definition, it can be expressed as:
   $$n = 2k + 1 \quad (\text{where } k \text{ is an integer})$$
2. **Square the Variable:** Expand the expression by squaring both sides:
   $$n^2 = (2k + 1)^2$$
   $$n^2 = 4k^2 + 4k + 1$$
3. **Establish structural parity:** Factor out a $2$ from the first two terms to isolate the algebraic form of an odd number ($2 \cdot \text{integer} + 1$):
   $$n^2 = 2(2k^2 + 2k) + 1$$
4. **Conclusion:** Because $(2k^2 + 2k)$ results in a clean whole integer, the entire expression simplifies down to a verified odd number format. $\blacksquare$

# Mathematical Proof Techniques
![[IMG_20260915_115537.jpg]]
## 1. Proof by Contraposition
A method that proves a conditional statement by establishing its logically equivalent flipped version. 

* **Logical Equivalence:** $p \rightarrow q \equiv \bar{q} \rightarrow \bar{p}$ (If $P$ then $Q$ is identical to If Not $Q$ then Not $P$).
* **Example Application:** 
  * *Theorem:* If $3n + 2$ is odd, then $n$ is odd.
  * *Strategy:* Prove the contrapositive instead: If $n$ is even, then $3n + 2$ is even.

## 2. Proof by Contradiction (Euclid's Theorem)
![[IMG_20260915_114830.jpg]]

A method where you assume the target statement is false and demonstrate that this assumption leads to an impossible logical paradox.

![[IMG_20260915_114826.jpg]]

* **Theorem:** There are infinitely many prime numbers.
* **The Proof Steps:**
  1. Assume the statement is false: Primes are finite, forming a complete list $\{p_1, p_2, \dots, p_n\}$.
  2. Construct a new number: $Q = (p_1 \cdot p_2 \dots \cdot p_n) + 1$.
  3. Analyze the contradiction:
     * If $Q$ is prime, it is a new prime missing from the "complete" list because $Q > p_n$.
     * If $Q$ is composite, it must be divisible by a prime $q$. However, dividing $Q$ by any prime on our list leaves a remainder of $1$. Thus, a new prime $q$ must exist outside the list.
  4. Conclusion: Both scenarios cause a logical breakdown, meaning the initial assumption is false. Primes are infinite.

## 3. Formal Logic Structure of Contradiction
Boolean algebra demonstrates the validity of proof by contradiction by showing how an invalid assumption self-destructs.

* **Objective:** Prove statement $X$ is true ($X = 1$).
* **The Process:**
  * Assume the negation of $X$ is true ($\bar{X}$).
  * Show that $\bar{X}$ simultaneously implies a condition $Y$ and its opposite $\bar{Y}$ ($Y \rightarrow \bar{Y}$).
  * Through logic optimization using OR ($\vee$) and AND operations, the expression reduces to $\bar{X} = 0$.
  * Therefore, $X = 1$ must be true.

# Ramsey Theory: The Theorem on Friends and Strangers

An introduction to **Ramsey Theory** demonstrating that complete disorder is impossible when a system is large enough. Specifically, this proves that the Ramsey number **$R(3,3) = 6$**.

## 1. Mathematical Setup ($K_6$)
* **Definition:** $K_6$ represents a **complete graph with 6 vertices** (points).
* **Edges:** Every vertex is directly connected to every other vertex, resulting in exactly **15 edges** (lines).
* **The Party Analogy:** Imagine 6 people at a party. Any two people are either friends or complete strangers.

## 2. The Theorem
* **Statement:** If every edge of a $K_6$ graph is colored either **red** or **blue**, there must exist at least one monochromatic triangle (a triangle where all three sides are the same color).
* **Game Analogy:** Two players alternate coloring lines. A player wins by completing a triangle of their own color (*"same color triangle $\rightarrow$ I win"*). The theorem proves a tie is impossible.
* **Real-world Meaning:** In any group of 6 people, there will always be at least 3 mutual friends or 3 mutual strangers.

## 3. Proof via the Pigeonhole Principle
To prove why a monochromatic triangle is guaranteed with 6 vertices, pick any single vertex $A$:

1. **Count the Connections:** Vertex $A$ connects to the remaining 5 vertices with 5 distinct edges.
2. **Apply Pigeonhole Principle:** Since there are 5 edges but only 2 color choices (Red or Blue), at least **3 edges** must share the same color. 
   * Let's assume at least 3 edges coming out of $A$ are **Red**, connecting to vertices $B$, $C$, and $D$.
3. **Analyze the Sub-Triangle ($BCD$):** Look at the edges connecting $B$, $C$, and $D$ to each other:
   * **Case 1:** If *any* edge between them ($BC$, $CD$, or $BD$) is **Red**, it completes a **Red triangle** with vertex $A$.
   * **Case 2:** If *none* of the edges between them are Red, then all three edges ($BC$, $CD$, and $BD$) must be **Blue**. This forms a **Blue triangle**.
1. **Conclusion:** In all possible scenarios, a single-colored triangle cannot be avoided. $\blacksquare$

# Formal Proof of R(3,3) = 6

The formal, step-by-step logic and structural breakdown demonstrating why a monochromatic triangle is guaranteed in a two-colored $K_6$ complete graph.

## 1. The Core Logic Sequence
The proof is structured as a chain of logical implications:

$$\begin{aligned}
P &\rightarrow P_1 \\
P_1 &\rightarrow P_2 \\
P_2 &\rightarrow P_3 \quad (\text{The Theorem Statement}) \\
\hline
\therefore P &\rightarrow P_3
\end{aligned}$$

## 2. Step-by-Step Breakdown

### $P$ : Initial Setup & Boundary Conditions
* **Statement:** Consider any valid edge-coloring configuration of a complete graph with $6$ vertices ($K_6$), where every single edge is colored either **red** or **blue**.

### $P_1$ : The Pigeonhole Principle Phase
* **Statement:** Isolate any single vertex. Because it is a complete graph of $6$ points, this vertex sends out exactly **$5$ edges** to the remaining points.
* **Implication:** Since there are $5$ lines but only $2$ available colors, the Pigeonhole Principle dictates that **at least $3$ of these edges must share the exact same color** (e.g., Red).

### $P_2$ : Structural Case Analysis
* **Statement:** Let $x$, $y$, and $z$ be the three distinct endpoint vertices connected to our initial point via those $3$ red edges. 
* **Implication:** We evaluate the sub-graph formed between endpoints $x$, $y$, and $z$:
  * **Case A:** If *any* connecting edge between them ($xy$, $yz$, or $xz$) is colored **red**, it immediately connects back to the starting point to complete a **Red Triangle**.
  * **Case B:** If *none* of the edges between them are red, then all three edges ($xy$, $yz$, and $xz$) must be **blue**, explicitly forming a **Blue Triangle**.

### $P_3$ : The Final Theorem Realization
* **Statement:** Since Case A and Case B are completely exhaustive, a monochromatic triangle **must** appear in any valid coloring setup. The proof holds true. $\blacksquare$
* 