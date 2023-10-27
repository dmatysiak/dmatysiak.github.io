---
title: "\"Type Theory and Functional Programming\"<br/>(Chapter One)"
layout: post
author: "Daniel A. Matysiak"
categories: ["summary", "ttfp", "thompson"]
usemathjax: true
---

In the introduction, Thompson introduces the reader to logic, which the author defines as
"the science of argument". Thompson focuses especially on *formal logic*, and endeavors to
give the reader a few justifications for the formalization of logic. Principal among these
justifications are:

1. clarity of characterization of valid proofs, which aids in judging particular arguments
   and in "sharpening our understanding of informal reasoning";
2. the ability to check the correctness of arguments mechanically;
3. its usefulness in mathematically studying properties, like expressive strength, of
   formal theories relative to other formal theories or according to some semantic
   interpretation for them.

<p class="noindent">Thompson's stated aim is to provide the reader a formal system by which he can express
arguments for the validity of particular sentences. He begins by introducing the reader to
natural deduction systems for propositional, and later, predicate logic.</p>

### Propositional logic

Propositional logic formalizes arguments that make use of connectives like "and", "or",
"not" and "implies" to join simpler formulas together into more complex formulas,
beginning with propositional variables and atomic propositions. A formula is either

1. a propositional variable $X_i$, or
2. a compound formula of one of the following forms:
   * $A \land B$
   * $A \lor B$‎
   * $\neg A$‎
   * $A \implies B$‎
   * $A \iff B$‎
   * $\bot$‎

where $A$, $B$,... are variables in the meta-language standing in for arbitary
formulas. This description is of the language in which propositions are encoded. A logical
system also needs to specify what constitutes a valid argument, often called *proofs* or
*derivations* of the system. An argument encodes an inference of some conclusions from
zero or more assumptions. Deduction rules are used to construct larger derivations out
of smaller deductions.

#### Derivation rules for propositional logic

The simplest derivation is the *assumption rule* which states that any formula $A$ may
be derived from the assumption of $A$ itself. The proof is trivial:

$$A$$

The *conjunction introduction rule* builds a composite derivation by combining derivations
of the conjuncts into a derivation of the conjunction:

$$\frac{\begin{array}{lr}A & B\end{array}}
       {A \land B}(\mathtt{\land I})$$

The *elimination rules for conjunction* allows us to derive the constituent conjuncts from
an existing conjunction:

$$\frac{A \land B}
       {A}(\mathtt{\land E_A})$$

$$\frac{A \land B}
       {B}(\mathtt{\land E_B})$$

*Implication introduction* allows us to infer the formula $A \implies B$ from a proof of
$B$. Note that $B$ *may* depend on $A$, but implication introduction does not depend
on $A$. We do not need a proof of $A$, only a proof of $B$. Therefore, we can
discharge $A$ (denoted by $[A]$):

$$\frac{\begin{array}{c}[A]\\\vdots\\B\end{array}}
       {A \implies B}(\mathtt{\Rightarrow{I})}$$

The application of the introduction rule can be related to the discharged premise by using
a label.

As with conjunction, there is an *implication elimination rule* which effectively
expresses modus ponens:

$$\frac{\begin{array}{lr}A & A \implies B\end{array}}
       {B}(\mathtt{\Rightarrow{E})}$$

*Disjunction* has two introduction rules, depending on which disjunct we have a proof for:

$$\frac{A}
       {A \lor B}(\mathtt{\lor{I_A})}$$

$$\frac{B}
       {A \lor B}(\mathtt{\lor{I_B})}$$

Because we don't necessarily know which disjunct in a disjuntion we have a proof for, the
elimination rule works by showing that, if we know that $A \lor B$, and that $C$ can be
derived from both $A$ and $B$, then we can conclude that $C$:

$$\frac{\begin{array}{ccc}
          & [A] & [B] \\
          & \vdots & \vdots \\
          A \lor B & C & C
        \end{array}}
       {C}(\mathtt{\lor{E})}$$

The premises $A$ and $B$ are discharged in order to infer $C$.

Absurdity ($\bot$), otherwise known as contradiction or the false proposition, does not
have an introduction rule, though it can be inferred from a proposition like $A \land
\neg A$. But it can be eliminated, because anything follows from contradiction (*ex falso
quodlibet*):

$$\frac{\bot}
       {A}(\mathtt{\bot E})$$

