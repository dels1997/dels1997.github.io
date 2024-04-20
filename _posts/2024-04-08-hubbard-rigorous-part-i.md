---
layout: post
title: Some rigorous results on the Fermi-Hubbard model (Part I)
subtitle: Lieb's theorems
---
<script>
MathJax = {
  loader: {load: ['[tex]/upgreek', '[tex]/mathtools']},
  tex: {
    packages: {'[+]': ['upgreek'], '[+]': ['mathtools']},
    inlineMath: [['$', '$'], ['\\(', '\\)']]
  },
  svg: {
    fontCache: 'global'
  }
};
</script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js">
</script>
We provide an overview of the first of the two theorems used in author's master thesis _Quantum Algorithms_ [^1], that relies on symmetries, classical algorithms and VQE to solve the Fermi-Hubbard model. The purpose of this work is not to be concise and focus intensely on physical and historical implications of the results, but rather provide a detailed derivation and pedagogical approach to included steps from the technical side, i.e. how to get from each point in the proofs. The intuitive understanding can be found from the original papers and many that followed, left for the readers to explore on their own --- perhaps we come to this in a future work. However, let us still note that Theorem 2 from Lieb's seminal 1989 paper[^2] is the first formally proven example of ferromagnetism that stems from itinerant electrons.

To set the stage for both of Lieb's theorems, consider a general finite lattice $\Lambda$ and the Fermi-Hubbard Hamiltonian on it:

$$\begin{equation}
\mathbf H = \sum\limits_\sigma\sum\limits_{x,y\in\Lambda}
	t_{xy}  \mathbf c_{x\sigma}^\dagger  \mathbf c_{y\sigma} +
	\sum\limits_{x\in\Lambda} U_x \mathbf n_{x\uparrow} \mathbf n_{x\downarrow}\;\text,
\end{equation}$$

with the hopping terms bundled into a $\|\Lambda\|\times\|\Lambda\|$ matrix $\mathbf T\coloneqq \left[t_{xy} \right]_{x,y\in\Lambda}$.

In both calculation, we use reflections in spin space and make no reference to spatial symmetries, but rather some reasonable assumptions. First, we assume the hopping terms to be real and symmetric under site exchange, i.e. $\forall x,y\in\Lambda\colon t_{xy}\in\mathbb R \mathrel{\land} t_{xy} = t_{yx}$, since they arise as wavefunction overlaps. Second, we assume that the lattice $\Lambda$ is connected because otherwise its strongly connected components could simply be handled separately. Furthermore, the first theorem places no restrictions on lattice topology, while the second requires it to be bipartite[^3], with an even number of sites $(2\mid)\|\Lambda\|$. Bipartitedness naturally corresponds to the fact that there exist nonempty and disjoint $\Lambda_A$ and $\Lambda_B$ such that

$$\forall x, y\in\Lambda\colon \left(x\in \Lambda_A\mathrel{\land}y\in \Lambda_B\right)\implies t_{xy} = 0\;\text.$$

We further use $N$ to denote the total number of electrons in the system, with obvious Pauli principle bound $N\leq2\|\Lambda\|$.

The most pertinent operators are expectedly $\mathbf S$ and $\mathbf S^z$, which both commute with $\mathbf H$[^4] and thus allow for separation of the Hilbert space into sectors with well-defined quantum numbers:

$$\begin{align}
\mathbf S^z & = \frac12\sum\limits_{x\in\Lambda}\left[\mathbf n_{x\uparrow} - \mathbf n_{x\downarrow}\right]\;\text;\\
\mathbf S^+ & = \sum\limits_{x\in\Lambda}\mathbf c_{x\uparrow}^\dagger \mathbf c_{x\downarrow} = \left(\mathbf S^-\right)^\dagger\;\text;\\
\mathbf S^2 &= \left(\mathbf S^z\right)^2 + \frac12\left[
\mathbf S^+\mathbf S^- + \mathbf S^- \mathbf S^+
\right]\;\text.
\end{align}$$

As per usual, the eigenvalues of different operators are denoted by italicized and nonbold versions of the operators. This notation is used for their matrix elements as well, however differentiated by indices in the subscript.

<div style="border: 1px solid black;margin: 0 auto;padding: 15px 15px 15px 25px;">
<b>Theorem 1 (attractive case)</b>: Assume $\forall x\in\Lambda\colon U_x\leq0$ and  $N$ even. Then:
<ol>
	<li>Among the ground states of $\mathbf H$ given by $(1)$, there exists one with vanishing total spin $S = 0$;</li>
	<li>Furthermore, if $\forall x\colon U_x < 0$, the ground state is unique (and by part 1., of total spin $S = 0$).</li>
</ol>
</div>

In the limiting case of diverging $U_x\to-\infty$, i.e. strong attraction of electrons of opposite spins, they are clumped in up-down pairs on $N/2$ lattice sites and from $(4)$ we see that the all three terms null such a state since its total spin projection $(2)$ vanishes, while the ladder operators $(3)$ flip spin of single particles on each site, yielding zero due to the exclusion principle. Conversely, if $\forall x\in\Lambda\colon U_x=0$, then $\mathbf H = \mathbf T$ and the first $N/2$ levels of that matrix are each filled with two particles, if we recall the assumption of even $N$ (otherwise, one level contains a single excitation only). Thus, $S = 0$ by the same reasoning as before. This makes the theorem seem intuitively reasonable, so let us now prove it.

**Proof [Theorem 1 (attractive case)]**\
To simplify the following process, we use the fact that $\mathbf H$ commutes with $\mathbf S^{x,y,z}$, which can be taken as a set of generators of the $SU(2)$ spin rotation symmetry[^5]. Thus, the Hamiltonian is invariant upon rotations in the spin space. These generators commute with $\mathbf S^2$ as well since they simply change the spin projection as they rotate the statevectors along the Bloch spheres. Thus, due to $[\mathbf H, \mathbf S^2] = 0$, we conclude that for each $S$, all the different spin projections are of the same energy. This implies that we can always work in the zero projection $S^z = 0$ subspace, because each class of states with different $S$ has an element in it. Put in different terms, once any   element of the ground state manifold is found, it can be rotated into the $S^z = 0$ subspace without changing its energy. In that subspace, there are $n \coloneqq N/2$ electrons of each spin, so we can write every state in it using the following basis. For each spin type, let $\{\ket{\Psi_\sigma^\alpha}\}$ be a set of linearly independent statevectors that are in form of _real_ (justification to follow soon) homogeneous polynomials of order $n$ in different creation operators of said spin type, acting on the vacuum state. Again for each spin type, in any such set there are $m \coloneqq \binom{\|\Lambda\|}n$ states since they are homogeneous in creation operators and we can choose $n$ sites out of the total $\|\Lambda\|$ to place the particles of same spin, and none more due to the exclusion principle. Then a general state in the $\mathbf S^z = 0$ subspace can be written as

$$\begin{equation}
\ket\Psi =\sum\limits_{\alpha\beta} W_{\alpha\beta}
\ket{\Psi_\uparrow^\alpha}\ket{\Psi_\downarrow^\beta}\;\text,
\end{equation}$$

where $\mathbf W \coloneqq [W_{\alpha\beta}]{\alpha,\beta\in[1\cdot\cdot m]}$ is an $m\times m$ complex matrix, thus justifying the choice of real coefficients in all $\ket{\Psi_\sigma^\alpha}$.

Furthermore, it is useful to see that $\mathbf W$ can always be chosen to be Hermitian as follows. First, Hamiltonian is symmetric upon exchange of up and down spins. This turns a general $\ket\Psi$ state into

