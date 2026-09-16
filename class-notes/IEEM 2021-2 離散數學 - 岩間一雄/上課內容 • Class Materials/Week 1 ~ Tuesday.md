![[IMG_20260910_123045.jpg]]![[IMG_20260910_123139.jpg]]![[IMG_20260910_123204.jpg]]![[IMG_20260910_123217.jpg]]

## The Basics of Truth Tables and Connectives

You can learn more about the [[Week 1 ~ Thursday |truth tables]] and [[Connectives]] in the following notes.

![[IMG_20260910_124025.jpg]]

# Propositional Logic: Equivalently Modifying a Formula

## Distributive Laws
The operations of logical conjunction (AND, represented here by concatenation) and disjunction (OR, represented by $\vee$) distribute over each other, unlike standard arithmetic.

* **Conjunction over Disjunction:**$$p(q \vee r) = pq \vee pr$$
- *Arithmetic Analogy:* 
$3(4 + 7) = 3 \cdot 4 + 3 \cdot 7$
  
* **Disjunction over Conjunction:**  
$$p \vee qr = (p \vee q)(p \vee r)$$
- *Arithmetic Analogy (Does NOT hold in standard math):* 
 $3 + 4 \cdot 7 \neq (3 + 4)(3 + 7)$

---

## De Morgan's Laws
De Morgan's Laws describe how mathematical negation interacts with conjunction and disjunction.

1. $$\overline{(p \wedge q)} \equiv \bar{p} \vee \bar{q}$$
2. $$\overline{(p \vee q)} \equiv \bar{p} \wedge \bar{q}$$

### Truth Table Verification
Below is the verification for the first law $\overline{p \wedge q} \equiv \bar{p} \vee \bar{q}$ (or $\overline{pq}$):

| $p$ | $q$ | $\bar{p}$ | $\overline{p \wedge q}$ |
| :---: | :---: | :---: | :---: |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 0 | 0 |

# Logical Simplification Proof

## Problem
Simplify the logical expression:
$$ \overline{p \lor \overline{p}q} $$

## Proof Steps
$$
\begin{aligned}
\overline{p \lor \overline{p}q} &= \overline{p} \land \overline{\overline{p}q} && \text{(De Morgan's Law)} \\
&= \overline{p} \land (p \lor \overline{q}) && \text{(De Morgan's Law \& Double Negation)} \\
&= \overline{p}p \lor \overline{p}\overline{q} && \text{(Distributive Law)} \\
&= 0 \lor \overline{p}\overline{q} && \text{(Complement Law: } \overline{p}p = 0\text{)} \\
&= \overline{p}\overline{q} && \text{(Identity Law)}
\end{aligned}
$$

## Used Laws Reference
* **De Morgan's Law:** $\overline{A \lor B} = \overline{A} \land \overline{B}$ and $\overline{A \land B} = \overline{A} \lor \overline{B}$
* **Double Negation:** $\overline{\overline{p}} = p$
* **Distributive Law:** $A \land (B \lor C) = (A \land B) \lor (A \land C)$
* **Complement Law:** $\overline{p} \land p = 0$
* **Identity Law:** $0 \lor A = A$

# Normal Forms in Boolean Logic

In boolean logic, propositional formulas can be converted into standard Canonical forms to make them easier to analyze, simplify, and use in truth tables or digital circuits.

## 1. Disjunctive Normal Form (DNF)
A formula is in **Disjunctive Normal Form** if it is an **"OR of ANDs"** (a disjunction of conjunctions). 

> [!INFO] Structure
> It consists of one or more clauses joined by $\vee$ (OR). Each clause contains variables or their negations joined strictly by $\wedge$ (AND).

* **Concept:** Sum of Products
* **Example:** $(A \wedge B) \vee (\neg C \wedge D) \vee (A \wedge \neg B)$
* **Single Term Case:** A single term like $\bar{p} \wedge \bar{q}$ is also technically in DNF.

---

## 2. Conjunctive Normal Form (CNF)
A formula is in **Conjunctive Normal Form** if it is an **"AND of ORs"** (a conjunction of disjunctions).

> [!INFO] Structure
> It consists of one or more clauses joined by $\wedge$ (AND). Each clause contains variables or their negations joined strictly by $\vee$ (OR).

* **Concept:** Product of Sums
* **Example:** $(A \vee B) \wedge (\neg C \vee D) \wedge (A \vee \neg B)$
* **Single Term Case:** A single term like $\bar{p} \wedge \bar{q}$ is also technically in CNF (viewed as two standalone single-variable clauses intersected together).

---

## Direct Comparison

| Feature | Disjunctive Normal Form (DNF) | Conjunctive Normal Form (CNF) |
| :--- | :--- | :--- |
| **Common Name** | OR of ANDs (Sum of Products) | AND of ORs (Product of Sums) |
| **Outer Operator** | $\vee$ (OR) | $\wedge$ (AND) |
| **Inner Operator** | $\wedge$ (AND) | $\vee$ (OR) |
| **Key Use Case** | Finding when an expression evaluates to **True** (1) | Finding when an expression evaluates to **False** (0) |
