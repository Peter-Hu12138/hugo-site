+++
date = '2025-11-29T02:39:04-05:00'
draft = false
title = 'CSC236: Introduction to Theory of Computation'
categories = ['Computer Science']
tags = ['Theory']
+++
{{< katex >}}
I am still on my way to finish the notes for CSC236.
## Introduction
Computer Science is about problem-solving: understanding, modelling, and solving problems. How do computer scientists ensure what they do is correct? Logic and math are all you need, and of all the math humans have explored, discrete mathematics is the most relevant to the core of computer science - logic, language, and formal representation of structures. Thus, naturally, a lot of contents in the course concern topics in discrete mathematics if not all.

## Induction

Empirical induction is [problematic](https://plato.stanford.edu/entries/induction-problem/), at least not rational if not completely unrealiable, but mathematical induction is not as it is based on deductive logic and gurantees truth through propagation along the chain of implication.

### Simple Induction (a.k.a. Weak Induction)
Let \\(P\\) be any predicate on naturals. Principle of Simple Induction is

$$[P(0) \wedge \forall n \in \mathbb{N}. P(n) \implies P(n+1)] \implies \forall n \in \mathbb{N}. P(n)$$

### Complete Induction (a.k.a Strong Induction)

Let \\(P\\) be any predicate on naturals. Principle of Complete Induction is

$$[\forall n \in \mathbb{N}. [\forall k \in \mathbb{N}. k < n \implies P(k)] \implies P(n)] \implies \forall n \in \mathbb{N} P(n)$$

Note that the base cases are still there but latent.

### Structural Induction

Speaking responsibly, I have to spend more time explaining recursively defined sets before I introduce structural induction; however, let's just understand them as the "smallest" sets that derive from basis and a set of derivation rules.

Let \\(B\\) be a set, \\(f_1, f_2, ..., f_k\\) be a set of derivation functions:

define set \\(S\\) as:
$$\forall b \in B, b \in S$$
$$\forall s_1, s_2, ..., s_m \in S, f_1(s_1, ...) \in S, f_2(s_1,) \in S, ..., f_k(s_1, ...) \in S$$

Then the Principle of Structural Induction states for any arbitrary predicate on \\(S\\):

proving
$$\forall b \in B, P(b) \wedge [\forall s_1, s_2, ..., s_m, \bigwedge_i^n {P(s_i)} \implies \bigwedge_i^k {P(f_i(s_1, ...)})]$$ is sufficient to show
$$\forall s \in S, P(s)$$


### Well Ordering Principle (WOP)

### Equivalence of Simple Induction, Complete Induction, and WOP
working on it next (but also mention complexty of formal axioms)


## Algorithm Correctness

Meaning of correctness:

`
If the precondition is met, then the algorithm will terminate free of error and meet post condition.
`

### Recursive Algorithm Correctness

What follows are conditions to check for recursive algorithm correctness, the principle goes:

Assume precondition is met, proving the following suffices as proof of the correctness of the algorithm:

1. Valid calls (Validity):  all the calls are valid (satisfying preconditions), non-recursive ones and recursive ones.
2. Valid recursively (Termination): Size is a function of input whose output is the a natural, and we need to prove size is strictly decreasing in consecutive recursive calls by showing each function delegates to recursive calls with a value whose size is smaller.
3. Correct Return Value (CRV): Assuming recursive calls are correct - i.e. if we supplement the calls with correct input values, they will terminate and return correct return values free of error, we prove the algorithm returns correct return values.

Why is it correct? The principle relies on Well Ordering Principle and induction under the hood.

We can justify termination with "valid recursively", if a size function has a codomain of naturals and decreases on consecutive recursive calls always, then any call to the algorithm would generate a decreasing sequence of naturals formed by the sizes of recursive calls on the call stack. Since all decreasing sequences of naturals are finite, which is a corollary of Well Ordering Principle (WOP), the sequence of finite calls terminates.

For valid calls and correct return values, we based correctness on correctness of recursive calls, but why can we assume recursive calls are correct and take what follows as a sufficient proof for the correctness of the entire algorithm. Isn't that circular reasoning? It isn't because we are not assuming \\(P(n)\\) is correct and proving \\(P(n)\\) is correct, what we do is essentially assuming \\(P(k)\\) is correct and proving \\(P(n)\\) is correct for some \\(k < n\\) with respect to the size function. 

This resembles and in fact depends on induction, I will first demonstrate one proof with complete induction. To start, let's assume inputs are of the set \\(C\\) and define \\(P\\) be the predicate that \\(\forall c \in C, P(c) \\) denotes the algorithm is correct on input \\(c\\). The ultimate goal is to prove \\(\forall c \in C, P(c)\\). To use complete induction, we need a size function to measure inputs; by valid recursively, it could be assumed w.l.o.g. there exists \\(size: C \to \mathbb{N}\\).

After setting up, we get this guy with aid of size function: \\(\forall n \in \mathbb{N}, \forall c \in C, n = size(c) \implies P(c)\\); note that the truth of this statement implies the truth of \\(\forall c \in C, P(c)\\), you can verify by selecting an arbitrary \\(c \in C\\), and realize \\(size(c) \in \mathbb{N}\\). Therefore it suffices to prove the longer one with complete induction:

Let \\(n \in \mathbb{N}\\) and assume \\(\forall k \in \mathbb{N}, k < n \implies \forall c \in C, k = size(c) \implies P(c)\\) (I.H.) Since we proved any recursive call made has a smaller size (valid recursively), by I.H., we can work with these recursive calls as if they are correct as they descend to smaller sizes by "valid recursively" and conclude with complete induction that the algorithm is correct on every input from \\(C\\).

> **Note:** Intuively, we can understand it as if we are trying to find a metric - the size function with a codomain of naturals - such that the recursive algorithm is always calling (or delegating) itself with smaller inputs with respect to the metric.

Similarly, a structural induction approach could make a proof but perhaps cleaner for this proof omits a definition of a size function but requires a construction of a recurrence definition of the input size. It is then needed that we prove \\(P\\) on the base cases and then on the constructor cases, which could naturally match the writeup of the algorithm.

### Iterative Algorithm Correctness

Proving that iterative algorithms are correct is similar, relying on inductive thinking, with differences in how repetitions are handled: iterative algorithms don't call themselves on smaller cases but base their correctness off what was left after previous iterations - a.k.a. Loop Invariants.

Usually, a iterative algorthm correctness has two major goals, normal termination (terminating free of error) and correct return value, which usually correspond to the following trifold procedure:

1. Validity: Every operation and function call are correct. This is usually done through proving variables are of certain types and/ or of certain ranges so that all the operands have the correct types and all the function calls are made with correct input value: satisfying precondition.
2. Termination: Similar to Recursive correctness, we define a bounded, strict monotonic integer variant (c.p. size) on loop variables so that by consequences of Well Ordering Principle, we know the sequence of variant at each iteration forms is finite.
3. Correct Return Value: This step requires possibly the most inventiveness out of all three. Unlike recursive algorithm where we can rely on post condition of itself, we have to come up with our invariant that ensures the correctness of return value exiting the loop and just before return.


## Mathematical Recurrence Formula and Runtime analysis

### Master's Theorem

#### Intuitive "Proof" of Master's Theorem

## Finite Automata, Regular Expression, and Regular Language

TODO: subset construction, general transition maps, regex in today's engines isn't regular


### Myhill-Nerode Theorem & Irregular Language