$$\begin{align*}
\ket\Psi = \sum\limits_{\alpha\beta} W_{\alpha\beta}
\ket{\Psi_\uparrow^\alpha}\ket{\Psi_\downarrow^\beta} \eqqcolon \ket{\Psi_{\mathbf W}}
\leadsto 
\ket{\Psi'} = &\sum\limits_{\alpha\beta} W_{\alpha\beta}
\ket{\Psi_\downarrow^\alpha}\ket{\Psi_\uparrow^\beta} =  \sum\limits_{\alpha\beta} W_{\beta\alpha}
\ket{\Psi_\downarrow^\beta}\ket{\Psi_\uparrow^\alpha} \sim \\
& \sum\limits_{\alpha\beta} W_{\beta\alpha}
\ket{\Psi_\uparrow^\alpha} \ket{\Psi_\downarrow^\beta}\;\text.
\end{align*}$$

Evidently, $\ket{\Psi'} \sim \ket{\Psi_{\mathbf W^{\text T}}}$ (up to a sign due to fermion anticommutation) and due to $\mathbf H$ invariance, if  $\mathbf W$ corresponds to a ground state, then so does $\mathbf W^{\text T}$. Furthermore, since $\ket{\Psi^\alpha}$ are with all coefficients real, the complex conjugate of $\ket{\Psi_{\mathbf W}}$ is equal to $\ket{\Psi_{\mathbf W^{\*}}}$. However, the complex conjugate of an eigenvector is an eigenvector as well, only with eigenvalue equal to the complex conjugate of the initial one, but eigenvalues of Hermitian $\mathbf H$ are real, so $\langle\mathbf H\rangle_{\Psi_{\mathbf W^{\*\text T} = \mathbf W^\dagger}} = \langle\mathbf H\rangle_{\Psi_{\mathbf W^{\text T}}} = \langle\mathbf H\rangle_{\Psi_{\mathbf W}}$. In other words, if $\mathbf W$ describes a ground state of $\mathbf H$, then so does $\mathbf W^\dagger$ and by linearity $\mathbf W + \mathbf W^\dagger$, which is manifestly Hermitian. Thus, we can always construct a ground state with Hermitian $\mathbf W$ and consequently assume that $\mathbf W = \mathbf W^\dagger$ from this point on. Based on this fact and statevector normalization, we further find

$$\begin{align*}
1 = \braket{\Psi|\Psi} =
\sum\limits_{\alpha_1\beta_1\alpha_2\beta_2} W_{\alpha_1\beta_1}^{*} W_{\alpha_2\beta_2}
\underbrace{\braket{\Psi_\uparrow^{\alpha_1}|\Psi_\uparrow^{\alpha_2}}}_{\delta_{\alpha_1\alpha_2}}
\underbrace{\braket{\Psi_\downarrow^{\beta_1}|\Psi_\downarrow^{\beta_2}}}_{\delta_{\beta_1\beta_2}} = \sum\limits_{\alpha\beta}W_{\alpha\beta}^*W_{\alpha\beta} = \sum\limits_{\alpha\beta} \lvert W_{\alpha\beta}\rvert^2\;\text,
\end{align*}$$

and note that the second sum is also equal to

$$\begin{align*}\tag{N}
1 =\sum\limits_{\alpha\beta}W_{\alpha\beta}^{*}W_{\alpha\beta} = 
\sum\limits_{\alpha\beta}W_{\beta\alpha}^{*\text T}W_{\alpha\beta} = 
\sum\limits_{\alpha\beta}W_{\beta\alpha}^\dagger W_{\alpha\beta} = 
\text{Tr}\left[\mathbf W^\dagger\mathbf W\right]
\overset{\mathbf W=\mathbf W^\dagger}{=} \text{Tr}\left[\mathbf W^2\right]\;\text,
\end{align*}$$

which we will use later on.

At this point, we have a generic ground state of the Hamiltonian in a convenient form and with some useful properties. Now let us consider the different contributions to expectation value $\langle\mathbf H\rangle_{\Psi}$. For the hopping terms, we find (as for $\ket{\Psi^\alpha}$ in $(5)$, we use $\mathbf c_x^\dagger$ and $\mathbf c_x$ for generic operators, with any of the two spins, but same in the whole expression):

<details style="text-align: center;"><summary markdown="span">$\braket{\Psi\|\sum\limits_{\sigma x y}t_{xy} \mathbf c_{x\sigma}^\dagger\mathbf c_{y\sigma}\|\Psi} = 2\sum\limits_{\alpha_1\alpha_2\beta} W_{\alpha_1\beta}^* W_{\alpha_2\beta}
\braket{\Psi^{\alpha_1}\| \sum\limits_{x y}t_{xy} \mathbf c_x^\dagger\mathbf c_y\|\Psi^{\alpha_2}}\;\text.$</summary>
$$\begin{align*}
&\braket{\Psi|\sum\limits_{\sigma x y}t_{xy} \mathbf c_{x\sigma}^\dagger\mathbf c_{y\sigma}|\Psi} =
\sum\limits_{\alpha_1\beta_1\alpha_2\beta_2} W_{\alpha_1\beta_1}^{*} W_{\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\alpha_1}\Psi_\downarrow^{\beta_1}}
\sum\limits_{\sigma x y}t_{xy} \mathbf c_{x\sigma}^\dagger\mathbf c_{y\sigma}
\ket{\Psi_\uparrow^{\alpha_2}\Psi_\downarrow^{\beta_2}} \\
&~~~~= \sum\limits_{\alpha_1\beta_1\alpha_2\beta_2} \sum\limits_{x y}W_{\alpha_1\beta_1}^{*} W_{\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\alpha_1}\Psi_\downarrow^{\beta_1}}
 t_{xy} \mathbf c_{x\uparrow}^\dagger\mathbf c_{y\uparrow} +
  t_{xy} \mathbf c_{x\downarrow}^\dagger\mathbf c_{y\downarrow}
\ket{\Psi_\uparrow^{\alpha_2}\Psi_\downarrow^{\beta_2}} \\
&~~~~= \sum\limits_{\alpha_1\beta_1\alpha_2\beta_2} \sum\limits_{x y}W_{\alpha_1\beta_1}^{*} W_{\alpha_2\beta_2}
\left[
\delta_{\beta_1\beta_2}
\braket{\Psi_\uparrow^{\alpha_1}|t_{xy} \mathbf c_{x\uparrow}^\dagger\mathbf c_{y\uparrow}|\Psi_\uparrow^{\alpha_2}} +
\delta_{\alpha_1\alpha_2}
\braket{\Psi_\downarrow^{\beta_1}| t_{xy} \mathbf c_{x\downarrow}^\dagger\mathbf c_{y\downarrow}|\Psi_\downarrow^{\beta_2}}
\right]\\
&~~~~= 2\sum\limits_{\alpha_1\alpha_2\beta} W_{\alpha_1\beta}^{*} W_{\alpha_2\beta}
\braket{\Psi^{\alpha_1}| \sum\limits_{x y}t_{xy} \mathbf c_x^\dagger\mathbf c_y|\Psi^{\alpha_2}}\;\text.
\end{align*}$$
</details>

If we now define an evidently real (recall that both $\ket{\Psi^\alpha}$ and $\mathbf T$ are real) and symmetric operator $\mathbf K$ via

$$\begin{align*}
K_{\alpha\beta} \coloneqq
\bra{\Psi^\alpha}
\sum\limits_{x y}t_{xy} \mathbf c_x^\dagger\mathbf c_y
\ket{\Psi^\beta}\;\text,
\end{align*}$$

then the expectation value of the hopping part of $\mathbf H$ becomes

<details style="text-align: center;"><summary markdown="span">$\bra{\Psi}\sum\limits_{\sigma x y}t_{xy} \mathbf c_{x\sigma}^\dagger\mathbf c_{y\sigma}\ket{\Psi} = 2\text{Tr}\left[\mathbf K\mathbf W^2\right]\;\text.$</summary>
$$\begin{align*}
\bra{\Psi}\sum\limits_{\sigma x y}t_{xy} \mathbf c_{x\sigma}^\dagger\mathbf c_{y\sigma}\ket{\Psi} &=
2\sum\limits_{\alpha_1\alpha_2\beta} W_{\alpha_1\beta}^* W_{\alpha_2\beta} K_{\alpha_1\alpha_2} = \{\mathbf W = \mathbf W^\dagger\}\\
&=
2\sum\limits_{\alpha_1\alpha_2\beta} W_{\beta\alpha_1} W_{\alpha_2\beta} K_{\alpha_1\alpha_2} \\
&=
2\sum\limits_{\alpha_1\alpha_2\beta} K_{\alpha_1\alpha_2} W_{\alpha_2\beta} W_{\beta\alpha_1}  \\
&= 2\text{Tr}\left[\mathbf K\mathbf W^2\right]\;\text.
\end{align*}$$
</details>

