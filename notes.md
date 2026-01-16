# 🎓 IQIQML Course Notes

## 📚 Lecture 1: Introduction & Quantum Platforms
*Goal: Understand the "physical" and "software" landscape of quantum computing before diving into the math.*

### 1. The "Hardware" of Quantum Computers
Unlike classical computers which use silicon transistors (switches that are either OFF or ON), quantum computers need physical systems that can exist in a "superposition" (both states at once). Different companies build these systems differently.

**Key Technologies to Know:**
* **Superconducting Qubits** (The most common type):
    * *Who:* **IBM**, Google, Rigetti, IQM.
    * *How:* They use electronic circuits cooled down to near absolute zero (cryogenic temperatures). They behave like artificial atoms.
    * *Pros/Cons:* They are fast to control but lose their quantum state (decoherence) quickly.
* **Ion Traps**:
    * *Who:* **IonQ**, Quantinuum.
    * *How:* They use electromagnetic fields to trap individual charged atoms (ions) in a vacuum.
    * *Pros/Cons:* Very stable and accurate, but slower operation speeds.
* **Neutral Atoms**:
    * *Who:* Pasqal, QuEra.
    * *How:* Atoms trapped by lasers (optical tweezers).
* **Photonic**:
    * *Who:* Xanadu.
    * *How:* Using particles of light (photons).

### 2. The "Software" Ecosystem
You don't plug wires into a quantum computer; you send it code over the internet (cloud).

**A. Amazon Braket**
Think of this as the "Amazon Marketplace" for quantum computers.
* It aggregates different hardware providers (IonQ, Rigetti, Oxford Quantum Circuits) into one platform.
* You use the **Amazon Braket SDK** (Software Development Kit) in Python to write code once and run it on different machines.

**B. IBM Quantum (The focus of this course)**
IBM builds both the hardware (Eagle/Osprey processors) and the software.
* **Qiskit**: The specific software library (SDK) used to program IBM's machines. It is written in **Python**.
* **Access**: You can run code via the *IBM Quantum Platform* (website) or install Qiskit locally on your computer.

### 3. Practical Setup (Exam Note: Know the tools)
To pass this course, you are expected to use **Qiskit**.
* **Language**: Python.
* **Environment**: Visual Studio Code (VSC) or Jupyter Notebooks.
* **Installation**: You use the Python package manager `pip`.
    * Command: `pip install qiskit`
    * visualization tools: `pip install qiskit[visualization]`

---

## 🧮 Lecture 2: The Quantum State of a Single Qubit
*Goal: Understand how we represent a qubit mathematically using vectors. This requires a small crash course in Linear Algebra.*

### 1. The Mathematical Foundation (Postulate I)
In classical physics, we describe a ball by its position $(x, y, z)$. In quantum mechanics, we describe a system by a **State Vector**.

**The Vector Space (Hilbert Space):**
* Imagine a 2D graph. A "vector" is just an arrow starting from the origin $(0,0)$.
* **Hilbert Space ($H$)**: This is just a fancy name for the "space" where our quantum arrows live. For a single qubit, this space is 2-dimensional (complex space $\mathbb{C}^2$).
* **Dirac Notation (The "Ket" $|\psi\rangle$)**:
    * Instead of writing a vector with an arrow on top like $\vec{v}$, quantum physicists write it inside a "ket": $|\psi\rangle$.
    * $|\psi\rangle$ simply represents the state of the qubit.

### 2. What is a Qubit?
A classical bit is either **0** or **1**.
A **Qubit** (Quantum Bit) is a vector that can be a combination of 0 and 1.

**The Computational Basis:**
We define two standard "directions" (basis vectors):
* $|0\rangle$: The state representing "0" (usually "spin up" or "ground state").
    * Matrix form: $\begin{pmatrix} 1 \\ 0 \end{pmatrix}$
* $|1\rangle$: The state representing "1" (usually "spin down" or "excited state").
    * Matrix form: $\begin{pmatrix} 0 \\ 1 \end{pmatrix}$