Using $\bot$ elimination, we can can define *negation*:

$$\neg A \equiv_{\mathtt{def}} A \implies \bot$$

and using this definition, we can define for negation its introduction rule:

$$\frac{\begin{array}{cc}
          [A] & [A] \\
          \vdots & \vdots \\
          B & \neg B
        \end{array}}
       {\neg A}(\mathtt{\neg{I})}$$

and elimination:

$$\frac{\begin{array}{lr}
          A & \neg A \\
        \end{array}}
       {B}(\mathtt{\neg{E})}$$

Bi-implication, as usual, can be defined as

$$\begin{array}{ccc}
    A \iff B & \equiv_{\mathtt{def}} & (A \implies B) \land (B \implies A)
  \end{array}$$

which allows us to define introduction:

$$\frac{\begin{array}{cc}
          A \implies B & B \implies A
        \end{array}}
       {A \iff B}(\mathtt{\iff{I})}$$

and elimination:

$$\frac{A \iff B}
       {A \implies B}(\mathtt{\iff{E_A})}$$

$$\frac{A \iff B}
       {B \implies A}(\mathtt{\iff{E_B})}$$

Thompson also notes that the approach taken here is intuitionistic, and that "classical
logic" is an extension of the intuitionistic case. This extension is accomplished by
adding the *law of excluded middle*:

$$\frac{}
       {A \lor \neg A}(\mathtt{EM})$$

Alternative rules that can be used to characterize classical logic are the *rule of double
negation*:

$$\frac{\neg\neg A}
       {A}(\mathtt{DN})$$

and the *rule of proof by contradiction*:

$$\frac{\begin{array}{c c}
          [\neg A] & [\neg A]\\
          \vdots & \vdots\\
          B & \neg B
        \end{array}}
       {A}(\mathtt{CC})$$


### Predicate logic

In the second section of the first chapter, Thompson covers predicate logic, "the logic of
properties or *predicates*". Whereas propositional logic composes complex propositions
from atomic, unanalyzed propositions, propositions in predicate logic are constructed from
predicates, or statements about certain properties holding for certain objects.

Syntactically, the language of predicate logic can be divided into formulae and
terms. Terms denote objects, and have one of the following forms:

* Variables ($v_0, v_1,\ldots$, or, $x, y, z, u, v,\ldots$).
* Constants ($c_0, c_1,\ldots$, or, $a, b, c,\ldots$).
* Composite terms ($f_{n,m}(t_1, \ldots, t_n)$, formed by applying *function symbols*,
  like $f, g, h,\ldots$, to other terms $s, t, t_1,\ldots$, where $n$ is the *arity*
  and $m$ is some index to distinguish function symbols).

Propositions have three forms:

* Atomic formulas. $P_{n,m}(t_1,\ldots,t_n)$ (where $P_{n,m}$ is the predicate symbol
  with arity $n$; $P$, $Q$, $R$, etc. are used for this purpose) expressing the
  fact, that the relation $P_{n,m}$ holds for terms $t_1,\dots,t_n$, taken
  together. Equality ($t_i = t_j$) is taken to be primitive, defining another class of
  formulas in the language.
* Propositional combinations. As with propositional calculus, connectives are used to
  combine propositional terms into new, compound propositions ($A \lor B$, $A \land
  B$, $A \implies B$, $A \iff B$, $\neg A$).
* Quantified formulas, which are of the form $\forall x.A$ and $\exists x.B$, where
  $x$ is an arbitrary variable, and $A$ and $B$ are arbitrary formulas.

#### Substitution in predicate logic

After providing a few examples of how quantifiers may be used, Thompson explains how
variables and substitutions operate in propositions with quantifiers. Quantifier
variables may be consistently renamed as long as the new name is not already in use in the
affected formula. A variable $x$ is *bound* in a fomula $\forall x.A$ or $\exists
x.B$. The quantifier binds values to $x$. All other occurrences are *free*. So in the
formula $\forall x.P(x,y)$, $x$ is bound, while $y$ is free.

*Substitution* is the act of consistently replacing a variable with an arbitrary
term. Only free variables may be substituted arbitrarily in this way, unlike bound
variables, which express a universal or existential claim. Thompson uses the notation
$A[t/x]$ to mean the formula $A$ with $t$ substituted for $x$.