For the onsite interaction, the procedure is similar and yields:

<details style="text-align: center;"><summary markdown="span">$\bra{\Psi}\sum\limits_{x}U_x \mathbf n_{x\uparrow}\mathbf c_{x\downarrow}\ket{\Psi} = \sum\limits_x U_x\,\text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right]\;\text,$</summary>
$$\begin{align*}
\bra{\Psi}\sum\limits_{x}U_x \mathbf n_{x\uparrow}\mathbf c_{x\downarrow}\ket{\Psi} &=
\sum\limits_x U_x \sum\limits_{\alpha_1\beta_1\alpha_2\beta_2}
W_{\alpha_1\beta_1}^* W_{\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\alpha_1}}\mathbf n_{x\uparrow}\ket{\Psi_\uparrow^{\alpha_2}}
\bra{\Psi_\downarrow^{\beta_2}}\mathbf n_{x\downarrow}\ket{\Psi_\downarrow^{\beta_2}}\\
&=
\sum\limits_x U_x \sum\limits_{\alpha\beta}
W_{\alpha\beta}^* W_{\alpha\beta}
\bra{\Psi_\uparrow^{\alpha}}\mathbf n_{x\uparrow}\ket{\Psi_\uparrow^{\alpha}}
\bra{\Psi_\downarrow^{\beta}}\mathbf n_{x\downarrow}\ket{\Psi_\downarrow^{\beta}}\\
&=
\left\{\left(L_x\right)_{\alpha\beta} \coloneqq \bra{\Psi^{\alpha}}\mathbf n_x\ket{\Psi^{\beta}}\right\}\\
&=
\sum\limits_x U_x \sum\limits_{\alpha\beta}
W_{\beta\alpha}\left(L_x\right)_{\alpha\alpha}W_{\alpha\beta}\left(L_x\right)_{\beta\beta} \\
&=\sum\limits_x U_x\,\text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right]\;\text,
\end{align*}$$
</details>
with $\mathbf L_x$ defined by $\left(L_x\right)_{\alpha\beta} \coloneqq \bra{\Psi^{\alpha}}\mathbf n_x\ket{\Psi^{\beta}}$ real and symmetric as well.

Now the energy expectation value as dependent on $\mathbf W$ can be written succinctly

$$\begin{equation}
E(\mathbf W) = \braket{\Psi_{\mathbf W}|\mathbf H|\Psi_{\mathbf W}} =
2\text{Tr}\left[\mathbf K\mathbf W^2\right] +
\sum\limits_x U_x \text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right]\;\text.
\end{equation}$$

To use this result, we define a positive semidefinite operator $\|\mathbf W\|$ via $\|\mathbf W\|^2 = \mathbf W^2$. To show that such an operator exists, we argue as follows. First, since $\mathbf W$ is Hermitian, it is diagonalizable with some operator, and that operator diagonalizes $\mathbf W^2$ as well. Since the eigenvalues of a Hermitian matrix are real, their squares are positive and taking a square root of this diagonal operator, we are left with their absolute values on the diagonal. Thus, in this orthonormal basis, $\mathbf W=\text{diag}((w_i)\_{i\in[1\cdot\cdot m]})$ with $\forall i\colon w\_i\in\mathbb R$ and $\|\mathbf W\| = \text{diag}((\|w\_i\|)_{i\in[1\cdot\cdot m]})$. Since $\|\mathbf W\|$ is diagonal with real eigenvalues, it is Hermitian, and since its eigenvalues are moreover all nonnegative, it is by definition a positive semidefinite operator.

<details><summary markdown="span">(A simple example of such operators)</summary>
As a simple example, one can consider (and observe that in nondiagonal bases, there will be no general simple relation between the two operators elementwise):

$$\begin{align*}
\mathbf W &= \begin{bmatrix} 1 & 2\imath\\ -2\imath & 1 \end{bmatrix}=
\begin{bmatrix} -\imath & \imath\\ 1 & 1 \end{bmatrix}
\begin{bmatrix} -1 & 0\\ 0 & 3 \end{bmatrix}
\frac12 \begin{bmatrix} \imath & 1\\ -\imath & 1 \end{bmatrix}\text;~~~~
\mathbf W^2 = \begin{bmatrix} 5 & 4\imath\\ -4\imath & 5 \end{bmatrix}\;\text;\\
|\mathbf W| &= \begin{bmatrix} 2 & \imath\\ -\imath & 2 \end{bmatrix}\,\,\,\,\,=
\begin{bmatrix} -\imath & \imath\\ 1 & 1 \end{bmatrix}
\begin{bmatrix} 1 & 0\\ 0 & 3 \end{bmatrix}
\frac12 \begin{bmatrix} \imath & 1\\ -\imath & 1 \end{bmatrix}\text;~~~~
\,|\mathbf W|^2 = \begin{bmatrix} 5 & 4\imath\\ -4\imath & 5 \end{bmatrix}\;\text.
\end{align*}$$
</details>

In that same basis in which these operators are diagonal, we simply find that

<details style="text-align: center;"><summary markdown="span">$\text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right] = \text{Tr}\left[|\mathbf W|\mathbf L_x|\mathbf W|\mathbf L_x\right]\;\text.$</summary>
$$\begin{align*}
\text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right] &=
\sum\limits_{\alpha\beta\gamma\delta}
\mathbf W_{\alpha\beta}  (\mathbf L_x)_{\beta\gamma} \mathbf W_{\gamma\delta} (\mathbf L_x)_{\delta\alpha} \\
&= \sum\limits_{\alpha\gamma}
\mathbf W_{\alpha\alpha} \mathbf W_{\gamma\gamma} (\mathbf L_x)_{\alpha\gamma} (\mathbf L_x)_{\gamma\alpha} \\
&= \sum\limits_{ij} w_i w_j (\mathbf L_x)_{ij} (\mathbf L_x)_{ji} \\
&= \sum\limits_{ij} w_i w_j \left[(\mathbf L_x)_{ij}\right]^2 \\
&\leq \sum\limits_{ij} |w_i| |w_j| \left[(\mathbf L_x)_{ij}\right]^2 \\
&= \text{Tr}\left[|\mathbf W|\mathbf L_x|\mathbf W|\mathbf L_x\right]\;\text.
\end{align*}$$
</details>

Recalling that $\forall x\in\Lambda\colon U_x\leq0$, we find that

$$\begin{align}
E(\mathbf W)\geq E(|\mathbf W|)
\end{align}$$

for ground states, because the first terms in $(6)$ are identical. Thus, if for each $\mathbf W$ in the ground state manifold it held that $\mathbf W\neq\|\mathbf W\|$, then all those $\|\mathbf W\|$ states would have the energy equal to that of the ground state, and should thus be elements of said manifold as well. Therefore, there must exist a ground state such that $\mathbf W = \|\mathbf W\|$. Since $\|\mathbf W\|$ is by construction positive semidefinite, so is that particular $\mathbf W$. With a natural choice $\ket{\Psi^\alpha} \coloneqq \Pi_{x\in\alpha} \mathbf c_x^\dagger \ket{\mathbf 0}$ for concreteness, we find that $\exists \alpha\in[1\cdot\cdot \;m]\colon W_{\alpha\alpha} >0$, because otherwise $\mathbf W=\mathbf 0$ and represents a vacuum, not a system with $N$ particles. However, $(4)$ implies that $\mathbf S^2 \ket{\Psi_\uparrow^\alpha} \ket{\Psi_\downarrow^\alpha} = 0$ (as we have argued for limiting cases before the proof) so the state described by $\mathbf W$ has a nonvanishing component in the $S=0$ subspace, thus proving part 1. of Theorem 1.

---

Before moving on to part 2. of Theorem 1, let us derive the equation for $\mathbf W$ from $(6)$. We vary each term of the equation with respect to $W_{\alpha'\beta'}$ and use Hermiticity of $\mathbf W$.