**Superposition:**
A qubit can be in a state $|\psi\rangle$ that is a linear combination of these two:
$$|\psi\rangle = c_0 |0\rangle + c_1 |1\rangle$$
* $c_0$ and $c_1$ are **Probability Amplitudes**.
* **Crucial Note**: These numbers ($c_0, c_1$) are **Complex Numbers** (they can have imaginary parts, e.g., $a + bi$).

### 3. Normalization (The "Rule of 1")
In probability, the chance of *something* happening must be 100% (or 1.0).
Since quantum mechanics is probabilistic, the state vector must be "normalized" to have a length of 1.

**The Formula:**
$$|c_0|^2 + |c_1|^2 = 1$$
* $|c_0|^2$ is the probability of measuring **0**.
* $|c_1|^2$ is the probability of measuring **1**.

**Example:**
If $|\psi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle$:
* Probability of 0: $(\frac{1}{\sqrt{2}})^2 = 0.5$ (50%)
* Probability of 1: $(\frac{1}{\sqrt{2}})^2 = 0.5$ (50%)
* Total: $0.5 + 0.5 = 1.0$ (Valid state ✅)

If you are given a vector that doesn't sum to 1, you must **normalize** it by dividing by its length (norm).

### 4. The Bloch Sphere (Visualizing the Qubit)
Since $c_0$ and $c_1$ are complex numbers, it's hard to draw them on a 2D piece of paper. However, we use a trick called the **Bloch Sphere**.

* Imagine a ball with radius 1.
* **North Pole**: Represents state $|0\rangle$.
* **South Pole**: Represents state $|1\rangle$.
* **Equator**: Represents superposition states (like 50/50 mixes).

**The Equation:**
Any pure qubit state can be written using two angles, $\theta$ (latitude) and $\varphi$ (longitude):
$$|\psi\rangle = \cos(\frac{\theta}{2})|0\rangle + e^{i\varphi}\sin(\frac{\theta}{2})|1\rangle$$
* $\theta$ (Theta): Controls how much "0" vs "1" you have.
    * If $\theta = 0$, you are at the North Pole ($|0\rangle$).
    * If $\theta = \pi$ (180°), you are at the South Pole ($|1\rangle$).
    * If $\theta = \pi/2$ (90°), you are on the equator (Superposition).
* $\varphi$ (Phi): The **Relative Phase**. It rotates the state around the Z-axis (like spinning a globe). It doesn't change the probability of measuring 0 or 1, but it changes how the qubit interferes with others.

### 5. Measurement (Z-Measurement)
When we measure a qubit, we can't "see" the vector $|\psi\rangle$. We only see a **0** or a **1**.
* This is the "collapse" of the wavefunction.
* If we measure in the standard **Z-basis** (checking if it points North or South):
    * The outcome "0" occurs with probability $p_0 = |\langle 0 | \psi \rangle|^2$.
    * The outcome "1" occurs with probability $p_1 = |\langle 1 | \psi \rangle|^2$.
* **Post-Measurement State**: After you see a "0", the qubit *becomes* $|0\rangle$. It forgets it was ever in a superposition.

---

### 📝 Exam Cheat Sheet (Summary)

| Concept | Definition | Math / Matrix |
| :--- | :--- | :--- |
| **Ket** | A column vector representing state | $|\psi\rangle = \begin{pmatrix} c_0 \\ c_1 \end{pmatrix}$ |
| **Bra** | A row vector (conjugate transpose) | $\langle\psi| = (c_0^*, c_1^*)$ |
| **Bra-Ket** | Inner product (measures overlap) | $\langle\phi|\psi\rangle$ (Scalar number) |
| **Normalization** | Requirement for total prob. to be 1 | $\sqrt{\langle\psi|\psi\rangle} = 1$ |
| **Basis |0>** | "Ground" state (North Pole) | $\begin{pmatrix} 1 \\ 0 \end{pmatrix}$ |
| **Basis |1>** | "Excited" state (South Pole) | $\begin{pmatrix} 0 \\ 1 \end{pmatrix}$ |
| **Bloch Vector** | 3D representation of the state | Vector $\vec{n}$ inside the sphere |