When substituting a variable with an arbitrary term, we must be careful to avoid *variable
capture*. Variable capture occurs when a free variable within the term substituted for a
variable contains a variable that is otherwise bound in the formula in which we are making
the substitution. Thompson gives the example of substituting the term $y+1$ for $x$ in the
formula $\exists y.y>x$. This substitution results in the formula $\exists y.y>y+1$, which
is obviously false. To avoid this problem, we can rename the variable $y$ in $y+1$ to
something that does not occur in $\exists y.y>x$ before performing the substitution.

Formally, Thompson defines substitution, inductively, in the following way.

For a term $s$, $s[t/x]$ is defined as follows:

* $x[t/x] \equiv_\text{df} t$
* if $y \not\equiv x$, then $y[t/x] \equiv_\text{df} y$
* $f_{n,m}(t_1,\ldots,t_n)[t/x] \equiv_\text{df} f_{n,m}(t_1[t/x],\ldots,t_n[t/x])$

For a formula $A$, $A[t/x]$ is defined as follows. For atomic formulas:

* $P_{n,m}(t_1,\ldots,t_n)[t/x] \equiv_{df} P_{n,m}(t_1[t/x],\ldots,t_n[t/x])$
* $(t_1 = t_2)[t/x] \equiv_\text{df} t_1[t/x] = t2[t/x]$

Substitution distributes over propositional combinations:

* $(A \lor B)[t/x] \equiv_\text{df} A[t/x] \lor B[t/x]$
* $(A \land B)[t/x] \equiv_\text{df} A[t/x] \land B[t/x]$
* $(A \implies B)[t/x] \equiv_\text{df} A[t/x] \implies B[t/x]$
* $(A \iff B)[t/x] \equiv_\text{df} A[t/x] \iff B[t/x]$
* $(\neg A)[t/x] \equiv_\text{df} \neg (A[t/x])$

For quantifiers:

* $(\forall x.B)[t/x] \equiv_\text{df} \forall x.B$
* $(\exists x.B)[t/x] \equiv_\text{df} \exists x.B$

If $y \not\equiv x$ and $y$ *does not* appear in $t$, then

* $(\forall y.B)[t/x] \equiv_\text{df} \forall y.(B[t/x])$
* $(\exists y.B)[t/x] \equiv_\text{df} \exists y.(B[t/x])$

On the other hand, if $y \not\equiv x$ and $y$ *does* appear in $t$, then

* $(\forall y.B)[t/x] \equiv_\text{df} \forall z.(B[z/y][t/x])$
* $(\exists y.B)[t/x] \equiv_\text{df} \exists z.(B[z/y][t/x])$

where $z$ is a variable that does not appear in either $t$ or $B$.

#### Quantifier rules

The first quantifier rule Thompson discusses is $\forall$ introduction.

$$\frac{A}
       {\forall x.A}(\mathtt{\forall I})$$

This rule obviously holds when all variables in $A$ are bound, but it is also true when
$x$ is free. That is because free variables may be freely substituted, which means the
formula is said to hold for *any* value in the same domain as the introduced universal
quantifier. The tacit assumption here is that, in order for us to be able to make this
inference, it must be the case that $x$ does not appear free in any assumptions in the
proof of $A$. The reason this condition must hold is, because for this introduction rule
to hold, $x$ must be arbitrary, but an assumption can constrain the domain of values $x$
can take such that it is no longer arbitrary (e.g., $x > 0$). Thompson calls this the
*side condition* of the rule.

$\forall$ elimination, on the other hand, says that, because $A$ is true for all values of
$x$, it follows that $A$ is true for any arbitary value of $x$.

$$\frac{\forall x.A}
       {A[t/x]}(\mathtt{\forall E})$$

$\exists$ introduction is, likewise, straightforward. If we know that $A$ is true for some
specific value of $x$, then we know there exists an $x$ for which $A$ is true.

$$\frac{A[t/x]}
       {\exists x.A}(\mathtt{\exists I})$$

$\exists$ elimination, however, has a side condition. Suppose that we can prove $B$ from
$A$ and that $x$ is free in $A$ and not in any other of the assumptions in the proof of
$B$. We can then prove $B$.

$$\frac{\begin{array}{cc}
                      & [A] \\
                      & \vdots \\
          \exists x.A & B
        \end{array}}
       {B}(\mathtt{\exists E)}$$

The introduction and elimination rules for quantifiers may be seen as generalizations of
their propositional counterparts for conjunction and disjunction.