<details style="text-align: center;"><summary markdown="span">$\frac\delta{\delta W_{\alpha'\beta'}}\braket{\Psi_{\mathbf W}|\mathbf H|\Psi_{\mathbf W}} = \left(2e\mathbf W\right)_{\beta'\alpha'}\;\text.$</summary>
$$\begin{align*}
\frac\delta{\delta W_{\alpha'\beta'}}&
\braket{\Psi_{\mathbf W}|\mathbf H|\Psi_{\mathbf W}} =
\frac\delta{\delta W_{\alpha'\beta'}}
\sum\limits_{\alpha_1\beta_1\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\alpha_1}}\bra{\Psi_\downarrow^{\beta_1}}
\underbrace{W_{\alpha_1\beta_1}^*}_{W_{\beta_1\alpha_1}} H_{\beta_1\alpha_2}W_{\alpha_2\beta_2}
\ket{\Psi_\uparrow^{\alpha_2}}\ket{\Psi_\downarrow^{\beta_2}} \\
&=
\sum\limits_{\alpha_1\beta_1\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\alpha_1}}\bra{\Psi_\downarrow^{\beta_1}}
\delta_{\alpha'\beta_1}\delta_{\beta'\alpha_1} H_{\beta_1\alpha_2}W_{\alpha_2\beta_2} +
W_{\beta_1\alpha_1} H_{\beta_1\alpha_2} \delta_{\alpha'\alpha_2}\delta_{\beta'\beta_2}
 \ket{\Psi_\uparrow^{\alpha_2}}\ket{\Psi_\downarrow^{\beta_2}} \\
&=
\sum\limits_{\alpha_2\beta_2}
\bra{\Psi_\uparrow^{\beta'}}\bra{\Psi_\downarrow^{\alpha'}}
H_{\alpha'\alpha_2}W_{\alpha_2\beta_2}
 \ket{\Psi_\uparrow^{\alpha_2}}\ket{\Psi_\downarrow^{\beta_2}} \\
 &~~~~~~~~~~~+
 \sum\limits_{\alpha_1\beta_1}
\bra{\Psi_\uparrow^{\alpha_1}}\bra{\Psi_\downarrow^{\beta_1}}
W_{\beta_1\alpha_1}H_{\beta_1\alpha'}
 \ket{\Psi_\uparrow^{\alpha'}}\ket{\Psi_\downarrow^{\beta'}} \\
 &=
 \underbrace{H_{\alpha'\beta'}}_{H_{\beta'\alpha'}}W_{\beta'\alpha'} + W_{\beta'\alpha'}H_{\beta'\alpha'} \\
 &= \left\{H_{\beta'\alpha'}\text{ is the energy of }\beta'\alpha'\text{ state; as in the original paper, we denote it }e\right\}\\
 &= \left(2e\mathbf W\right)_{\beta'\alpha'}\;\text;
\end{align*}$$
</details>

<details style="text-align: center;"><summary markdown="span">$\frac\delta{\delta W_{\alpha'\beta'}}2\text{Tr}\left[\mathbf K\mathbf W^2\right] = \left(2\left[\mathbf W\mathbf K + \mathbf K\mathbf W\right]\right)_{\beta'\alpha'}\;\text.$</summary>
$$\begin{align*}
\frac\delta{\delta W_{\alpha'\beta'}}&
2\text{Tr}\left[\mathbf K\mathbf W^2\right] =
2\frac\delta{\delta W_{\alpha'\beta'}}
\sum\limits_{\alpha\beta\gamma}
K_{\alpha\beta}W_{\beta\gamma}W_{\gamma\alpha} =\\
&=
2\sum\limits_{\alpha\beta\gamma}
K_{\alpha\beta}\delta_{\alpha'\beta}\delta_{\beta'\gamma}W_{\gamma\alpha} + 
2\sum\limits_{\alpha\beta\gamma}
K_{\alpha\beta}W_{\beta\gamma}\delta_{\alpha'\gamma}\delta_{\beta'\alpha} \\
&=
2\sum\limits_\alpha
K_{\alpha\alpha'}W_{\beta'\alpha} + 
2\sum\limits_\beta
K_{\beta'\beta}W_{\beta\alpha'}\\
&= 2(\mathbf W\mathbf K)_{\beta'\alpha'} + 2\left(\mathbf K\mathbf W\right)_{\beta'\alpha'}\\
&= \left(2\left[\mathbf W\mathbf K + \mathbf K\mathbf W\right]\right)_{\beta'\alpha'}\;\text;
\end{align*}$$
</details>

<details style="text-align: center;"><summary markdown="span">$\frac\delta{\delta W_{\alpha'\beta'}}\sum\limits_x U_x \text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right] = \left(2\sum\limits_x U_x \mathbf L_x\mathbf W\mathbf L_x\right)_{\beta'\alpha'}\;\text.$</summary>
$$\begin{align*}
\frac\delta{\delta W_{\alpha'\beta'}}&
\sum\limits_x U_x \text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right] =
\sum\limits_x U_x
\frac\delta{\delta W_{\alpha'\beta'}}
\sum\limits_{\alpha\beta\gamma\delta}
W_{\alpha\beta} (L_x)_{\beta\gamma} W_{\gamma\delta} (L_x)_{\delta\alpha} = \\
&=
\sum\limits_x U_x \sum\limits_{\alpha\beta\gamma\delta}\left[
\delta_{\alpha'\alpha}\delta_{\beta'\beta} (L_x)_{\beta\gamma} W_{\gamma\delta} (L_x)_{\delta\alpha} +
W_{\alpha\beta} (L_x)_{\beta\gamma} \delta_{\alpha'\gamma}\delta_{\beta'\delta} (L_x)_{\delta\alpha}
\right] \\
&=
\sum\limits_x U_x \left[
\sum\limits_{\gamma\delta}
(L_x)_{\beta'\gamma} W_{\gamma\delta} (L_x)_{\delta\alpha'} +
\sum\limits_{\alpha\beta}
W_{\alpha\beta} (L_x)_{\beta\alpha'} (L_x)_{\beta'\alpha}
\right] \\
&=
\sum\limits_x U_x \left[
(\mathbf L_x\mathbf W\mathbf L_x)_{\beta'\alpha'} +
(\mathbf L_x\mathbf W\mathbf L_x)_{\beta'\alpha'}\right] \\
&= \left(2\sum\limits_x U_x \mathbf L_x\mathbf W\mathbf L_x\right)_{\beta'\alpha'}
\end{align*}$$
</details>

to get:

$$\begin{equation}
e\mathbf W = \mathbf W\mathbf K + \mathbf K\mathbf W + \sum\limits_{x\in\Lambda} U_x \text{Tr}\left[\mathbf W\mathbf L_x\mathbf W\mathbf L_x\right]\;\text.
\end{equation}$$

---

We now turn to part 2. of Theorem 1. First, let us prove that when $\forall x\in\Lambda\colon U_x<0$, it must hold either $\mathbf W = -\|\mathbf W\|$ or $\mathbf W = \|\mathbf W\|$. To this end, note that $\mathbf R\coloneqq \|\mathbf W\| - \mathbf W$ is a Hermitian positive semidefinite operator with kernel $\mathcal Q\coloneqq \left\\{V\colon \mathbf RV=0\right\\}$. This operator is positive semidefinite since in the diagonal basis the elements of $\|\mathbf W\|$ are larger or equal to those of $\mathbf W$. For an arbitrary $V\in\mathcal Q$ and $\mathbf W\leadsto\mathbf R$ (found by subtracting equations for $\|\mathbf W\|$ and $\mathbf W$; they both hold if $\mathbf W$ describes a ground state due to $(7)$), it is simple to find the expectation value of $(8)$ because the LHS and the first two terms on the RHS vanish by definition of $\mathcal Q$

$$\begin{align*}
e\braket{V|\mathbf R|V} =
\braket{V|\mathbf K\mathbf R + \mathbf R\mathbf K + \sum\limits_x U_x \mathbf L_x\mathbf R\mathbf L_x |V}\implies
\sum\limits_x U_x
\braket{V| \mathbf L_x\mathbf R\mathbf L_x |V} = 0\;\text,
\end{align*}$$

but by $\forall x\in\Lambda\colon U_x<0$ and positive semidefiniteness of $\mathbf L_x\mathbf R\mathbf L_x$ (as a product of such operators), we immediately find that

