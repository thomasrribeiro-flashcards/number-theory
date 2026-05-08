+++
order = 5
subject = "Math"
tags = ["math", "number-theory", "diophantine", "linear-congruence", "bezout", "integer-solutions"]
+++

# Number Theory — Linear Diophantine Equations and Linear Congruences

## 5.1 What Is a Diophantine Equation?

C: A [Diophantine equation] is an equation whose solutions are required to be integers.

Q: Why single out integer solutions from real solutions?
A: Because integer structure (discreteness, factorization, divisibility) dramatically changes the equation's landscape. Over $\mathbb{R}$, $x + y = 1$ has infinitely many solutions; over $\mathbb{Z}$, still infinitely many. But over $\mathbb{R}$, $3x + 6y = 5$ has infinitely many; over $\mathbb{Z}$, NONE. Divisibility constraints turn continuous problems into combinatorial ones.

## 5.2 Linear Diophantine Equations in Two Variables

C: A [linear Diophantine equation in two variables] has the form $ax + by = c$ where $a, b, c \in \mathbb{Z}$ and solutions $(x, y) \in \mathbb{Z}^2$ are sought.

Q: When does $ax + by = c$ have an integer solution?
A: Iff $\gcd(a, b) \mid c$. Necessary: $\gcd(a, b)$ divides every integer linear combination of $a, b$, hence divides $c$ if a solution exists. Sufficient: if $d = \gcd(a, b) \mid c$, write $c = dk$, find Bézout coefficients $ax_0 + by_0 = d$, and scale: $a(kx_0) + b(ky_0) = c$.

Q: If $(x_0, y_0)$ is one solution to $ax + by = c$, how do you describe all solutions?
A: All solutions are $(x, y) = (x_0 + (b/d)t, y_0 - (a/d)t)$ for $t \in \mathbb{Z}$, where $d = \gcd(a, b)$. Verification: $a(x_0 + (b/d)t) + b(y_0 - (a/d)t) = ax_0 + by_0 + (ab/d - ab/d)t = c$. The step size $b/d$ (resp. $a/d$) is the smallest nonzero $\Delta x$ (resp. $\Delta y$) preserving the equation.

## 5.3 Solving a Linear Diophantine Equation

P: Find all integer solutions to $21x + 14y = 35$.
S:
**IDENTIFY**: Two-variable Diophantine; check solvability and parametrize.

**PLAN**: Compute $\gcd(21, 14)$, verify divides $35$, find one solution, parametrize.

**EXECUTE**:
1. $\gcd(21, 14) = 7$, and $7 \mid 35$ ✓.
2. Divide through: $3x + 2y = 5$.
3. By inspection (or extended Euclidean), $3 \cdot 1 + 2 \cdot 1 = 5$, so $(x_0, y_0) = (1, 1)$.
4. All solutions: $x = 1 + 2t$, $y = 1 - 3t$ for $t \in \mathbb{Z}$ (step sizes $2 = 14/7$ and $3 = 21/7$).

**EVALUATE**: Check: $21(1 + 2t) + 14(1 - 3t) = 21 + 42t + 14 - 42t = 35$ ✓ for all $t$. Integer solutions form an infinite arithmetic sequence indexed by $t$.

## 5.4 Diophantine Equations in Applications

Q: What's a classic word-problem setting for linear Diophantine equations?
A: "I want to buy $x$ items at $\$5$ each and $y$ items at $\$7$ each, spending exactly $\$100$." This is $5x + 7y = 100$ with nonneg-integer solutions. $\gcd(5,7) = 1$ divides $100$, so solutions exist; parametrization then plus constraint $x, y \geq 0$ yields finitely many solutions. Number theory meets integer programming.

## 5.5 The Frobenius (Chicken McNugget) Problem

Q: What is the [Frobenius number] $g(a, b)$ of two coprime positive integers?
A: The largest integer NOT expressible as a non-negative-integer combination $ax + by$ with $x, y \geq 0$. Sylvester: $g(a, b) = ab - a - b$. For $(a, b) = (5, 7)$: $g = 35 - 12 = 23$ — $23$ cannot be made as $5x + 7y$ with $x, y \geq 0$, but every integer $\geq 24$ can. No closed form is known for three or more variables.

