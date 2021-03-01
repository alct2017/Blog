# Lattice

## Review
### Partial order set(Poset)
Given a set $A$ and a relation $R$ on it, $(A,R)$ is called a ***partial order set*** if $R$ is **reflexive**, **antisymmetric**, and **transitive**.

Given a poset $(A,\leq)$, we can define:
1. $a$ is ***maximal*** (***minimal***) if $\exist b \in A$ s.t. $a\leq b$ ($a\geq b$)
2. $a$ is ***greatest*** (***least***) if $\forall b \in A$ s.t. $a\geq b$ ($a\leq b$)

Given a poset $(A,\leq)$ and a set $S\subseteq A$:
1. $u\in A$ is a ***upper bound*** (***lower bound***) of $S$ if $\forall s\in S,\ s\leq u$ ($s\geq u$)
2. $u\in A$ is a ***least upper bound*** (***greatest lower bound***) of $S$, or $LUB(S)$ ($GLB(S)$), if $u$ is the upper(lower) bound of $S$ and $\forall$upper(lower) bound $u'$ of $S$, $u\leq u'$ ($u\geq u'$)

A poset has at most one $LUB$ or $GLB$.

### Algebra structures
A set with an operation satisfies ***closed*** and ***some properties*** is an **algebra structures**.

Structure with one operation $+$: Semigroup or Group.

Structure with two operations $+$ and $*$: Ring or Field.

## Lattice
### Set View
A ***lattice*** is a poset $(A,\leq)$ in which any two elements $a,\ b$ have a $LUB(a,b)$ and a $GLB(a,b)$.

Define that:
1. **join** $a\cup b=LUB(a,b)$
2. **meet** $a\cap b=GLB(a,b)$

### Representation
Hasse diagram

### Properties
Lattice has the following properties:
1. Commutative
   $$a\cap b=b\cap a$$
   Proof: $a\cap b=GLB(a,b)=GLB(b,a)=b\cap a$, since $GLB(a,b)$ means $GLB(\{a,b\})$.
   $$a\cup b=b\cup a$$
2. Associative
   $$(a\cap b)\cap c=a\cap (b\cap c)$$
   $$(a\cup b)\cup c=a\cup (b\cup c)$$
3. Idempotent
   $$a\cup a=a$$
   $$a\cap a=a$$
4. Absorption
   $$(a\cup b)\cap a=a$$
   Proof: Suppose there exists $c=a\cup b$. Now we prove that $c\cup a=a$.

   (1)$a$ is a lower bound. Since $c=a\cup b$, $c\geq a$. Obviously $a\geq a$. So $a$ is a lower bound.

   (2)$a$ is greater than any lower bound. For any lower bound $l$, $l\leq a$.

   So $a=GLB(a,c)$.

   $$(a\cap b)\cup a=a$$

A lattice could be divided into a join-semilattice and a meet-semilattice.

### Algebra View
A ***semilattice*** is an algebra $S=(S,*)$ s.t. $\forall x, y, z\in S$:
1. $x*x=x$
2. $x*y=y*x$
3. $x*(y*z)=(x*y)*z$

A lattice is an algebra $L=L(L,\cup ,\cap )$ s.t.
1. $(L,\cap)$ and $(L,\cup)$ are two semilattice
2. $$(a\cup b)\cap a=a$$
   $$(a\cap b)\cup a=a$$

Theorem: Lattice in algebra view equals to that in set view.

Proof: (1)Introduce an order $\leq$. If $a\cup b=b$, then $a\leq b$.

(2)$GLB$/$LUB$ exists. We need to prove that $GLB(a,b)=a\cap b$, $LUB(a,b)=a\cup b$.

For $LUB(a,b)=a\cup b$, suppose there exists $c=a\cup b$.

First we prove that $c$ is an upper bound. $a\cup c=a\cup (a\cup b)=(a\cup a)\cup b=a\cup b=c$, so $a\leq c$. Similarly $b\leq c$, so $c$ is an upper bound.

Next we prove that $c$ is less than any upper bound. For any $c'$ s.t. $a\leq c'$ and $b\leq c'$, we know
$$a\cup c'=c'\quad b\cup c'=c'$$
then $c'=a\cup c'=a\cup (b\cup c')=(a\cup b)\cup c'=c\cup c'$, so $c\leq c'$.

### Sublattice and Extention
A subset $S$ of a lattice $L$ is called sublattice if it is closed under operation $\cap$ and $\cup$.