$$\begin{equation*}
\forall x\in\Lambda,\forall V\in\mathcal Q:
\braket{V|\mathbf L_x\mathbf R\mathbf L_x |V} = 0\;\text.
\end{equation*}$$

From this and positive semidefiniteness of $\mathbf R$, necessarily $\forall V\in\mathcal Q\colon\mathbf R\mathbf L_x V = 0$ so $\mathbf L_x$ preserves $\mathcal Q$, i.e. maps it to itself: $\mathbf L_x\mathcal Q=\mathcal Q$. Using $(8)$ to act on an arbitrary $V\in\mathcal Q$ with $\mathbf W\leadsto\mathbf R$ as before, the LHS vanishes and so do the second and third terms on the RHS (because $\mathbf R$ and $\mathbf L_x$ null all $V\in\mathcal Q$), but then so does the first term on the RHS, implying that $\forall V\in\mathcal Q\colon \mathbf R\mathbf KV=0$, so $\mathbf K$ preserves $\mathcal Q$ as well: $\mathbf K\mathcal Q=\mathcal Q$.

Now let us define $\mathbf L^\alpha\coloneqq \Pi_{x\in\alpha}\mathbf L_x$ which projects onto a basis vector $\mu^\alpha$ in the space of $m\times m$ complex matrices with $\alpha$ and $\gamma$ both equal to some configuration of $m$ raised particles, and is simply determined by $\left(\mu^\alpha\right)\_\gamma = \delta\_{\alpha\gamma}$ (if the particle configurations differ at all, their overlap vanishes). This can be seen from its matrix elements, which are found immediately if we recall that it contains the number operators only: $\left(L^\alpha\right)\_{\gamma\delta} = \delta\_{\alpha\gamma}\delta\_{\gamma\delta}$. Furthermore, since each $\mathbf L_x$ preserves $\mathcal Q$, then so does each $\mathbf L^\alpha$ as a product of such operators.

On the other hand, let us consider some two configurations of particles $\alpha$ and $\beta$ and assume that they differ, but of course contain the same number of particles, i.e. they are among the $m$ possible configurations. Then if $\Lambda$ is strongly connected, we are able to simply relate states of any two configurations $\alpha$ and $\beta$ using two $\mathbf K$ terms, one for each spin species.

<details><summary markdown="span">(Hopping term example)</summary>
For example, on a lattice with $\|\Lambda\|=4$ sites and $N=2$ particles, each of spin up, we consider the following two configurations of up spins. $\alpha$ with one spin up on site $1$ and one spin up on site $2$ and $\beta$ with one spin up on site $1$ and one spin up on site $3$. Then the corresponding matrix element is

$$\begin{align*}
K_{\alpha\beta} &= \braket{\Psi_\uparrow^\alpha|\sum\limits_{x,y\in\{1,2,3,4\}}
t_{xy}\mathbf c_{x\uparrow}^\dagger \mathbf c_{y\uparrow} |\Psi_\uparrow^\beta}=\\
&=
\braket{\mathbf 0| \mathbf c_{2\uparrow} \mathbf c_{1\uparrow}
\left(\sum\limits_{x,y\in\{1,2,3,4\}}
t_{xy}\mathbf c_{x\uparrow}^\dagger \mathbf c_{y\uparrow}\right)
\mathbf c_{1\uparrow}^\dagger \mathbf c_{3\uparrow}^\dagger
|\mathbf 0} \\
&= t_{23}\;\text,
\end{align*}$$

as is expected physically.
</details>

We recall the assumption that $\Lambda$ is connected and in this way, we see an intuitive procedure of relating any two configurations $\alpha$ and $\beta$. Focusing on one spin type (since the other can be handled analogously and separately), let us assume that these two configurations differ in a particular site. Then, due to connectedness, there must exist a hopping path between the two sites, and the amplitude it results in will be a string of nonzero $K\_{\alpha'\beta'}$ terms, because there is nonvanishing overlap in each of the subsequent steps due to the existence of this path. We can then iteratively do this for each of the sites and then repeat the procedure for other spin type. In this way, the configurations $\alpha$ and $\beta$ are related by a finite (possibly zero) number of terms of such terms. Let their number be some $p\in\mathbb N$, and the intermediate configurations labeled by $\{\gamma_i\}\_{i\in[1\cdot\cdot\;p]}$. Then the coefficient

$$\begin{equation}
G_{\alpha\beta}\coloneqq
K_{\alpha\gamma_1}K_{\gamma_1\gamma_2}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta}
\end{equation}$$

is nonzero. For nontrivial $\mathcal Q$ (the converse will be handled after), there exists $V\in\mathcal Q$ such that $V\neq0$ and thus some configuration $\alpha$ such that $\mathbf L^\alpha V\neq 0$ (because $\sum\limits\_\alpha \mathbf L^\alpha=\mathbb 1$ since these operators are projectors onto the $\mu$ basis) and consequently $\mathbf L^\alpha V=z\mu^\alpha$ for some $z\neq0$.

Now one can define a vector in $\mathcal Q$ using this fact and $(9)$ in an explicit operator form in which it will be evident that it is an element of $\mathcal Q$, because $\left(L^\alpha\right)\_{\gamma\delta} = \delta\_{\alpha\gamma}\delta\_{\gamma\delta}$:

<details style="text-align: center;"><summary markdown="span">$G_{\alpha\beta} = \left(\mathbf K\mathbf L_x^{\gamma_1}\mathbf K\mathbf L_x^{\gamma_2}\mathbf K \cdots
\mathbf K\mathbf L_x^{\gamma_{p-1}}\mathbf K\mathbf L_x^{\gamma_p}\mathbf K \right)_{\alpha\beta}\;\text,$</summary>
$$\begin{align*}
G_{\alpha\beta}=K_{\alpha\gamma_1}K_{\gamma_1\gamma_2}K_{\gamma_2\gamma_3}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta} &=
\sum\limits_{\gamma_1'\gamma_2'}
K_{\alpha\gamma_1'}K_{\gamma_2'\gamma_2}
\delta_{\gamma_1\gamma_1'}\delta_{\gamma_1\gamma_2'}K_{\gamma_2\gamma_3}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta} \\
&=
\sum\limits_{\gamma_1'\gamma_2'}
K_{\alpha\gamma_1'} (L_x^{\gamma_1})_{\gamma_1'\gamma_2'} K_{\gamma_2'\gamma_2}
K_{\gamma_2\gamma_3}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta} \\
&=
\left(\mathbf K\mathbf L_x^{\gamma_1}\mathbf K\right)_{\alpha\gamma_2} 
K_{\gamma_2\gamma_3}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta} \\
&=
\left(\mathbf K\mathbf L_x^{\gamma_1}\mathbf K\mathbf L_x^{\gamma_2}\mathbf K\right)_{\alpha\gamma_3} 
K_{\gamma_3\gamma_4}\cdots K_{\gamma_{p-1}\gamma_p}
K_{\gamma_p\beta} \\
&=
\left(
\mathbf K\mathbf L_x^{\gamma_1}\mathbf K\mathbf L_x^{\gamma_2}\mathbf K \cdots
\mathbf K\mathbf L_x^{\gamma_{p-1}}\mathbf K\mathbf L_x^{\gamma_p}\mathbf K
\right)_{\alpha\beta}\;\text,
\end{align*}$$
</details>

implying that $\mathbf G$ preserves $\mathcal Q$, because all of its constituents do as well.

We can now define a vector using some arbitrary configuration $\beta$ and the aforementioned particular configuration $\alpha$

$$\begin{equation}
F^{(\beta)}\coloneqq \mathbf L^\beta\mathbf G\mathbf L^{\alpha}V\;\text,
\end{equation}$$

where the indexing is done by $\beta$ since $\alpha$ is some particular fixed configuration and we can choose arbitrary $\beta$. We first note that this vector is evidently in $\mathcal Q$ since $V$ is and all the operators acting on it map $\mathcal Q$ to itself. Second, components of this vector are found in the following simple form

