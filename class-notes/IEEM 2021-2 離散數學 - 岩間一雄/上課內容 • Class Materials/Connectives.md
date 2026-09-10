Prefix or connections from one proposition to another

## List of Connective elements

| Connective Symbols |     Words      |     Terms     |        Example        |
| :----------------: | :------------: | :-----------: | :-------------------: |
|      $\land$       |      AND       |  Conjunction  |      $A \land B$      |
|       $\lor$       |       OR       |  Disjunction  |      $A \lor B$       |
|   $\rightarrow$    |    Implies     |  Implication  |   $A \rightarrow B$   |
| $\leftrightarrow$  | If and only if | Biconditional | $A \leftrightarrow B$ |
|       $\neg$       |      Not       |   Negation    |       $\neg A$        |

## Negation
- The negation of a proposition.
- If $p$ denotes "the grass is green" $\rightarrow$ It is not the case that the grass is green ($\neg p$).

## Conjunction
- The conjunction of propositions.
- Which could be read as "and".
- For a conjunction to be true BOTH propositions must be true.

## Disjunction
- The disjunction of propositions.
- Which could be read as "or".
- For a disjunction to be true AT LEAST ONE proposition must be true.


## Implication (Conditional Statement)

#### 1. Definition and Reading
* **Notation**: The implication of propositions $p$ and $q$ is denoted as **$p \rightarrow q$**.
- Which could be read as "implies" or "if..., then...".
* **How to read**:
    * "If $p$ then $q$"
    * "$p$ implies $q$"

#### 2. Truth Value Rules
* When the **hypothesis is True**, the **conclusion must be True** for the implication to be True.
* When the **hypothesis is False**, the implication is **automatically True** (regardless of the conclusion's truth value).

#### 3. Truth Table

| $p$ | $q$ | $p \rightarrow q$ |
| :---: | :---: | :---: |
| T | T | **T** |
| T | F | **F** |
| F | T | **T** |
| F | F | **T** |

> [!NOTE] Core Rule
> The conditional statement $p \rightarrow q$ is **only False** when the **hypothesis ($p$) is True and the conclusion ($q$) is False**. In all other cases, it is True.

#### 4. Example
* **$p$ denotes**: "It is a holiday"
* **$q$ denotes**: "The store is closed"
* **$p \rightarrow q$**: "If it is a holiday, then the store is closed."


## Converse, Inverse, and Contrapositive

### Core Logical Concepts
From our foundational **implication (conditional statement)** $p \rightarrow q$, we can construct 3 new conditional statements:

| Statement Type | Symbolic Form | Logical Operation | Notes |
| :--- | :---: | :---: | :--- |
| **Implication (Original)** | $p \rightarrow q$ | — | If $p$, then $q$. |
| **Converse** | $q \rightarrow p$ | **Switch order** | Swaps the antecedent and the consequent. |
| **Inverse** | $\neg p \rightarrow \neg q$ | **Negate** | Negates both the antecedent and the consequent. |
| **Contrapositive** | $\neg q \rightarrow \neg p$ | **Switch and negate** | 💡 **Shares the same truth value as $p \rightarrow q$** (Logically equivalent). |

---

### Example Breakdown

#### Original Sentence
> *"It is raining is a sufficient condition for me not going to town."*

#### If-Then Form
$$\text{If } \underbrace{\text{it is raining}}_{p}, \text{ then } \underbrace{\text{I won't go to town}}_{q}.$$

*   **$p$ (Antecedent):** It is raining
*   **$\neg p$:** It is not raining
*   **$q$ (Consequent):** I won't go to town
*   **$\neg q$:** I will go to town

#### The Four Propositional Forms

*   **Implication ($p \rightarrow q$):** 
    If it is raining, then I won't go to town.
*   **Converse ($q \rightarrow p$):** 
    If I won't go to town, then it is raining.
*   **Inverse ($\neg p \rightarrow \neg q$):** 
    If it is not raining, then I will go to town.
*   **Contrapositive ($\neg q \rightarrow \neg p$):** 
    If I will go to town, then it is not raining.

## Biconditional

### 1. Core Definition
* **Notation**: $p \leftrightarrow q$
* **Reading**: Read as **"p if and only if q"** (frequently abbreviated as **iff**).
* **Rule**: For a biconditional to be true, **both propositions must share the same truth value**.

---

### 2. Truth Table

| Proposition $p$ | Proposition $q$ | Biconditional $p \leftrightarrow q$ |
| :-------------: | :-------------: | :---------------------------------: |
|      **T**      |      **T**      |                **T**                |
|      **T**      |      **F**      |                **F**                |
|      **F**      |      **T**      |                **F**                |
|      **F**      |      **F**      |                **T**                |

> [!TIP] Quick Summary
> * **Same Values = True**: If both $p$ and $q$ match (TT or FF), the outcome is **T**.
> * **Different Values = False**: If $p$ and $q$ do not match (TF or FT) the outcome is **F**.

## Biconditional Equivalence

Our **biconditional** $p \leftrightarrow q$ can also be written as a **compound proposition**:

$$ (p \leftrightarrow q) \equiv (p \rightarrow q) \land (q \rightarrow p) $$

> [!info] Concept
> This demonstrates that a biconditional statement ("$p$ if and only if $q$") is logically equivalent ($\equiv$) to the conjunction ($\land$) of two conditional statements ("$p$ implies $q$" AND "$q$ implies $p$").

---

### Truth Table Verification

The truth table below verifies this equivalence. Notice that the truth values of the last two columns match exactly (⭐).

| $P$ | $Q$ | $P \rightarrow Q$ | $Q \rightarrow P$ | $(P \rightarrow Q) \land (Q \rightarrow P)$ | $P \leftrightarrow Q$ |
| :-: | :-: | :---------------: | :---------------: | :-----------------------------------------: | :-------------------: |
|  T  |  T  |         T         |         T         |                   **T** ⭐                   |        **T** ⭐        |
|  T  |  F  |         F         |         T         |                    **F**                    |         **F**         |
|  F  |  T  |         T         |         F         |                    **F**                    |         **F**         |
|  F  |  F  |         T         |         T         |                   **T** ⭐                   |        **T** ⭐        |