## 5.6 From Diophantine to Linear Congruence

Q: How does $ax \equiv b \pmod n$ relate to a linear Diophantine equation?
A: $ax \equiv b \pmod n$ iff $ax - b = ny$ for some $y \in \mathbb{Z}$, iff $ax - ny = b$. Solving the congruence is solving a 2-variable Diophantine equation. The two settings are equivalent; the reason to prefer one form is pedagogical — congruences emphasize residues, Diophantine equations emphasize integer combinations.

## 5.7 Linear Congruence Solvability (Review)

C: The linear congruence $ax \equiv b \pmod n$ has a solution iff [$\gcd(a, n) \mid b$]; when solvable, it has exactly $\gcd(a, n)$ solutions mod $n$.

Q: Why does the number of solutions mod $n$ equal $\gcd(a, n)$?
A: Let $d = \gcd(a, n)$. Divide through: $(a/d) x \equiv (b/d) \pmod{n/d}$ with $\gcd(a/d, n/d) = 1$, so unique $x \equiv x_0 \pmod{n/d}$. Lifting back to mod $n$: the solutions are $x_0, x_0 + n/d, x_0 + 2 \cdot n/d, \dots, x_0 + (d-1) \cdot n/d$ — exactly $d$ of them.

## 5.8 Systems of Linear Congruences

Q: How do you solve a system $a_1 x \equiv b_1 \pmod{n_1}, \dots, a_k x \equiv b_k \pmod{n_k}$?
A: (i) First solve each individually, getting $x \equiv c_i \pmod{m_i}$ (with $m_i \mid n_i$). (ii) Combine these via the Chinese Remainder Theorem (when the $m_i$ are pairwise coprime); when they aren't, use compatibility conditions. The next chapter handles the CRT mechanics.

## 5.9 Linear Diophantine Equations in More Variables

Q: When does $a_1 x_1 + a_2 x_2 + \dots + a_k x_k = c$ have an integer solution?
A: Iff $\gcd(a_1, a_2, \dots, a_k) \mid c$. Same structure as the two-variable case, with multivariate Bézout providing coefficients $\gcd(a_1,\dots,a_k) = \sum x_i a_i$. Parametrization of all solutions uses a $(k - 1)$-dimensional lattice of free parameters — more machinery but same principles.

P: Does $6x + 10y + 15z = 1$ have an integer solution?
S:
**IDENTIFY**: Multivariate Diophantine — check divisibility.

**PLAN**: Compute $\gcd(6, 10, 15)$ and check against $c = 1$.

**EXECUTE**:
1. $\gcd(6, 10) = 2$; $\gcd(2, 15) = 1$. So $\gcd(6, 10, 15) = 1$.
2. $1 \mid 1$, so a solution exists.
3. Example: $x = 1, y = -2, z = 1$: $6 - 20 + 15 = 1$ ✓.

**EVALUATE**: The gcd-divides-RHS test is a two-line solvability check for any linear Diophantine equation. If it fails, no solution exists without further work.

## 5.10 Nonlinear Diophantine Equations (Outlook)

Q: Why are nonlinear Diophantine equations much harder than linear ones?
A: Because no general method exists — Hilbert's 10th problem asks for an algorithm deciding whether a given polynomial Diophantine equation has integer solutions, and Matiyasevich (1970) proved NO such algorithm exists. Individual equations (Pell $x^2 - Dy^2 = 1$, Fermat $x^n + y^n = z^n$) require deep machinery. Linear Diophantine equations are the one fully-solved family.

Q: Name one famous nonlinear Diophantine equation and its resolution.
A: [Fermat's Last Theorem]: for $n \geq 3$, $x^n + y^n = z^n$ has no solutions in positive integers. Conjectured by Fermat (1637); proved by Andrew Wiles (1994) after $357$ years, using elliptic curves and modular forms. Illustrates how "simple" Diophantine equations can take centuries and entirely new mathematics to resolve.