<details style="text-align: center;"><summary markdown="span">$F_\gamma^{(\beta)} = zG_{\beta\alpha}(\mu^\beta)_\gamma\;\text,$</summary>
$$\begin{align*}
F_\gamma^{(\beta)} &= \left[\mathbf L^\beta\mathbf G\mathbf L^{\alpha}V\right]_\gamma =
\left[\mathbf L^\beta\mathbf G z\mu^{\alpha}\right]_\gamma \\
&=z\sum\limits_{\gamma_1\gamma_2} (L^\beta)_{\gamma\gamma_1}
G_{\gamma_1\gamma_2}  (\mu^\alpha)_{\gamma_2} \\
&=z\sum\limits_{\gamma_1\gamma_2}
\delta_{\beta\gamma}\delta_{\beta\gamma_1}
G_{\gamma_1\gamma_2}
\delta_{\alpha\gamma_2} \\
&=
z\delta_{\beta\gamma} G_{\beta\alpha} \\
&= zG_{\beta\alpha}(\mu^\beta)_\gamma\;\text,
\end{align*}$$
</details>

allowing us to write $F^{(\beta)}$ as

$$\begin{equation*}
F^{(\beta)} = z G_{\beta\alpha}\mu^\beta\;\text.
\end{equation*}$$

In other words, by choosing different $\beta$, we find that all base elements $\mu^\beta$ are contained in $\mathcal Q$, implying that $\mathcal Q$ is the whole space because kernels are vector spaces and thus linear. Since $\mathcal Q$ is the kernel of operator $\mathbf R$, we conclude that $\mathbf R=\mathbf 0$ so by its definition: $\mathbf W = \|\mathbf W\|$. It is important to observe that this conclusion rests upon the fact that $\mathcal Q$ is nontrivial, i.e. the existence of nonzero $V$ in it. However, the converse case is handled easily because if $\mathcal Q$ is trivial, then there are no nonzero vectors that $\mathbf R$ maps to zero. Since $\mathbf W$ and $\|\mathbf W\|$ are simultaneously diagonalizable, in common diagonal basis this implies that none of their eigenvalues are equal, because otherwise that diagonal element would vanish upon subtraction in definition of $\mathbf R$, so the corresponding eigenvalue would be in $\mathcal Q$. However, if none of these pairs are equal, because they are related by an absolute value, none of the eigenvalues of $\mathbf W$ can be positive (otherwise it would be equal to its own absolute value) and are thus all negative, so in this case $\mathbf W = -\|\mathbf W\|$.

From $\mathbf W = \pm\|\mathbf W\|$, we can now finally prove part 2. of Theorem 1 via contradiction. Suppose that there exist two normalized --- in sense of $(\text{N})$ --- ground states described by $\mathbf W_1$ and $\mathbf W_2$ such that $\mathbf W_1\neq \pm \mathbf W_2$. Then for every $d\in\mathbb R$, we can define $\mathbf W^{(d)} \coloneqq \mathbf W_1 + d\mathbf W_2$ which is evidently nonzero and (after normalization) a ground state of the system due to $(8)$ being linear in $\mathbf W$. Furthermore, due to $\mathbf W = \pm \|\mathbf W\|$, $\mathbf W_1$ and $\mathbf W_2$ are either p.s.d. or n.s.d. We can assume WLOG that both $\mathbf W_1$ and $\mathbf W_2$ are, for example, p.s.d. because if they are of different definiteness, we can simply employ $d\leadsto-d$, or change sign of the whole expression (evidently not changing the fact that the total operator is neither p.s.d. nor n.s.d.), and  by combining these two operations, all four cases are handled analogously. Thus, with this assumption, we conclude that $\text{Tr}\mathbf W_1 > 0$ and $\text{Tr}\mathbf W_2 > 0$ because otherwise all their eigenvalues would be zero. But due to linearity of trace, $\text{Tr}\mathbf W^{(d)} = \text{Tr}\mathbf W_1+d\,\text{Tr}\mathbf W_2$  and by choosing an obvious $d$, we get $\text{Tr}\mathbf W^{(d)} = 0$. Since $\mathbf W_1$ and $\mathbf W_2$ are not scalar multiples of one another, it is not possible that all eigenvalues of $\mathbf W^{(d)}$ are zero. But if at least one of them is negative (positive), there ought to exist at least one positive (negative) so that they could sum up to vanishing trace. Thus, we conclude that for this choice of $d$, the operator $\mathbf W^{(d)}$ is neither p.s.d. nor n.s.d. and therefore does not describe a ground state of the system[^6].

<details><summary markdown="span">Another line of reasoning to show that $\mathbf W^{(d)}$ is neither p.s.d. nor n.s.d., due to Ilja Gogić, and also valid on infinite-dimensional Hilbert spaces.[^7]</summary>
For an arbitrary Hermitian matrix $\mathbf A$, define $m(\mathbf A)$ and $M(\mathbf A)$ to be the minimum and maximum eigenvalues of $\mathbf A$, respectively. Then $m$ and $M$ as functions from the set of such matrices of a given order to real numbers are continuous, e.g. because one way to write them is using the spectral norm $\lVert\cdot\rVert_{\text s}$:

$$\begin{align*}
M(\mathbf A) &= \lVert\lVert\mathbf A\rVert_{\text s}\mathbb 1 + \mathbf A\rVert_{\text s}  -
\lVert\mathbf A\rVert_{\text s}\;\text;\;\\
m(\mathbf A) &= - M(\mathbf A)\;\text,
\end{align*}$$

as can easily be seen in the diagonal basis of Hermitian matrix $\mathbf A$.

As before, we take arbitrary ground state matrices $\mathbf W_1$ and $\mathbf W_2$ that are p.s.d., but not scalar multiples of each other and write $\mathbf W^{(d)}$ as a function of $d$, i.e. $\mathbf W(d) \coloneqq \mathbf W_1 + d\mathbf W_2$. Then for any vector $\mathbf x$ and any nonnegative $d$, evidently

$$\begin{align*}
\langle\mathbf W(d)\rangle_{\mathbf x} = \langle\mathbf W_1\rangle_{\mathbf x} + d\langle\mathbf W_2\rangle_{\mathbf x}\geq0\;\text,
\end{align*}$$

due to both $\mathbf W_1$ and $\mathbf W_2$ being p.s.d. Consequently, $\forall d\geq 0\colon m(\mathbf W(d))\geq0$.

Furthermore, by a simple bound in form of

$$\begin{align*}
\mathbf W(d) = \mathbf W_1 + d\mathbf W_2 \leq \mathbb1 M(\mathbf W_1) + d\mathbf W_2
\end{align*}$$

and noting that $\mathbb1 M(\mathbf W_1) + d\mathbf W_2$ has at least one negative value in its spectrum for all $d < -M(\mathbf W_1) / M(\mathbf W_2)$ for nonzero $M(\mathbf W_2)$ (satisfied because otherwise, since $\mathbf W_2$ is p.s.d., it would be null and thus not correspond to a ground state), we conclude that so does $\mathbf W(d)$. Thus, $d\mapsto m(\mathbf W(d))$ is negative on $\langle-\infty,-M(\mathbf W_1) / M(\mathbf W_2)\rangle$. If we now denote the smallest zero of this function by $d_0$, then for an arbitrary $\varepsilon>0$, it holds that $m(\mathbf W(d_0-\varepsilon))<0$. Such a point exists because, as was observed before, this function is nonnegative for nonnegative $d$, and negative on the aforementioned interval -- thus, it attains a zero value.

Furthermore, since $m(\mathbf W(d\_0))=0$, it follows that $M(\mathbf W(d\_0))\neq0$ because otherwise $\mathbf W(d\_0)=\mathbf 0$, which is impossible since $\mathbf W\_1$ and $\mathbf W\_2$ are not scalar multiples of each other. Therefore, $M(\mathbf W(d\_0))>0$.

Now we can use the continuity of $d\mapsto m(\mathbf W(d))$ to conclude that there exists some $\varepsilon_0>0$ such that $M(\mathbf W(d\_0-\varepsilon_0))>0$, but we have already argued that for any $\varepsilon_0>0$, $m(\mathbf W(d_0-\varepsilon_0))<0$.

In other words, there exists a parameter choice $d\coloneqq d_0-\varepsilon_0$ such that $\mathbf W(d_0 -\varepsilon_0)$ and consequently $\mathbf W^{(d\_0-\varepsilon_0)}$ is neither p.s.d. nor n.s.d.
</details>