**Important Python/Qiskit Note:**
In Qiskit, the command `plot_bloch_vector` creates the sphere visualization. The standard simulator `qasm_simulator` mimics a real quantum computer by running the circuit many times ("shots") to estimate the probabilities (e.g., if you run 100 shots and get '0' 50 times, probability is 0.5).


## 🛠️ Lecture 3: Operators & Matrix Representations
*Goal: Learn the "verbs" of quantum computing. If a qubit is a vector, an operator is a machine that transforms that vector into a new one.*

### 1. What is a Linear Operator?
In simple terms, a linear operator is a function $\hat{A}$ that takes a vector and spits out a new vector.
* **Linearity**: It must follow two rules:
    1.  [cite_start]**Additivity**: $\hat{A}(|\psi\rangle + |\varphi\rangle) = \hat{A}|\psi\rangle + \hat{A}|\varphi\rangle$[cite: 8115].
    2.  [cite_start]**Homogeneity**: $\hat{A}(c|\psi\rangle) = c\hat{A}|\psi\rangle$ (where $c$ is a number)[cite: 8117].

### 2. The "Outer Product" (Ket-Bra)
You know the inner product (Bra-Ket $\langle a | b \rangle$) produces a number.
The **Outer Product** (Ket-Bra $|b\rangle\langle a|$) produces an **operator (matrix)**.

* **Definition**: An operator $\hat{P}_{ba} = |b\rangle\langle a|$ acts on a vector $|\psi\rangle$ like this:
    $$(|b\rangle\langle a|) |\psi\rangle = |b\rangle (\langle a | \psi \rangle)$$
    * [cite_start]*Translation:* It calculates the overlap (dot product) between $a$ and $\psi$, and scales the vector $|b\rangle$ by that amount[cite: 8131].

* **Projection Operators ($\hat{P}$)**:
    If you make an operator out of the *same* basis vector, like $\hat{P}_0 = |0\rangle\langle 0|$, it acts as a filter.
    * [cite_start]$\hat{P}_0$ keeps the part of the vector pointing along $|0\rangle$ and deletes the rest[cite: 8257].
    * **Identity Operator ($\hat{\mathbb{I}}$)**: The "do nothing" operator. It is the sum of all projectors: $\hat{\mathbb{I}} = |0\rangle\langle 0| + [cite_start]|1\rangle\langle 1|$[cite: 8446].

### 3. Matrix Representation
Since vectors are columns of numbers, operators are **matrices** (grids of numbers).
* If you have a basis $\{|0\rangle, |1\rangle\}$, the element in row $i$ and column $j$ of operator $\hat{A}$ is:
    $$A_{ij} = \langle i | \hat{A} | j \rangle$$
* *Student Tip:* To find the matrix of a gate, look at what it does to the basis vectors $|0\rangle$ and $|1\rangle$. The result of acting on $|0\rangle$ becomes the **first column**. [cite_start]The result of acting on $|1\rangle$ becomes the **second column**[cite: 8470].

### 4. Important Quantum Gates (Cheat Sheet)
You must recognize these matrices.

