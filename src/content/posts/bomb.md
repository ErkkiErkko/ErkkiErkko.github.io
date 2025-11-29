---
title: Analysis for Noisy Elitzur-Vaidman Bomb Detection
published: 2025-11-30
description: This work gives an upper bound for the error in the Elitzur-Vaidman bomb detection protocol with arbitrary noises.
tags: [Quantum]
category: Quantum
draft: false
---

## 1 Preparing Works

In most cases, we are not able to know the specific form (bit-flip channel, phase-flip channel, etc) of the noise in the black box, so it would be difficult to develop a protocol to significantly develop the success rate in the next section. However, we want to know the success rate of the original protocol in a more general situation. This section gives an analysis towards this problem when the scale of noise is limited.

The basic idea of the following analysis is that if the quantum noise in the black box has an upper bound, its influence on the testing process should also be limited and thus could be estimated. To describe the upper bound of an quantum noise, we introduce the definitions of metric space and metric in mathematics.

**Definition 1 (Metric Space and Metric).** Formally, a metric space is an ordered pair $(M, d)$ where $M$ is a set and $d$ is a metric on $M$, i.e., a function

$$d : M \times M \to [0, +\infty)$$

satisfying the following axioms for all points $x, y, z \in M$:

- $d(x, y) = 0$ if and only if $x = y$,
- (Symmetry) $d(x, y) = d(y, x)$,
- (Triangle Inequality) $d(x, z) \leq d(x, y) + d(y, z)$.

Intuitively, the metric describes the distance between two elements of a metric space. When it comes to the space of all density operators, to define a metric on it, we first give the definition of fidelity.

**Definition 2 (Fidelity).** The fidelity of state $\rho$ and $\sigma$ is defined to be

$$F(\rho, \sigma) = \text{tr}\sqrt{\rho^{1/2}\sigma\rho^{1/2}}$$

Specially, for a pure state $|\psi\rangle$ and an arbitrary state $\rho$, the fidelity between could be calculated

$$F(|\psi\rangle, \rho) = \text{tr}\sqrt{\langle\psi|\rho|\psi\rangle|\psi\rangle\langle\psi|} = \sqrt{\langle\psi|\rho|\psi\rangle}.$$

Furthermore, the fidelity between two pure states $|\phi\rangle, |\psi\rangle$ is

$$F(|\phi\rangle, |\psi\rangle) = |\langle\phi|\psi\rangle|.$$

From the fact that $\sqrt{UAU^\dagger} = U\sqrt{A}U^\dagger$ for any positive operator $A$ and unitary operator $U$, we could easily know that fidelity is invariant under unitary transforms, which means

$$F(U\rho U^\dagger, U\sigma U^\dagger) = F(\rho, \sigma), \quad \forall\text{density operators } \rho, \sigma, \text{ unitary operator } U.$$

It is clear that the fidelity itself is not a metric over all density operators, since the fidelity of two same states is 1, rather than expected 0. To prove more useful property of fidelity, it is necessary to introduce Uhlmann's theorem.