In other words, the assumption that there exist two distinct normalized ground state vectors was erroneous, proving the uniqueness of the ground state and consequently part 2. of Theorem 1. $\blacksquare$

---

<div style="border: 1px solid black;margin: 0 auto;padding: 15px 15px 5px 25px;">
<b>Theorem 2 (repulsive case)</b>: Assume $\forall x\in\Lambda\colon U_x = U>0$,  $\Lambda$ bipartite with $|\Lambda|$ even and $N=|\Lambda|$ (half-filling). Then the ground state of $\mathbf H$ given by $(1)$ is unique (apart from the trivial $(2S + 1)$-fold spin projection $SU(2)$ degeneracy) and has spin
$$\begin{equation}
S = \frac12\lvert|\Lambda_A| - |\Lambda_B|\rvert\;\text.
\end{equation}$$
</div>

As in the original paper, we follow an intuitive argument to motivate why in at least a part of the ground state manifold, it makes sense that $S= \frac12\lvert\|\Lambda_A\| - \|\Lambda_B\|\rvert$. Naively, it seems that for $U\to0$ we can follow the same handwavy argument as for Theorem 1 and conclude that the spin of the ground state vanishes. However, it is important to note that $\mathbf H=\mathbf T$ in this case cannot simply be filled up to $N/2$ levels due to bipartitedness --- when the lattice is bipartite, the hopping matrix $\mathbf T$ and therefore the Hamiltonian are of form

$$\begin{equation}\tag{12}
\mathbf T =
\begin{bmatrix}
    \begin{array}{c|c}
  \mathbf 0_{\Lambda_A} & \mathbf T_{AB} \\
  \hline
  \vphantom{\left[A\right]^\dagger}
  \mathbf T_{BA} \left(= \mathbf T_{AB}^{\text T}\right) & \mathbf 0_{\Lambda_{B}}
  \end{array}
  \end{bmatrix}\;\text,
\end{equation}$$

with $\mathbf T_{AB}$ of dimension $\|\Lambda_A\|\times\|\Lambda_B\|$ and $\mathbf T_{BA}$ of dimension $\|\Lambda_B\|\times\|\Lambda_A\|$ and equal to transpose of $\mathbf T_{AB}$ due to condition on symmetry of $t_{xy}$ upon site exchange. The two vanishing diagonal blocks would correspond to hopping terms between different sites in $\Lambda_A$ or $\Lambda_B$ and are not permitted by assumption. From this and the fact that the rank of a matrix is equal to that of its determinant, we see that the rank of $\mathbf T$ is at most equal to $2\min\{\|\Lambda_A\|, \|\Lambda_B\|\}$, due to this being twice the lower dimension of each of the blocks in given equation.

From this, it follows that $\mathbf T$ has at least

$$\begin{equation}\tag{13}
|\Lambda| - 2\min\{|\Lambda_A|, |\Lambda_B|\} = \max\{|\Lambda_A|, |\Lambda_B|\} - \min\{|\Lambda_A|, |\Lambda_B|\}
\end{equation}$$

zero eigenvalues, and the remaining are in plus-minus pairs. The latter fact can be seen as follows for $\|\Lambda_A\|=\|\Lambda_B\|$. Let $V^{(+)}=\begin{pmatrix}v_{\Lambda_A}\\ v_{\Lambda_B}\end{pmatrix}$ be some eigenvector of $\mathbf T$ with eigenvalue $\lambda$, conveniently separated into parts in a given bipartition. Then from simple block form of $(12)$, the corresponding eigenvalue relation yields a system of two equations: $\mathbf T_{AB} v_{\Lambda_B}=\lambda v_{\Lambda_A}$ and $\mathbf T_{BA} v_{\Lambda_A}=\lambda v_{\Lambda_B}$. From this, it is somewhat intuitive (in a purely mathematical sense) to define $V^{(-)}\coloneqq\begin{pmatrix}v_{\Lambda_B}\\ -v_{\Lambda_A}\end{pmatrix}$. Based on the previously found two equations, it is straightforward to see that

$$\begin{align*}
\mathbf T V^{(-)}=
\begin{pmatrix}
-\mathbf T_{AB}v_{\Lambda_A}\\\mathbf T_{BA}v_{\Lambda_B}
\end{pmatrix}\overset{\mathbf T=\mathbf T^{\text T}}{=}
\begin{pmatrix}
-\mathbf T_{BA}v_{\Lambda_A}\\\mathbf T_{AB}v_{\Lambda_B}
\end{pmatrix}=
\begin{pmatrix}
-\lambda v_{\Lambda_B}\\\lambda v_{\Lambda_A}
\end{pmatrix}=
-\lambda
\begin{pmatrix}
v_{\Lambda_B}\\- v_{\Lambda_A}
\end{pmatrix}=-\lambda
V^{(-)}\;\text.
\end{align*}$$

To demonstrate the same conclusion in general case $\|\Lambda_A\|\neq\|\Lambda_B\|$, a more involved calculation is needed, along with separation of these vectors into an overlapping part of the two subspaces as well. Thus, we leave it as an exercise and show this fact in a more elegant way (the number of zero eigenvalues of $\mathbf T$ will follow as well).

The determinant of a general matrix separated into blocks

$$\begin{equation*}
\mathbf M=
\begin{bmatrix}
\mathbf C_1&\mathbf C_2\\\mathbf C_3&\mathbf C_4
\end{bmatrix}\;\text,
\end{equation*}$$

can be written, under the condition that $\mathbf C\_1$ is invertible, as

$$\begin{equation}\tag{14}
\det\mathbf M = \det\mathbf C_1\det\left(\mathbf C_4 - \mathbf C_3\mathbf C_1^{-1}\mathbf C_2\right)\;\text.
\end{equation}$$

To find the eigenvalues of $\mathbf T$, we solve the usual equation $\det\left(\mathbf T-\lambda\mathbb1\right) = 0$. In this case, for the matrix we seek the determinant of it holds that $\mathbf C\_1=-\lambda\mathbb1\_{\Lambda\_A}$, $\mathbf C\_2 = \mathbf T\_{BA}$, $\mathbf C\_3 = \mathbf T\_{AB}$, $\mathbf C\_4=-\lambda\mathbb1\_{\Lambda\_B}$ so the LHS of eigenvalue equation $(14)$ can be expanded as (ignoring the irrelevant sign factor $(-1)^{\|\Lambda\|}$)

$$\begin{align*}\tag{15}
\det\left(\lambda\mathbb 1_{\Lambda_A}\right)
\det\left(\lambda\mathbb 1_{\Lambda_B} - \mathbf T_{BA}\frac1\lambda\mathbb 1_{\Lambda_B}\mathbf T_{AB}\right)=
\lambda^{|\Lambda_A|}
\det\left(\lambda\mathbb 1_{\Lambda_B} - \mathbf T_{BA}\frac1\lambda\mathbb 1_{\Lambda_B}\mathbf T_{AB}\right)\;\text,
\end{align*}$$

where we note that all the matrix dimensions are indeed matched, as is needed for a matrix product on the RHS.

$$\begin{align*}
\lambda^{|\Lambda_A|-|\Lambda_B|}
\det\left(
\lambda^2\mathbb 1_{\Lambda_B}
-\mathbf T_{BA}\mathbf T_{AB}
\right)\;\text,
\end{align*}$$

with potentially nonzero values evidently always in pairs due to $\lambda^2$ and the fact that the product of a real matrix (which $\mathbf T\_{AB}$ and $\mathbf T\_{BA}$ are, because $\mathbf T$ is) with its transpose has the same rank as the initial matrix, and there are evidently total of $2\|\Lambda\_B\| = 2\min\{\|\Lambda\_A\|, \|\Lambda\_B\|\}$ of these values, as argued before. Furthermore, by simply considering the $\|\Lambda\_A\|\leq\|\Lambda\_B\|$ case, we both find that the formula for the number of nonzero eigenvalues agrees with the previously obtained one (because the rank of, e.g. $\mathbf T\_{AB}$ now becomes $\min\{\|\Lambda\_A\|,\|\Lambda\_B\|\}=\|\Lambda\_A\|$), as well as that the exponent of the preceding factor can in a more general case be written exactly as the RHS of $(13)$, as expected since it is the multiplicity of the zero eigenvalue.

