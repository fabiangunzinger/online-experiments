# Approaches to causal inference

- See holland1986statistics
- See also https://www.the100.ci/2024/06/26/sometimes-a-causal-effect-is-just-a-causal-effect-regardless-of-how-its-mediated-or-moderated/

## Perl

Directed Acyclic Graphs

- A *confounder* is a variable that simultaneously affects the treatment indicator and the outcome (the same as an omitted variable).
- A *collider* is a variable that is simultaneously affected by the treatment indicator and the outcome.
- A *backdoor path* is a path from the treatment indicator to the outcome via a confounder.
- There are two ways to close a backdoor path:
  - Control for the confounder if it is available
  - Have a collider on the backdoor path
- An analysis design meets the *backdoor criterion* if all backdoor paths are closed, in which case we have isolated a causal effect.

## Thought experiments

- See @heckman2023econometric


## From chat:

In response to my question on why rubin2005causal would refer to probabilistic causal inference instead of just causal inference (i.e. what is former and what alternatives are there):



---

## 📦 Other Types of Causal Inference

### **1. Deterministic Causal Inference**
- Assumes a **fixed functional relationship** between treatment and outcome.
- Often seen in **structural equation modeling** or **causal graphs** where the model specifies exact deterministic links (e.g., \( Y = f(X) \)).
- No randomness in assignment or outcomes.

👉 Used more in **computer science, logic-based systems, or some economic theory**.

---

### **2. Bayesian Causal Inference**
- Similar to probabilistic, but **uncertainty is modeled using probability distributions over unknown quantities**.
- Rather than computing confidence intervals, you compute **posterior distributions** for causal effects.
- Assignment mechanism and potential outcomes may both be modeled probabilistically.

👉 Useful when combining **prior beliefs** with observed data.

---

### **3. Graphical (Structural) Causal Inference**
- Based on **causal diagrams (DAGs)** and **do-calculus** (Pearl).
- Focuses on **identifying** whether a causal effect can be computed from observed data, given a model.
- Doesn't always require probability to be the central tool — focuses more on **structure and logic**.

👉 Excellent for **reasoning about identifiability**, even in observational settings.

---

### **4. Experimental vs. Observational Inference**
- **Experimental** (e.g., RCTs): the assignment is controlled and known → inference is cleaner.
- **Observational**: assignment is not random → you need modeling (e.g., propensity scores, instrumental variables) to estimate causal effects.

---

## 🧠 So what's special about "probabilistic"?
Rubin emphasizes it because it:
- Formalizes how randomness in assignment supports inference.
- Anchors **randomized experiments** as the gold standard.
- Gives us tools like **randomization-based inference**, **confidence intervals**, **hypothesis testing**, etc.

---

## ✅ Summary Table

| Type                          | Randomness? | Assignment Mechanism | Tools |
|-------------------------------|-------------|-----------------------|-------|
| **Probabilistic (Rubin)**     | Yes         | Explicit & modeled    | Estimation + uncertainty |
| **Deterministic**             | No          | Often implicit        | Logical or structural reasoning |
| **Bayesian**                  | Yes         | Modeled + prior belief| Posterior distributions |
| **Graphical (DAG-based)**     | Maybe       | Encoded in DAG        | Do-calculus, identifiability |
| **Experimental**              | Yes         | Controlled            | Randomization inference |
| **Observational**             | No          | Modeled from data     | Matching, IVs, etc. |

---