**Theorem 1 (Uhlmann's Theorem).** Suppose $\rho$ and $\sigma$ are states of a quantum system $Q$. Introduce a second quantum system $R$ which is a copy of $Q$. Then

$$F(\rho, \sigma) = \max_{|\psi\rangle,|\phi\rangle} |\langle\psi|\phi\rangle|,$$

where the maximization is over all purification $|\psi\rangle$ of $\rho$ and $|\phi\rangle$ of $\sigma$ into $RQ$.

You could see the proof of this theorem on Nielsen and Chuang's book. Furthermore, we could fix the purification of $\rho$, that is $|\psi\rangle$, and the maximization is over all purifications of $\sigma$.

Using Uhlmann's theorem, we could easily see that $0 \leq F(\rho, \sigma) \leq 1$ always holds, and fidelity is symmetric in its inputs. Moreover, since the purifications of different states are always different, we have $F(\rho, \sigma) < 1$ if $\rho \neq \sigma$. Although fidelity is not a metric over density operators, we could define a metric based on fidelity, which is called angle.

**Definition 3 (Angle).** The angle between states $\rho$ and $\sigma$ is defined to be

$$A(\rho, \sigma) \equiv \arccos F(\rho, \sigma).$$

From the properties of fidelity discussed above, we could derive that:

- The angle is non-negative, and is equal to zero if and only if $\rho = \sigma$.
- The angle is symmetric in its inputs.

In fact, the angle also obey the triangle equation, that is

$$A(\rho, \tau) \leq A(\rho, \sigma) + A(\sigma, \tau).$$

It could be proved using Uhlmann's theorem again: let $|\phi\rangle$ be the purification of $\sigma$, and choose purifications $|\psi\rangle$ of $\rho$ and $|\gamma\rangle$ of $\tau$ such that

$$F(\rho, \sigma) = \langle\psi|\phi\rangle, \quad F(\sigma, \tau) = \langle\phi|\gamma\rangle.$$

Here we need to let $\langle\psi|\gamma\rangle$ be real and positive by multiplying appropriate phase factors. From some obvious facts about vectors in three dimensions, we have

$$\arccos(\langle\psi|\gamma\rangle) \leq \arccos(\langle\psi|\phi\rangle) + \arccos(\langle\phi|\gamma\rangle) = A(\rho, \sigma) + A(\sigma, \tau).$$

From Uhlmann's theorem, we have $F(\rho, \tau) \geq \langle\psi|\gamma\rangle$, and therefore $A(\rho, \tau) \leq \arccos(\langle\psi|\gamma\rangle)$. Combining this with the inequality above, we have the triangle inequality of the angle

$$A(\rho, \tau) \leq A(\rho, \sigma) + A(\sigma, \tau).$$

Hence, the angle is a metric defined over all density operators.

Moreover, the angle is also invariant under unitary transforms, which means

$$A(U\rho U^\dagger, U\sigma U^\dagger) = A(\rho, \sigma), \quad \forall\text{density operators } \rho, \sigma, \text{ unitary operator } U.$$

Following the basic idea mentioned at the beginning of this section, we assume a limitation on the influence of quantum noise in the black box using the angle, i.e.,

$$A(\rho, E(\rho)) \leq \epsilon, \quad \forall \text{ density operator } \rho,$$

where $E$ is the quantum operation referring to the noise, and $\epsilon > 0$.

To estimate the danger of igniting the bomb and other properties under the noise, we also need to examine the rotation along y-axis for an angle $\theta$, which is $R_y(\theta)$, using the angle metric. Before doing this, we need to introduce the following theorem.

**Theorem 2 (Strong Concavity of the Fidelity).** Let $p_i$ and $q_i$ be probability distributions over the same index set, and $\rho_i$ and $\sigma_i$ density operators also indexed by the same index set. Then

$$F\left(\sum_i p_i\rho_i, \sum_i q_i\sigma_i\right) \geq \sum_i \sqrt{p_iq_i}F(\rho_i, \sigma_i).$$

**Proof**

Using Uhlmann's theorem, we choose $|\psi_i\rangle$ and $|\phi_i\rangle$ the purifications of $\rho_i$ and $\sigma_i$ such that $F(\rho_i, \sigma_i) = \langle\psi_i|\phi_i\rangle$. Then, we introduce an ancillary system which has orthonormal basis states $|i\rangle$ corresponding to the index set $i$, and define

$$|\psi\rangle \equiv \sum_i \sqrt{p_i}|\psi_i\rangle|i\rangle, \quad |\phi\rangle \equiv \sum_i \sqrt{q_i}|\phi_i\rangle|i\rangle.$$

Here we note that $|\psi\rangle$ and $|\phi\rangle$ are also purifications of $\sum_i p_i\rho_i$ and $\sum_i q_i\sigma_i$ respectively. Hence, we could apply Uhlmann's theorem again and get the expected result:

$$F\left(\sum_i p_i\rho_i, \sum_i q_i\sigma_i\right) \geq |\langle\psi|\phi\rangle| = \left|\sum_i \sqrt{p_iq_i}\langle\psi_i|\phi_i\rangle\right| = \sum_i \sqrt{p_iq_i}F(\rho_i, \sigma_i).$$

There is a direct corollary of this theorem, called the joint concavity of fidelity.

**Corollary 1 (Joint Concavity of Fidelity).**

$$F\left(\sum_i p_i\rho_i, \sum_i p_i\sigma_i\right) \geq \sum_i p_iF(\rho_i, \sigma_i).$$

With the joint concavity of fidelity, we could prove the following lemma about the property of $R_y(\theta)$.

**Lemma 1** About the unitary operator $R_y(\theta)$, we have

$$F(\rho, R_y(\theta)\rho R_y(\theta)^\dagger) \geq \left|\cos\frac{\theta}{2}\right|$$

for any quantum state $\rho$.

**Proof**

From the Corollary 2.1 (Joint Concavity of Fidelity), we could only consider pure states $|\psi\rangle$.

$$F(|\psi\rangle, R_y(\theta)|\psi\rangle) = |\langle\psi|R_y(\theta)|\psi\rangle| = \left|\cos\frac{\theta}{2}\right|$$

Hence, rewrite this result using the angle metric, we have

$$A(\rho, R_y(\theta)\rho R_y(\theta)^\dagger) \leq \frac{\theta}{2}$$

for $\theta \in [0, \pi]$.

So far, we have made all the necessary preparing works for the analysis of the original protocol under the influence of noise. In the next two subsections, we will analyse the success rate of the protocol when there is or is not bomb in the black box respectively.

## 2 When the Box is a Bomb

Here we examine the situation that a bomb is inside the black box and has the danger to explode. Suppose that there are noises both before $(E_1)$ and after $(E_2)$ the measurement in the black box, and both noises are limited by $\epsilon$ using the angle metric, i.e.,

$$A(\rho, E_i(\rho)) \leq \epsilon, \quad \forall \text{ density operator } \rho, i = 1, 2.$$

If the outcome of one measurement in the black box is 0, the state would change to

$$E_1(R_y(\theta)E_2(|0\rangle\langle0|)R_y(\theta)^\dagger)$$

before the next measurement in the box. Using triangle inequality of the angle, we have

$$A(|0\rangle\langle0|, E_1(R_y(\theta)E_2(|0\rangle\langle0|)R_y(\theta)^\dagger)) < 2\epsilon + \frac{\theta}{2}.$$

Back to fidelity, it could be easily derived that

$$F(|0\rangle\langle0|, E_1(R_y(\theta)E_2(|0\rangle\langle0|)R_y(\theta)^\dagger)) > \cos\left(2\epsilon + \frac{\theta}{2}\right).$$

Since $F(|\psi\rangle, \rho) = \sqrt{\langle\psi|\rho|\psi\rangle}$ for a pure state $|\psi\rangle$ and an arbitrary state $\rho$, we get

$$\langle0|E_1(R_y(\theta)E_2(|0\rangle\langle0|)R_y(\theta)^\dagger)|0\rangle > \cos^2\left(2\epsilon + \frac{\theta}{2}\right).$$

Noting that the left side of the inequalities is the probability of getting result 0 in the next measurement in the black box, which means the bomb is not ignited, we have

$$p(\text{safe in the next measurement}) > \cos^2\left(2\epsilon + \frac{\theta}{2}\right),$$

$$p(\text{explode in the next measurement}) < \sin^2\left(2\epsilon + \frac{\theta}{2}\right) \leq \left(2\epsilon + \frac{\theta}{2}\right)^2.$$

for the next measurement.

Using the above upper bound of probability to explode in one measurement and some numerical methods, we have the following estimation for the whole testing process:

- $\epsilon = 0.01$, choose $\theta = \frac{\pi}{26}$, $p(\text{safe}) > 0.8448$,
- $\epsilon = 0.001$, choose $\theta = \frac{\pi}{251}$, $p(\text{safe}) > 0.9830$,
- $\epsilon = 0.0001$, choose $\theta$ possibly small, $p(\text{safe}) \to 0.9994+$,
- $\epsilon = 0.00001$, choose $\theta$ possibly small, $p(\text{safe}) \to 0.9999+$.
- ...

Besides, even if the testing process completes safely, there is still a small chance for the final measurement to give a wrong answer claiming that there is no bomb in the black box. This error probability is relatively small, since it is only due to the noise after the last measurement in the black box, i.e., the state before the final measurement to determine the existence of a bomb is $E_2(|0\rangle\langle0|)$. It is quite easy to compute that because $A(|0\rangle\langle0|, E_2(|0\rangle\langle0|)) < \epsilon$, we have $F(|0\rangle\langle0|, E_2(|0\rangle\langle0|)) > \cos(\epsilon)$, and thus $p(\text{correct}) > \cos^2(\epsilon)$.

## 3 When the Box is Empty

In this subsection, we need to study the success rate of the original protocol when there is no bomb in the black box, which means the only thing inside the box we need to take into consideration is the quantum noise $E$. Like the previous subsection, the noise is also limited using the angle metric:

$$A(\rho, E(\rho)) \leq \epsilon, \quad \forall \text{ density operator } \rho.$$

To show the success rate, here we prove the following lemma.

**Lemma 2** After $m$ ($m \geq 1$) steps of iterations in the original protocol, i.e., after $m$-th rotation $R_y(\theta)$, the state is denoted by $\rho_m$. We have

$$A(\rho_m, R_y(\theta)^m|0\rangle\langle0|(R_y(\theta)^\dagger)^m) < m\epsilon.$$

**Proof**

We proof this lemma using induction.

For $m = 1$, $\rho_1 = R_y(\theta)E(|0\rangle\langle0|)R_y(\theta)^\dagger$. From the limitation on the noise $(E)$, we already have

$$A(E(|0\rangle\langle0|), |0\rangle\langle0|) < \epsilon.$$

Because of the invariance of angle under unitary transforms, we could derive that

$$A(\rho_1, R_y(\theta)|0\rangle\langle0|R_y(\theta)^\dagger) < \epsilon.$$

If this proposition holds for $m = k$, consider the situation of $m = k + 1$, where $\rho_{k+1} = R_y(\theta)E(\rho_k)R_y(\theta)^\dagger$. According to the assumption, we already have

$$A(\rho_k, R_y(\theta)^k|0\rangle\langle0|(R_y(\theta)^\dagger)^k) < k\epsilon.$$

Use triangle inequality of the angle metric, and remember the limitation on the noise, we could get

$$A(E(\rho_k), R_y(\theta)^k|0\rangle\langle0|(R_y(\theta)^\dagger)^k) < (k + 1)\epsilon.$$

Again, use the invariance of angle under unitary transforms, it is clear that

$$A(\rho_{k+1}, R_y(\theta)^{k+1}|0\rangle\langle0|(R_y(\theta)^\dagger)^{k+1}) < (k + 1)\epsilon$$

Therefore, this lemma holds for all $m \in \mathbb{N}$.

If the whole process of original protocol requires $n$ iterations, then $R_y(\theta)^n|0\rangle\langle0|(R_y(\theta)^\dagger)^n = |1\rangle\langle1|$. Combine it with the result of Lemma 2.2, for the final measurement after all $n$ iterations,

$$A(\rho_n, |1\rangle\langle1|) < n\epsilon.$$

From this inequality, the lower bound of success rate could be derived like the previous section:

$$p(\text{correct}) > \cos^2(n\epsilon).$$

## 4 Discussion

In this whole section, we assume a limitation on the quantum noise using the angle metric. In fact, we could also use other distance measure other than angle. Obviously, limitation on the fidelity would make sense, because it is nearly equivalent to the angle using the formula

$$A(\rho, \sigma) = \arccos F(\rho, \sigma).$$

There is another distance measure of density operators called trace distance. Its definition is described below.

**Definition 4 (Trace Distance).** The trace distance between quantum states $\rho$ and $\sigma$ is

$$D(\rho, \sigma) \equiv \frac{1}{2}\text{tr}|\rho - \sigma|$$

where $|A| \equiv \sqrt{A^\dagger A}$.

Since the following relation holds for trace distance and fidelity

$$1 - F(\rho, \sigma) \leq D(\rho, \sigma) \leq \sqrt{1 - F(\rho, \sigma)^2}.$$

We could also assume such limitation on the trace distance.