Recall that all this was to motivate the total spin of the ground state from Theorem 2 in the limit of vanishing onsite interaction $U=0$.

**Proof [Theorem 2 (repulsive case)]**\
To prove this theorem we employ the particle-hole spin transformation (Shiba map) given by the following relations for an arbitrary $x\in\Lambda$

$$\begin{align}\tag{16}
\mathbf c_{x\uparrow}\leadsto\epsilon(x)\mathbf c_{x\uparrow}^\dagger\;\text;~~~
\mathbf c_{x\uparrow}^\dagger&\leadsto\epsilon(x)\mathbf c_{x\uparrow}\;\text,~~~
\mathbf c_{x\downarrow}\leadsto\mathbf c_{x\downarrow}\;\text,~~~
\mathbf c_{x\downarrow}^\dagger\leadsto\mathbf c_{x\downarrow}^\dagger\text,\\
\forall x\in\Lambda\colon\epsilon(x)&=\begin{cases}
1, & x\in\Lambda_A\\
-1, & x\in\Lambda_B
\end{cases}\;\text.
\end{align}$$

Evidently, the down spin number operators $\mathbf n_{x\downarrow}$ are unchanged, while the up spin number operators $\mathbf n_{x\uparrow}$ are transformed into $\epsilon^2(x)\mathbf c_{x\uparrow}\mathbf c_{x\uparrow}^\dagger=\mathbb 1-\mathbf n_{x\uparrow}$. Furthermore, the spin operators are transformed to pseudospin operators (we don't write their transformations explicitly here and leave them as an exercise, because they are completely analogous to the succeeding Hamiltonian transformation):

$$\begin{align}
\mathbf S^z&\leadsto
\mathbf{\tilde S}^z = \frac12\left[|\Lambda| - \mathbf N_\uparrow - \mathbf N_\downarrow\right]\;\text;\\
\mathbf S^+&\leadsto
\mathbf{\tilde S}^+=\sum\limits_{x\in\Lambda}\epsilon(x)\mathbf c_{x\uparrow}\mathbf c_{x\downarrow}\;\text;\\
\mathbf S^2&\leadsto
\mathbf{\tilde S}^2 = \left(\mathbf{\tilde S}^z\right)^2 + \frac12\left[
\mathbf{\tilde S}^+\mathbf{\tilde S}^- + \mathbf{\tilde S}^-\mathbf{\tilde S}^+
\right]\;\text.
\end{align}
$$

We can now find the transformed Hamiltonian by recalling the assumption of bipartitedness, i.e. no hopping terms between $\Lambda\_A$ and $\Lambda\_B$ (so we split the hopping terms in two parts, depending on the hopping direction)

<details style="text-align: center;"><summary markdown="span">$\mathbf H\leadsto \sum_{xy}t_{xy} \mathbf c_{y\sigma}^\dagger \mathbf c_{x\sigma}
-U\sum_x \mathbf n_{x\uparrow}\mathbf n_{x\downarrow} + U\mathbf N_\downarrow \eqqcolon \mathbf {\tilde H} + U\mathbf N_\downarrow\;\text.$</summary>
$$\begin{align*}
\mathbf H&\leadsto
\frac12\sum_{xy} t_{xy}\;
\underbrace{\epsilon(x\in\Lambda_A)\epsilon(y\in\Lambda_B)}_{-1\text{ from } (16)}
\;\mathbf c_{x\sigma}\mathbf c_{y\sigma}^\dagger +
\frac12\sum_{xy} t_{xy}\;
\underbrace{\epsilon(x\in\Lambda_B)\epsilon(y\in\Lambda_A)}_{-1\text{ from } (16)}
\;\mathbf c_{x\sigma}\mathbf c_{y\sigma}^\dagger \\
&~~~~~~~~~~+
U\sum_x\left[\mathbb 1 - \mathbf n_{x\uparrow}\right]\mathbf n_{x\downarrow} ~~~~~~~~~~~~~~~~~~ \left\{\mathbf N_\downarrow\coloneqq\sum_x \mathbf n_{x\downarrow}\right\}\\
&= -\sum_{xy}t_{xy} \mathbf c_{x\sigma}\mathbf c_{y\sigma}^\dagger + U\mathbf N_\downarrow -U\sum_x \mathbf n_{x\uparrow}\mathbf n_{x\downarrow} =\\
&= \sum_{xy}t_{xy} \mathbf c_{y\sigma}^\dagger \mathbf c_{x\sigma}  -U\sum_x \mathbf n_{x\uparrow}\mathbf n_{x\downarrow} + U\mathbf N_\downarrow \\
&\eqqcolon \mathbf {\tilde H} + U\mathbf N_\downarrow\;\text.
\end{align*}$$
</details>

In this expression, we find the second part irrelevant to the ground state choice because upon the described transformation, we easily see that the initial half-filling condition $\mathbf N\_\uparrow + \mathbf N\_\downarrow=\|\Lambda\|$ turns into $\mathbf N\_\uparrow=\mathbf N\_\downarrow$ so there is no difference of this term's energy between different possible states.


Furthermore, we see from definition of $\mathbf{\tilde H}$ that it is of the same form as the initial Hamiltonian $(1)$ with $U\_x=-U<0$ for all $x\in\Lambda$ and thus satisfies the assumptions of part 2. of Theorem 1. In other words, $\mathbf{\tilde H}$ and thus the whole transformed Hamiltonian has a unique ground state with spin $S=0$. Evidently, because the transformation $(16)$ is idempotent of order $2$, after applying it once more (or its inverse), $\mathbf {\tilde H}$ is transformed back into $\mathbf H$, while $\mathbf S$ is transformed to pseudospin operator $\mathbf {\tilde S}$. In other words, the ground state of the initial Hamiltonian $\mathbf H$ is unique and has pseudospin zero $\tilde S=0$.

On the other hand, proving the rest of Theorem 2, i.e. that $S = \frac12\lvert\|\Lambda\_A\| - \|\Lambda\_B\|\rvert$, is not as straightforward. As a rough outline, we note that the just proved nondegeneracy of the ground state of $\mathbf H$ for all $U>0$ implies that the spin of said unique ground state must not depend on value of $U$ --- otherwise, since $U$ is a real parameter and can thus be varied continuously, we could find ground states of different spins using simple perturbation theory. However, for $U\to\infty$, the Hamiltonian at hand is equivalent up to a leading order to the Heisenberg antiferromagnetic Hamiltonian, that has, for $S^z=0$, a unique ground state of total spin $S=\frac12\lvert\|\Lambda\_A\|-\|\Lambda\_B\|\rvert$. The uniqueness of this AFM Hamiltonian ground state implies the existence of (perhaps an arbitrarily small, but finite) gap, and thus for a large enough value of $U$, the $S$ values of ground states of the two Hamiltonians will be the same, proving Theorem 2 in full[^8].  $\blacksquare$


[^1]: Karlo Delić. [_Quantum Algorithms_](https://repozitorij.pmf.unizg.hr/islandora/object/pmf:12738). 2024.
[^2]: [E. H. Lieb. _Two Theorems on the Hubbard Model_. Physical Review Letters **62**. 1989.](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.62.1201)
[^3]: This statement actually relates only to $t_{xy}$ and not the lattice $\Lambda$ per se, but we still conventionally say that $\Lambda$ is bipartite, implicitly making it inseparable from $t_{xy}$.
[^4]: The hopping terms in $(1)$ create/destroy particles of the same spin in pairs, while the onsite term only counts them, so $\mathbf S^z$ is conserved. Conservation of $\mathbf S^2$ then follows from a short calculation  using $(4)$.
[^5]: A generic $U(n)$ matrix has $n^2$ free parameters, so an additional condition of unit determinant results in $n^2-1$ of them, yielding the same number of generators.
[^6]: Great thanks to Elliott H. Lieb for illuminating answers.
[^7]: Great thanks to [Ilja Gogić](https://web.math.pmf.unizg.hr/~ilja/) for the argument.
[^8]: Perhaps we come to an extended discussion of these theorems in a future post.
