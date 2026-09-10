## Proposition
- A declarative statement that is either true or false.

## Compound Propositions
- Compromises of propositions and one or more of the [[Connectives]].

## Propositional Variables
- Are represented commonly with p, q, r

## Truth Tables
- Each row gives us one possibility for the truth values of our proposition(s).
- Each proposition will have exactly 2 values **true** or **false**.
- Will have $2^n$ rows where $n$ is the amount of propositions.

![[Screenshot_2026-09-10-10-50-33-098_com.google.android.googlequicksearchbox-edit.jpg]]


## Compound Proposition Truth Table Walk-Thru

### 📊 Rows
* **Formula**: Need $2^n$ rows (where $n$ is the number of propositional variables).
* **Purpose**: Needed for every possible combination of values for the compound propositions.

---

### 📋 Columns
* **Variables**: Need a column for each propositional variable.
* **Sub-expressions**: Need a column for the truth value of each expression that occurs in the compound proposition as it is built up.
* **Final Compound**: Need a column for the compound proposition (usually at far right).

---

### 🔢 Order of Operations
- When analyzing logical expressions, operators must be evaluated according to the following precedence hierarchy (from 1 to 5, highest to lowest):

| Precedence | Operator | Logical Meaning |
| :---: | :---: | :--- |
| **1** | $\neg$ | Negation (Not) |
| **2** | $\wedge$ | Conjunction (And) |
| **3** | $\vee$ | Disjunction (Or) |
| **4** | $\rightarrow$ | Conditional (Implication) |
| **5** | $\leftrightarrow$ | Biconditional (If and only if) |

## 📝 Compound Proposition Truth Table Walk-Thru

### 🔍 Example Expression: $(p \vee q) \rightarrow \neg r$

---

### 🛠️ Step-by-Step Construction

#### Step 1: Base Variables
First, construct columns for each atomic proposition: $p$, $q$, and $r$.

| $p$ | $q$ | $r$ |
| :---: | :---: | :---: |

#### Step 2: Intermediate Compound Propositions
Next, create a column for each sub-expression needed to build the final statement: $p \vee q$ and $\neg r$.

| $p$ | $q$ | $r$ | $p \vee q$ | $\neg r$ |
| :---: | :---: | :---: | :---: | :---: |

#### Step 3: Final Compound Proposition
Lastly, create a column for the complete compound proposition.

| $p$ | $q$ | $r$ | $p \vee q$ | $\neg r$ | $(p \vee q) \rightarrow \neg r$ |
| :---: | :---: | :---: | :---: | :---: | :---: |

---

### ❓ Why is $p \vee q$ written before $\neg r$ in Step 2?

Even though Negation ($\neg$) has a higher operator precedence than Disjunction ($\vee$), the table puts $p \vee q$ first simply to follow the **left-to-right reading order** of the expression. 

* **Independence:** Both $p \vee q$ and $\neg r$ are independent building blocks. Neither one relies on the other.
* **Flexibility:** You can write them in either order. Swapping the columns for $\neg r$ and $p \vee q$ would result in the exact same final answer.

---

### ⚠️ When Does Precedence Actually Matter?

Operator precedence is crucial **when you need to determine how to group components if there are no parentheses**. It tells you which operator "grabs" its variables first.

#### 1. Determining What an Operator Applies To
In the expression $\neg p \wedge q$:
* Because $\neg$ has higher precedence than $\wedge$, it applies **only** to $p$. 
* The expression is treated as $(\neg p) \wedge q$, **not** $\neg(p \wedge q)$.

#### 2. Identifying Column Dependencies
In the expression $p \vee q \wedge r$:
* Because $\wedge$ has higher precedence than $\vee$, the expression means $p \vee (q \wedge r)$.
* Therefore, you **must** build a column for $q \wedge r$ first. You cannot build a column for $p \vee q$ because $q$ is bound to $r$.

![[IMG_20260910_125029.jpg]]

![[IMG_20260910_125535.jpg]]

![[IMG_20260910_125545.jpg]]


![[IMG_20260910_125815.jpg]]


![[IMG_20260910_125830.jpg]]

![[IMG_20260910_125856.jpg]]

![[IMG_20260910_130034.jpg]]


![[IMG_20260910_130058.jpg]]****


![[IMG_20260910_130125.jpg]]


![[IMG_20260910_130141.jpg]]