| Gate | Symbol | Matrix | Effect |
| :--- | :--- | :--- | :--- |
| **Hadamard** | **H** | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix}$ | [cite_start]Creates superposition ($|0\rangle \to |+\rangle$, $|1\rangle \to |-\rangle$)[cite: 8525]. |
| **Phase (S)** | **S** | $\begin{pmatrix} 1 & 0 \\ 0 & i \end{pmatrix}$ | [cite_start]Adds $90^{\circ}$ ($i$) phase to $|1\rangle$[cite: 8550]. |
| **T Gate** | **T** | $\begin{pmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \end{pmatrix}$ | Adds $45^{\circ}$ phase to $|1\rangle$. [cite_start]It's the "square root" of S[cite: 8574]. |
| **Y-Rotation** | **Ry** | $\begin{pmatrix} \cos(\theta/2) & -\sin(\theta/2) \\ \sin(\theta/2) & \cos(\theta/2) \end{pmatrix}$ | [cite_start]Rotates the state vector around the Y-axis[cite: 8595]. |
| **CNOT** | **CX** | $\begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{pmatrix}$ | [cite_start]Flips the second qubit (Target) if the first (Control) is 1[cite: 8607]. |

### 5. Adjoint and Hermitian Operators
* **Adjoint ($\dagger$)**: The "conjugate transpose". You swap rows/columns AND flip the sign of imaginary numbers ($i \to -i$).
    * [cite_start]$\hat{A}^\dagger_{ij} = A_{ji}^*$[cite: 8790].
* **Hermitian Operator**: A matrix that is equal to its own adjoint ($\hat{A} = \hat{A}^\dagger$).
    * [cite_start]*Significance:* These represent **measurable quantities** (observables) in physics (like energy)[cite: 8812].
* **Unitary Operator**: A matrix whose inverse is its adjoint ($\hat{U}^{-1} = \hat{U}^\dagger$).
    * [cite_start]*Significance:* All **quantum gates** must be unitary to preserve probability[cite: 8848].

---

## ⏳ Lecture 4: Dynamics of Qubit States
*Goal: Understand how qubits change over time and the math governing their rotation.*

### 1. Key Theorems for Exam
1.  **Conservation of Probability**: A unitary operator ($\hat{U}$) preserves the **norm** (length) of a vector. [cite_start]If you start with a valid quantum state (length 1), applying a gate keeps it a valid state (length 1)[cite: 4239].
2.  **Eigenvalues**:
    * [cite_start]**Hermitian** matrices have **Real** eigenvalues (measurable outcomes are real numbers)[cite: 4373].
    * [cite_start]**Unitary** matrices have eigenvalues with **modulus 1** (complex numbers on the unit circle, $|e^{i\phi}| = 1$)[cite: 4389].
3.  [cite_start]**Orthogonality**: Eigenvectors belonging to different eigenvalues are always orthogonal (perpendicular)[cite: 4409].

### 2. Spectral Decomposition
Any "nice" matrix (Hermitian or Unitary) can be broken down into a sum of its eigenvalues and projectors:
$$\hat{A} = \sum a_n |a_n\rangle\langle a_n|$$
* Where $a_n$ are the eigenvalues and $|a_n\rangle$ are the eigenvectors. [cite_start]This is like writing a recipe for the matrix[cite: 4443].

### 3. Functions of Matrices ($e^{\hat{A}}$)
We often see exponentials of matrices, like $e^{i\hat{H}t}$.
* [cite_start]**Definition**: You expand it as a Taylor series: $e^{\hat{A}} = \hat{I} + \hat{A} + \frac{\hat{A}^2}{2!} + \dots$[cite: 4471].
* **Crucial Identity**: If $\hat{A}^2 = \hat{I}$ (which is true for Pauli matrices $X, Y, Z$), then:
    $$e^{i\theta \hat{A}} = \cos(\theta)\hat{I} + i\sin(\theta)\hat{A}$$
    * [cite_start]*Exam Tip:* This formula is used constantly to convert exponentials into readable gate operations[cite: 4513].

### 4. Rotation Operators
Any single-qubit gate can be viewed as a rotation of the Bloch vector around some axis $\vec{n}$.
$$\hat{R}_{\vec{n}}(\theta) = e^{-i\frac{\theta}{2} \hat{\sigma}_{\vec{n}}} = \cos(\frac{\theta}{2})\hat{I} - i\sin(\frac{\theta}{2})\hat{\sigma}_{\vec{n}}$$
* [cite_start]$\hat{\sigma}_{\vec{n}}$ is a mix of Pauli matrices ($X, Y, Z$) representing the axis of rotation[cite: 4557].

**Decomposition Theorem (Z-Y-Z):**
Any single-qubit unitary gate $\hat{U}$ can be broken down into three rotations:
$$\hat{U} = e^{i\alpha} \hat{R}_z(\beta) \hat{R}_y(\gamma) \hat{R}_z(\delta)$$
* [cite_start]This means you can build *any* qubit operation just by rotating around Z, then Y, then Z again[cite: 4721].

### 5. Time Evolution (The Physics Part)
How does a qubit change if we just leave it alone or apply a magnetic field?
* **Schrödinger Equation**: Describes the change over time.
    $$i\hbar \frac{d}{dt}|\psi(t)\rangle = \hat{H}|\psi(t)\rangle$$
    * [cite_start]$\hat{H}$ is the **Hamiltonian** (the operator representing the Energy of the system)[cite: 4906].
* **The Solution**: If $\hat{H}$ is constant, the state at time $t$ is:
    $$|\psi(t)\rangle = e^{-\frac{i}{\hbar}\hat{H}t} |\psi(0)\rangle$$
* [cite_start]**Key Insight**: Quantum gates are actually just time evolution operators where we engineer the Hamiltonian $\hat{H}$ and the time $t$ to get the rotation we want[cite: 4987].


## 🔗 Lecture 5: Multi-Qubit States & Gates
*Goal: Understand how to describe systems with more than one qubit (tensor products) and the unique quantum phenomenon of entanglement.*

### 1. The Tensor Product (Gluing Spaces Together)
[cite_start]When we have two qubits, $A$ and $B$, their combined state lives in a larger Hilbert space formed by the **Tensor Product** ($H^A \otimes H^B$)[cite: 30].

* [cite_start]**Dimension**: If space $A$ has dimension $N$ and space $B$ has dimension $M$, the combined space has dimension $N \times M$[cite: 32]. For two qubits ($2 \times 2$), the space is 4-dimensional.
* **Basis**: The new basis is formed by all combinations of the original basis vectors:
    [cite_start]$$|00\rangle, |01\rangle, |10\rangle, |11\rangle$$ [cite: 130-133].
    *(Note: $|00\rangle$ is shorthand for $|0\rangle \otimes |0\rangle$)*.

**How to Calculate it (Kronecker Product):**
[cite_start]To combine two vectors, you multiply every element of the first vector by the *entire* second vector[cite: 122].
$$\begin{pmatrix} a \\ b \end{pmatrix} \otimes \begin{pmatrix} c \\ d \end{pmatrix} = \begin{pmatrix} a \cdot \begin{pmatrix} c \\ d \end{pmatrix} \\ b \cdot \begin{pmatrix} c \\ d \end{pmatrix} \end{pmatrix} = \begin{pmatrix} ac \\ ad \\ bc \\ bd \end{pmatrix}$$

### 2. Entanglement vs. Separability
Not all multi-qubit states are created equal.
* [cite_start]**Separable States**: A state is separable if it can be written as a clean product of individual qubit states: $|\Psi\rangle = |\psi^A\rangle \otimes |\psi^B\rangle$[cite: 231].
    * *Example:* $|00\rangle$ is separable because it is just qubit A in $|0\rangle$ and qubit B in $|0\rangle$.
* [cite_start]**Entangled States**: A state is entangled if it **cannot** be broken down into individual qubit states[cite: 238].
    * [cite_start]*Example (Bell State):* $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$[cite: 233]. You cannot say qubit A is definitely "0" or "1" without knowing qubit B. They are linked.

**Measuring Entanglement (Concurrence):**
For a state $|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle$, we calculate **Concurrence ($C$)**:
[cite_start]$$C = 2|c_{00}c_{11} - c_{01}c_{10}|$$[cite: 255].
* If $C=0$, the state is Separable.
* [cite_start]If $C=1$, the state is Maximally Entangled (like a Bell state)[cite: 259, 262].

### 3. The Non-Cloning Theorem
[cite_start]**Crucial Concept:** You cannot copy an unknown quantum state[cite: 341].
* [cite_start]There is no unitary operation (quantum gate) $\hat{U}$ that can take an arbitrary state $|\psi\rangle$ and a blank state $|0\rangle$ and output two copies $|\psi\rangle \otimes |\psi\rangle$[cite: 341].
* [cite_start]*Proof intuition:* Cloning would violate the linearity of quantum mechanics [cite: 369-389].

### 4. Controlled Gates
These are gates where one qubit (Control) decides whether an operation happens on another qubit (Target).
* **Controlled-NOT (CNOT)**: Flips the target bit only if the control bit is $|1\rangle$.
    * Formula: $cX = |0\rangle\langle 0| \otimes \hat{I} + |1\rangle\langle 1| [cite_start]\otimes \hat{X}$[cite: 951].
* [cite_start]**Decomposition**: Any controlled unitary gate can be built from single-qubit gates and CNOTs[cite: 1101]. This is important because real hardware usually only supports a few basic gates.

---

## 🔎 Lecture 6: Grover's Algorithm
*Goal: Understand how to search a database faster than any classical computer.*

### 1. The Problem
You have an unsorted database with $N$ items. You want to find a specific item $a$ (where $f(a)=1$).
* **Classical Search**: You have to check items one by one. On average, you check $N/2$ items. [cite_start]Complexity: $O(N)$[cite: 2911].
* **Grover's Algorithm**: Finds the item in roughly $\sqrt{N}$ steps. [cite_start]Complexity: $O(\sqrt{N})$[cite: 2921]. This is a **quadratic speedup**.

### 2. The Setup
* **The Oracle ($U_f$)**: A "black box" function. If the input is the target solution $|a\rangle$, it flips its phase (multiplies by -1). [cite_start]If it's not the target, it does nothing[cite: 2932].
    * [cite_start]Mathematically: $U_f|x\rangle = (-1)^{f(x)}|x\rangle$[cite: 2776].
* **The Initial State ($|\phi\rangle$)**: We start in a "uniform superposition" of all possible answers. [cite_start]This means we have an equal probability amplitude for every item in the database[cite: 2951, 2955].

### 3. Geometric Interpretation (The Rotation)
Imagine a 2D plane defined by two vectors:
1.  $|a\rangle$: The solution we want.
2.  [cite_start]$|a_{\perp}\rangle$: Everything that is *not* the solution[cite: 2935, 2943].

[cite_start]Grover's algorithm works by rotating our state vector $|\phi\rangle$ towards $|a\rangle$[cite: 2952].
* [cite_start]**Step 1 (Oracle $\hat{V}$)**: Reflects the vector around $|a_{\perp}\rangle$[cite: 2969].
* **Step 2 (Diffusion $\hat{W}$)**: Reflects the vector around the initial average state $|\phi\rangle$. [cite_start]This is often called "inversion about the mean"[cite: 3314].

**The Result:**
[cite_start]Each pair of these operations (Oracle + Diffusion) rotates the state vector by an angle $2\theta$ closer to the solution $|a\rangle$[cite: 2994]. [cite_start]After about $\frac{\pi}{4}\sqrt{N}$ rotations, the state points almost exactly at $|a\rangle$[cite: 3121].

---

## 🌊 Lecture 7: Quantum Fourier Transform (QFT)
*Goal: Learn the quantum version of the Discrete Fourier Transform, a tool essential for many advanced algorithms (like breaking encryption).*

### 1. What is QFT?
[cite_start]Just like the classical Fourier Transform changes data from the "time domain" to the "frequency domain," the QFT changes a quantum state into a new basis[cite: 7666].
* [cite_start]It transforms a basis state $|x\rangle$ into a superposition of all states, weighted by complex phases (roots of unity)[cite: 7362].
    [cite_start]$$QFT|x\rangle = \frac{1}{\sqrt{N}} \sum_{y=0}^{N-1} e^{\frac{2\pi i xy}{N}} |y\rangle$$[cite: 7362].

### 2. The Circuit Implementation
Implementing QFT as a matrix is hard ($N \times N$ matrix is huge). But as a circuit, it's efficient.
* **Key Components**:
    1.  **Hadamard Gates (H)**: Create superposition.
    2.  [cite_start]**Controlled Phase Rotations ($R_k$ or $CP$)**: These add specific phase shifts based on the relationship between qubits[cite: 7988].
        * [cite_start]The rotation angles get smaller as you go deeper: $\pi/2, \pi/4, \pi/8 \dots$[cite: 7636].
    3.  **SWAP Gates**: The QFT circuit naturally outputs bits in reverse order. [cite_start]We use SWAP gates at the end to flip them back to the correct order[cite: 7529].

### 3. Why is it useful?
* **Period Finding**: QFT is incredibly good at finding repeating patterns (periods) in data.
* **Phase Estimation**: It allows us to estimate the eigenvalues (phases) of a unitary operator, which is the core step in algorithms for quantum chemistry and factoring numbers.

---

### 📝 Exam Cheat Sheet (Summary)

| Concept | Key Equation / Definition | Why it matters |
| :--- | :--- | :--- |
| **Tensor Product** | $\begin{pmatrix} a \\ b \end{pmatrix} \otimes \begin{pmatrix} c \\ d \end{pmatrix} = \begin{pmatrix} ac \\ ad \\ bc \\ bd \end{pmatrix}$ | Describes multi-qubit systems. |
| **Entanglement** | Cannot factorize: $|\Psi\rangle \neq |\phi\rangle \otimes |\psi\rangle$ | Allows quantum correlations stronger than classical ones. |
| **Non-Cloning** | No $\hat{U}$ exists s.t. $\hat{U}|\psi\rangle|0\rangle = |\psi\rangle|\psi\rangle$ | You can't copy quantum data; you must teleport or move it. |
| **Grover's Algo** | Iterations $r \approx \frac{\pi}{4}\sqrt{N}$ | Quadratic speedup for searching unstructured data. |
| **Oracle ($U_f$)** | $U_f|x\rangle = (-1)^{f(x)}|x\rangle$ | Marks the "correct" answer by flipping its phase. |
| **Diffusion ($W$)** | $2|\phi\rangle\langle\phi| - \hat{I}$ | Amplifies the probability of the marked item (inversion about mean). |
| **QFT** | Basis change using roots of unity | Exponentially faster than classical FFT; used for period finding. |


## ⏱️ Lecture 8: Quantum Phase Estimation (QPE)
*Goal: Estimate the "phase" (eigenvalue) of a unitary operator. This is the engine behind many famous algorithms, including Shor's algorithm.*

### 1. The Core Problem
We have a unitary operator $\hat{U}$ and its eigenvector $|u\rangle$.
$$\hat{U}|u\rangle = e^{2\pi i \theta} |u\rangle$$
We want to find the value of $\theta$ (the phase).
* [cite_start]Since $\hat{U}$ is unitary, its eigenvalues always lie on the unit circle in the complex plane ($|\lambda|=1$)[cite: 5020].
* The phase $\theta$ is a number between 0 and 1.

### 2. The Setup
We need two registers:
1.  **Clock Register (Auxiliary)**: $m$ qubits initialized to $|0\rangle$. These will store the estimated phase.
2.  **Target Register**: Stores the eigenvector $|u\rangle$.

**The Circuit Steps:**
1.  [cite_start]**Hadamard**: Apply $H$ gates to all qubits in the Clock Register to create a uniform superposition [cite: 5127-5130].
2.  **Controlled Unitaries**: Apply a sequence of controlled-$\hat{U}$ operations.
    * The $k$-th qubit in the clock register controls the operation $\hat{U}^{2^k}$.
    * [cite_start]This uses "Phase Kickback" to copy the phase information from the target register into the clock register's relative phases [cite: 5164-5166].
3.  **Inverse QFT**: Apply the Inverse Quantum Fourier Transform ($QFT^\dagger$) to the Clock Register. [cite_start]This decodes the phase information from the "frequency domain" back into a binary number state[cite: 5333].
4.  **Measurement**: Measure the Clock Register to get an integer $y$. [cite_start]The estimated phase is $\theta \approx y / 2^m$[cite: 5024].

### 3. Precision vs. Qubits
The more qubits ($m$) you use in the clock register, the more precise your estimate of $\theta$.
* With $m$ qubits, you can distinguish $2^m$ different phases.
* [cite_start]If $\theta$ cannot be written exactly as a fraction $y/2^m$, you will measure the closest approximation with high probability[cite: 5420, 5433].


## 🧮 Lecture 9 & 10: HHL & VQE Algorithms
*Goal: Use quantum computers for practical math problems: Solving linear equations (HHL) and finding ground state energies (VQE).*

### 1. The HHL Algorithm (Solving Linear Systems)
**Problem:** Solve $\hat{A}\vec{x} = \vec{b}$ for $\vec{x}$.
* Classically, this is hard for large matrices. HHL can do it exponentially faster under specific conditions.

**How it works (High Level):**
1.  [cite_start]**State Preparation**: Encode the vector $\vec{b}$ into a quantum state $|b\rangle$[cite: 6814].
2.  **Phase Estimation (QPE)**: Use $\hat{U} = e^{i\hat{A}t}$ to estimate the eigenvalues ($\lambda_i$) of matrix $\hat{A}$. [cite_start]These eigenvalues get stored in a clock register[cite: 6820].
3.  **Rotation (The "Math" Step)**: Add an extra "ancilla" qubit. Rotate this qubit by an amount proportional to $1/\lambda_i$. [cite_start]This physically implements the division by the eigenvalues[cite: 6830].
4.  [cite_start]**Uncomputation**: Run Inverse QPE to clean up the clock register (disentangle it)[cite: 6834].
5.  **Measurement**: If you measure "1" on the ancilla qubit, the remaining register has the state proportional to $|x\rangle = \hat{A}^{-1}|b\rangle$.

**Key Limitation**: You don't get the vector $\vec{x}$ as a list of numbers. You get a quantum state $|x\rangle$. To get useful info, you must measure specific properties (like expectation values) of $|x\rangle$.

---

### 2. The VQE Algorithm (Variational Quantum Eigensolver)
**Problem:** Find the lowest energy (ground state) of a molecule or system described by Hamiltonian $\hat{H}$.
* [cite_start]This is a **Hybrid Algorithm**: It uses a quantum computer *and* a classical computer working together in a loop [cite: 1982-2004].

**How it works (The Loop):**
1.  [cite_start]**Ansatz (Quantum)**: The quantum computer prepares a "guess" state $|\psi(\theta)\rangle$ using a circuit with tunable parameters $\theta$ (like rotation angles)[cite: 2382].
2.  **Measurement (Quantum)**: The quantum computer measures the expectation value (average energy) $\langle \hat{H} \rangle$ for that state.
    * [cite_start]Since we can't measure energy directly easily, we break $\hat{H}$ into simple Pauli terms (like $X, Y, Z$) and measure those separately: $\hat{H} = a\hat{X} + b\hat{Z} + \dots$ [cite: 1972-1974].
3.  **Optimization (Classical)**: A classical computer looks at the energy result. [cite_start]If it's not the minimum, it tells the quantum computer to adjust the parameters $\theta$ to try and lower the energy[cite: 2695].
4.  **Repeat**: This loop continues until the energy stops going down (convergence).

**Why it's important:** VQE is robust against noise, making it the most promising algorithm for the current era of imperfect quantum computers (NISQ era).

---

### 📝 Exam Cheat Sheet (Algorithms)

| Algorithm | Purpose | Key Quantum Mechanism |
| :--- | :--- | :--- |
| **QPE** | Estimate phase $\theta$ of eigenvalue | $H^{\otimes m} \to$ Controlled-$U$ $\to IQFT$ |
| **HHL** | Solve linear equations $\hat{A}\vec{x}=\vec{b}$ | QPE on $\hat{A}$ $\to$ Rotation by $1/\lambda$ $\to$ Inverse QPE |
| **VQE** | Find minimum eigenvalue (Energy) | Hybrid Loop: Quantum measures $\langle H \rangle$, Classical optimizes parameters. |

**Important Python/Qiskit Note:**
* **QPE**: Requires implementing `QFT` and its inverse `IQFT_dg`.
* **VQE**: In Qiskit, you define a `QuantumCircuit` for the Ansatz (the guess) and use a classical optimizer (like `COBYLA` or `SPSA`) to update the parameters.