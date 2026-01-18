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
| **Ket** | A column vector representing state | $\lvert\psi\rangle = \begin{pmatrix} c_0 \\\\ c_1 \end{pmatrix}$ |
| **Bra** | A row vector (conjugate transpose) | $\langle\psi\rvert = (c_0^\*, c_1^\*)$ |
| **Bra-Ket** | Inner product (measures overlap) | $\langle\phi\lvert\psi\rangle$ (Scalar number) |
| **Normalization** | Requirement for total prob. to be 1 | $\sqrt{\langle\psi\lvert\psi\rangle} = 1$ |
| **Basis** $\lvert 0\rangle$ | "Ground" state (North Pole) | $\begin{pmatrix} 1 \\\\ 0 \end{pmatrix}$ |
| **Basis** $\lvert 1\rangle$ | "Excited" state (South Pole) | $\begin{pmatrix} 0 \\\\ 1 \end{pmatrix}$ |
| **Bloch Vector** | 3D representation of the state | Vector $\vec{n}$ inside the sphere |

**Important Python/Qiskit Note:**
In Qiskit, the command `plot_bloch_vector` creates the sphere visualization. The standard simulator `qasm_simulator` mimics a real quantum computer by running the circuit many times ("shots") to estimate the probabilities (e.g., if you run 100 shots and get '0' 50 times, probability is 0.5).


## 🛠️ Lecture 3: Operators & Matrix Representations
*Goal: Learn the "verbs" of quantum computing. If a qubit is a vector, an operator is a machine that transforms that vector into a new one.*

### 1. What is a Linear Operator?
In simple terms, a linear operator is a function $\hat{A}$ that takes a vector and spits out a new vector.
* **Linearity**: It must follow two rules:
    1.  **Additivity**: $\hat{A}(|\psi\rangle + |\varphi\rangle) = \hat{A}|\psi\rangle + \hat{A}|\varphi\rangle$.
    2.  **Homogeneity**: $\hat{A}(c|\psi\rangle) = c\hat{A}|\psi\rangle$ (where $c$ is a number).

### 2. The "Outer Product" (Ket-Bra)
You know the inner product (Bra-Ket $\langle a | b \rangle$) produces a number.
The **Outer Product** (Ket-Bra $|b\rangle\langle a|$) produces an **operator (matrix)**.

* **Definition**: An operator $\hat{P}_{ba} = |b\rangle\langle a|$ acts on a vector $|\psi\rangle$ like this:
    $$(|b\rangle\langle a|) |\psi\rangle = |b\rangle (\langle a | \psi \rangle)$$
    * *Translation:* It calculates the overlap (dot product) between $a$ and $\psi$, and scales the vector $|b\rangle$ by that amount.

* **Projection Operators ($\hat{P}$)**:
    If you make an operator out of the *same* basis vector, like $\hat{P}_0 = |0\rangle\langle 0|$, it acts as a filter.
    * $\hat{P}_0$ keeps the part of the vector pointing along $|0\rangle$ and deletes the rest.
    * **Identity Operator ($\hat{\mathbb{I}}$)**: The "do nothing" operator. It is the sum of all projectors: $\hat{\mathbb{I}} = |0\rangle\langle 0| + |1\rangle\langle 1|$.

### 3. Matrix Representation
Since vectors are columns of numbers, operators are **matrices** (grids of numbers).
* If you have a basis $\{|0\rangle, |1\rangle\}$, the element in row $i$ and column $j$ of operator $\hat{A}$ is:
    $$A_{ij} = \langle i | \hat{A} | j \rangle$$
* *Student Tip:* To find the matrix of a gate, look at what it does to the basis vectors $|0\rangle$ and $|1\rangle$. The result of acting on $|0\rangle$ becomes the **first column**. The result of acting on $|1\rangle$ becomes the **second column**.

### 4. Important Quantum Gates (Cheat Sheet)
You must recognize these matrices.

| Gate | Symbol | Matrix | Effect |
| :--- | :--- | :--- | :--- |
| **Hadamard** | **H** | $\frac{1}{\sqrt{2}}\begin{pmatrix} 1 & 1 \\\\ 1 & -1 \end{pmatrix}$ | Creates superposition ($\lvert 0\rangle \to \lvert +\rangle$, $\lvert 1\rangle \to \lvert -\rangle$). |
| **Phase (S)** | **S** | $\begin{pmatrix} 1 & 0 \\\\ 0 & i \end{pmatrix}$ | Adds $90^{\circ}$ ($i$) phase to $\lvert 1\rangle$. |
| **T Gate** | **T** | $\begin{pmatrix} 1 & 0 \\\\ 0 & e^{i\pi/4} \end{pmatrix}$ | Adds $45^{\circ}$ phase to $\lvert 1\rangle$. It's the "square root" of S. |
| **Y-Rotation** | **Ry** | $\begin{pmatrix} \cos(\theta/2) & -\sin(\theta/2) \\\\ \sin(\theta/2) & \cos(\theta/2) \end{pmatrix}$ | Rotates the state vector around the Y-axis. |
| **CNOT** | **CX** | $\begin{pmatrix} 1 & 0 & 0 & 0 \\\\ 0 & 1 & 0 & 0 \\\\ 0 & 0 & 0 & 1 \\\\ 0 & 0 & 1 & 0 \end{pmatrix}$ | Flips the second qubit (Target) if the first (Control) is 1. |

### 5. Adjoint and Hermitian Operators
* **Adjoint ($\dagger$)**: The "conjugate transpose". You swap rows/columns AND flip the sign of imaginary numbers ($i \to -i$).
    * $\hat{A}^\dagger_{ij} = A_{ji}^*$.
* **Hermitian Operator**: A matrix that is equal to its own adjoint ($\hat{A} = \hat{A}^\dagger$).
    * *Significance:* These represent **measurable quantities** (observables) in physics (like energy).
* **Unitary Operator**: A matrix whose inverse is its adjoint ($\hat{U}^{-1} = \hat{U}^\dagger$).
    * *Significance:* All **quantum gates** must be unitary to preserve probability.

---

## ⏳ Lecture 4: Dynamics of Qubit States
*Goal: Understand how qubits change over time and the math governing their rotation.*

### 1. Key Theorems for Exam
1.  **Conservation of Probability**: A unitary operator ($\hat{U}$) preserves the **norm** (length) of a vector. If you start with a valid quantum state (length 1), applying a gate keeps it a valid state (length 1).
2.  **Eigenvalues**:
    * **Hermitian** matrices have **Real** eigenvalues (measurable outcomes are real numbers).
    * **Unitary** matrices have eigenvalues with **modulus 1** (complex numbers on the unit circle, $|e^{i\phi}| = 1$).
3.  **Orthogonality**: Eigenvectors belonging to different eigenvalues are always orthogonal (perpendicular).

### 2. Spectral Decomposition
Any "nice" matrix (Hermitian or Unitary) can be broken down into a sum of its eigenvalues and projectors:
$$\hat{A} = \sum a_n |a_n\rangle\langle a_n|$$
* Where $a_n$ are the eigenvalues and $|a_n\rangle$ are the eigenvectors. This is like writing a recipe for the matrix.

### 3. Functions of Matrices ($e^{\hat{A}}$)
We often see exponentials of matrices, like $e^{i\hat{H}t}$.
* **Definition**: You expand it as a Taylor series: $e^{\hat{A}} = \hat{I} + \hat{A} + \frac{\hat{A}^2}{2!} + \dots$.
* **Crucial Identity**: If $\hat{A}^2 = \hat{I}$ (which is true for Pauli matrices $X, Y, Z$), then:
    $$e^{i\theta \hat{A}} = \cos(\theta)\hat{I} + i\sin(\theta)\hat{A}$$
    * *Exam Tip:* This formula is used constantly to convert exponentials into readable gate operations.

### 4. Rotation Operators
Any single-qubit gate can be viewed as a rotation of the Bloch vector around some axis $\vec{n}$.

$$\hat{R}_n(\theta) = e^{-i\frac{\theta}{2} \hat{\sigma}_n} = \cos\left(\frac{\theta}{2}\right)\hat{I} - i\sin\left(\frac{\theta}{2}\right)\hat{\sigma}_n$$

* $\hat{\sigma}_n$ is a mix of Pauli matrices ($X, Y, Z$) representing the axis of rotation.

**Decomposition Theorem (Z-Y-Z):**
Any single-qubit unitary gate $\hat{U}$ can be broken down into three rotations:

$$\hat{U} = e^{i\alpha} \hat{R}_z(\beta) \hat{R}_y(\gamma) \hat{R}_z(\delta)$$

* This means you can build *any* qubit operation just by rotating around Z, then Y, then Z again.

### 5. Time Evolution (The Physics Part)
How does a qubit change if we just leave it alone or apply a magnetic field?
* **Schrödinger Equation**: Describes the change over time.
    $$i\hbar \frac{d}{dt}|\psi(t)\rangle = \hat{H}|\psi(t)\rangle$$
    * $\hat{H}$ is the **Hamiltonian** (the operator representing the Energy of the system).
* **The Solution**: If $\hat{H}$ is constant, the state at time $t$ is:
    $$|\psi(t)\rangle = e^{-\frac{i}{\hbar}\hat{H}t} |\psi(0)\rangle$$
* **Key Insight**: Quantum gates are actually just time evolution operators where we engineer the Hamiltonian $\hat{H}$ and the time $t$ to get the rotation we want.


## 🔗 Lecture 5: Multi-Qubit States & Gates
*Goal: Understand how to describe systems with more than one qubit (tensor products) and the unique quantum phenomenon of entanglement.*

### 1. The Tensor Product (Gluing Spaces Together)
When we have two qubits, $A$ and $B$, their combined state lives in a larger Hilbert space formed by the **Tensor Product** ($H^A \otimes H^B$).

* **Dimension**: If space $A$ has dimension $N$ and space $B$ has dimension $M$, the combined space has dimension $N \times M$. For two qubits ($2 \times 2$), the space is 4-dimensional.
* **Basis**: The new basis is formed by all combinations of the original basis vectors:
    $$|00\rangle, |01\rangle, |10\rangle, |11\rangle$$
    *(Note: $|00\rangle$ is shorthand for $|0\rangle \otimes |0\rangle$)*.

**How to Calculate it (Kronecker Product):**
To combine two vectors, you multiply every element of the first vector by the *entire* second vector.
$$\begin{pmatrix} a \\ b \end{pmatrix} \otimes \begin{pmatrix} c \\ d \end{pmatrix} = \begin{pmatrix} a \cdot \begin{pmatrix} c \\ d \end{pmatrix} \\ b \cdot \begin{pmatrix} c \\ d \end{pmatrix} \end{pmatrix} = \begin{pmatrix} ac \\ ad \\ bc \\ bd \end{pmatrix}$$

### 2. Entanglement vs. Separability
Not all multi-qubit states are created equal.
* **Separable States**: A state is separable if it can be written as a clean product of individual qubit states: $|\Psi\rangle = |\psi^A\rangle \otimes |\psi^B\rangle$.
    * *Example:* $|00\rangle$ is separable because it is just qubit A in $|0\rangle$ and qubit B in $|0\rangle$.
* **Entangled States**: A state is entangled if it **cannot** be broken down into individual qubit states.
    * *Example (Bell State):* $|\Psi\rangle = \frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$. You cannot say qubit A is definitely "0" or "1" without knowing qubit B. They are linked.

**Measuring Entanglement (Concurrence):**
For a state $|\Psi\rangle = c_{00}|00\rangle + c_{01}|01\rangle + c_{10}|10\rangle + c_{11}|11\rangle$, we calculate **Concurrence ($C$)**:
$$C = 2|c_{00}c_{11} - c_{01}c_{10}|$$
* If $C=0$, the state is Separable.
* If $C=1$, the state is Maximally Entangled (like a Bell state).

### 3. The Non-Cloning Theorem
**Crucial Concept:** You cannot copy an unknown quantum state.
* There is no unitary operation (quantum gate) $\hat{U}$ that can take an arbitrary state $|\psi\rangle$ and a blank state $|0\rangle$ and output two copies $|\psi\rangle \otimes |\psi\rangle$.
* *Proof intuition:* Cloning would violate the linearity of quantum mechanics.

### 4. Controlled Gates
These are gates where one qubit (Control) decides whether an operation happens on another qubit (Target).
* **Controlled-NOT (CNOT)**: Flips the target bit only if the control bit is $|1\rangle$.
    * Formula: $cX = |0\rangle\langle 0| \otimes \hat{I} + |1\rangle\langle 1| \otimes \hat{X}$.
* **Decomposition**: Any controlled unitary gate can be built from single-qubit gates and CNOTs. This is important because real hardware usually only supports a few basic gates.

---

## 🔎 Lecture 6: Grover's Algorithm
*Goal: Understand how to search a database faster than any classical computer.*

### 1. The Problem
You have an unsorted database with $N$ items. You want to find a specific item $a$ (where $f(a)=1$).
* **Classical Search**: You have to check items one by one. On average, you check $N/2$ items. Complexity: $O(N)$.
* **Grover's Algorithm**: Finds the item in roughly $\sqrt{N}$ steps. Complexity: $O(\sqrt{N})$. This is a **quadratic speedup**.

### 2. The Setup
* **The Oracle ($U_f$)**: A "black box" function. If the input is the target solution $|a\rangle$, it flips its phase (multiplies by -1). If it's not the target, it does nothing.
    * Mathematically: $U_f|x\rangle = (-1)^{f(x)}|x\rangle$.
* **The Initial State ($|\phi\rangle$)**: We start in a "uniform superposition" of all possible answers. This means we have an equal probability amplitude for every item in the database.

### 3. Geometric Interpretation (The Rotation)
Imagine a 2D plane defined by two vectors:
1.  $|a\rangle$: The solution we want.
2.  $|a_{\perp}\rangle$: Everything that is *not* the solution.

Grover's algorithm works by rotating our state vector $|\phi\rangle$ towards $|a\rangle$.
* **Step 1 (Oracle $\hat{V}$)**: Reflects the vector around $|a_{\perp}\rangle$.
* **Step 2 (Diffusion $\hat{W}$)**: Reflects the vector around the initial average state $|\phi\rangle$. This is often called "inversion about the mean".

**The Result:**
Each pair of these operations (Oracle + Diffusion) rotates the state vector by an angle $2\theta$ closer to the solution $|a\rangle$. After about $\frac{\pi}{4}\sqrt{N}$ rotations, the state points almost exactly at $|a\rangle$.

---

## 🌊 Lecture 7: Quantum Fourier Transform (QFT)
*Goal: Learn the quantum version of the Discrete Fourier Transform, a tool essential for many advanced algorithms (like breaking encryption).*

### 1. What is QFT?
Just like the classical Fourier Transform changes data from the "time domain" to the "frequency domain," the QFT changes a quantum state into a new basis.
* It transforms a basis state $|x\rangle$ into a superposition of all states, weighted by complex phases (roots of unity).
    $$QFT|x\rangle = \frac{1}{\sqrt{N}} \sum_{y=0}^{N-1} e^{\frac{2\pi i xy}{N}} |y\rangle$$

### 2. The Circuit Implementation
Implementing QFT as a matrix is hard ($N \times N$ matrix is huge). But as a circuit, it's efficient.
* **Key Components**:
    1.  **Hadamard Gates (H)**: Create superposition.
    2.  **Controlled Phase Rotations ($R_k$ or $CP$)**: These add specific phase shifts based on the relationship between qubits.
        * The rotation angles get smaller as you go deeper: $\pi/2, \pi/4, \pi/8 \dots$
    3.  **SWAP Gates**: The QFT circuit naturally outputs bits in reverse order. We use SWAP gates at the end to flip them back to the correct order.

### 3. Why is it useful?
* **Period Finding**: QFT is incredibly good at finding repeating patterns (periods) in data.
* **Phase Estimation**: It allows us to estimate the eigenvalues (phases) of a unitary operator, which is the core step in algorithms for quantum chemistry and factoring numbers.

---

### 📝 Exam Cheat Sheet (Summary)

| Concept | Key Equation / Definition | Why it matters |
| :--- | :--- | :--- |
| **Tensor Product** | $\begin{pmatrix} a \\\\ b \end{pmatrix} \otimes \begin{pmatrix} c \\\\ d \end{pmatrix} = \begin{pmatrix} ac \\\\ ad \\\\ bc \\\\ bd \end{pmatrix}$ | Describes multi-qubit systems. |
| **Entanglement** | Cannot factorize: $\lvert\Psi\rangle \neq \lvert\phi\rangle \otimes \lvert\psi\rangle$ | Allows quantum correlations stronger than classical ones. |
| **Non-Cloning** | No $\hat{U}$ exists s.t. $\hat{U}\lvert\psi\rangle\lvert 0\rangle = \lvert\psi\rangle\lvert\psi\rangle$ | You can't copy quantum data; you must teleport or move it. |
| **Grover's Algo** | Iterations $r \approx \frac{\pi}{4}\sqrt{N}$ | Quadratic speedup for searching unstructured data. |
| **Oracle ($U_f$)** | $U_f\lvert x\rangle = (-1)^{f(x)}\lvert x\rangle$ | Marks the "correct" answer by flipping its phase. |
| **Diffusion ($W$)** | $2\lvert\phi\rangle\langle\phi\rvert - \hat{I}$ | Amplifies the probability of the marked item (inversion about mean). |
| **QFT** | Basis change using roots of unity | Exponentially faster than classical FFT; used for period finding. |


## ⏱️ Lecture 8: Quantum Phase Estimation (QPE)
*Goal: Estimate the "phase" (eigenvalue) of a unitary operator. This is the engine behind many famous algorithms, including Shor's algorithm.*

### 1. The Core Problem
We have a unitary operator $\hat{U}$ and its eigenvector $|u\rangle$.
$$\hat{U}|u\rangle = e^{2\pi i \theta} |u\rangle$$
We want to find the value of $\theta$ (the phase).
* Since $\hat{U}$ is unitary, its eigenvalues always lie on the unit circle in the complex plane ($|\lambda|=1$).
* The phase $\theta$ is a number between 0 and 1.

### 2. The Setup
We need two registers:
1.  **Clock Register (Auxiliary)**: $m$ qubits initialized to $|0\rangle$. These will store the estimated phase.
2.  **Target Register**: Stores the eigenvector $|u\rangle$.

**The Circuit Steps:**
1.  **Hadamard**: Apply $H$ gates to all qubits in the Clock Register to create a uniform superposition.
2.  **Controlled Unitaries**: Apply a sequence of controlled-$\hat{U}$ operations.
    * The $k$-th qubit in the clock register controls the operation $\hat{U}^{2^k}$.
    * This uses "Phase Kickback" to copy the phase information from the target register into the clock register's relative phases.
3.  **Inverse QFT**: Apply the Inverse Quantum Fourier Transform ($QFT^\dagger$) to the Clock Register. This decodes the phase information from the "frequency domain" back into a binary number state.
4.  **Measurement**: Measure the Clock Register to get an integer $y$. The estimated phase is $\theta \approx y / 2^m$.

### 3. Precision vs. Qubits
The more qubits ($m$) you use in the clock register, the more precise your estimate of $\theta$.
* With $m$ qubits, you can distinguish $2^m$ different phases.
* If $\theta$ cannot be written exactly as a fraction $y/2^m$, you will measure the closest approximation with high probability.


## 🧮 Lecture 9 & 10: HHL & VQE Algorithms
*Goal: Use quantum computers for practical math problems: Solving linear equations (HHL) and finding ground state energies (VQE).*

### 1. The HHL Algorithm (Solving Linear Systems)
**Problem:** Solve $\hat{A}\vec{x} = \vec{b}$ for $\vec{x}$.
* Classically, this is hard for large matrices. HHL can do it exponentially faster under specific conditions.

**How it works (High Level):**
1.  **State Preparation**: Encode the vector $\vec{b}$ into a quantum state $|b\rangle$.
2.  **Phase Estimation (QPE)**: Use $\hat{U} = e^{i\hat{A}t}$ to estimate the eigenvalues ($\lambda_i$) of matrix $\hat{A}$. These eigenvalues get stored in a clock register.
3.  **Rotation (The "Math" Step)**: Add an extra "ancilla" qubit. Rotate this qubit by an amount proportional to $1/\lambda_i$. This physically implements the division by the eigenvalues.
4.  **Uncomputation**: Run Inverse QPE to clean up the clock register (disentangle it).
5.  **Measurement**: If you measure "1" on the ancilla qubit, the remaining register has the state proportional to $|x\rangle = \hat{A}^{-1}|b\rangle$.

**Key Limitation**: You don't get the vector $\vec{x}$ as a list of numbers. You get a quantum state $|x\rangle$. To get useful info, you must measure specific properties (like expectation values) of $|x\rangle$.

---

### 2. The VQE Algorithm (Variational Quantum Eigensolver)
**Problem:** Find the lowest energy (ground state) of a molecule or system described by Hamiltonian $\hat{H}$.
* This is a **Hybrid Algorithm**: It uses a quantum computer *and* a classical computer working together in a loop.

**How it works (The Loop):**
1.  **Ansatz (Quantum)**: The quantum computer prepares a "guess" state $|\psi(\theta)\rangle$ using a circuit with tunable parameters $\theta$ (like rotation angles).
2.  **Measurement (Quantum)**: The quantum computer measures the expectation value (average energy) $\langle \hat{H} \rangle$ for that state.
    * Since we can't measure energy directly easily, we break $\hat{H}$ into simple Pauli terms (like $X, Y, Z$) and measure those separately: $\hat{H} = a\hat{X} + b\hat{Z} + \dots$
3.  **Optimization (Classical)**: A classical computer looks at the energy result. If it's not the minimum, it tells the quantum computer to adjust the parameters $\theta$ to try and lower the energy.
4.  **Repeat**: This loop continues until the energy stops going down (convergence).

**Why it's important:** VQE is robust against noise, making it the most promising algorithm for the current era of imperfect quantum computers (NISQ era).

---

### 📝 Exam Cheat Sheet (Algorithms)

| Algorithm | Purpose | Key Quantum Mechanism |
| :--- | :--- | :--- |
| **QPE** | Estimate phase $\theta$ of eigenvalue | $H^{\otimes m} \to$ Controlled-$U$ $\to$ IQFT |
| **HHL** | Solve linear equations $\hat{A}\vec{x}=\vec{b}$ | QPE on $\hat{A}$ $\to$ Rotation by $1/\lambda$ $\to$ Inverse QPE |
| **VQE** | Find minimum eigenvalue (Energy) | Hybrid Loop: Quantum measures $\langle H \rangle$, Classical optimizes parameters. |

**Important Python/Qiskit Note:**
* **QPE**: Requires implementing `QFT` and its inverse `IQFT_dg`.
* **VQE**: In Qiskit, you define a `QuantumCircuit` for the Ansatz (the guess) and use a classical optimizer (like `COBYLA` or `SPSA`) to update the parameters.

# Solving Multiple Choice Questions

---

## 🖥️ 1. Quantum Hardware & Qiskit Syntax

### **Q: Choosing a Quantum Computer**
**The Dilemma:** You have options with different "Number of qubits" and "Quantum Volume (QV)".
* **The Rule:** Qubit count isn't everything. A machine with 100 qubits is useless if they are too noisy to finish a calculation. **Quantum Volume (QV)** is the "holistic" metric that measures the *actual* useful power of the computer, accounting for gate errors, connectivity, and coherence time.
* **Strategy:** Always choose the machine with the **highest Quantum Volume** first. If QVs are equal, then choose the higher qubit count.

### **Q: Interpreting Qiskit Results**
**The Format:** `{'0': 1, '1': 0}` or `{'00': 500, '11': 500}`.
* **The Logic:** This is a Python dictionary: `{ 'Outcome': Count }`.
    * **Keys ('0', '1'):** These are the measurement results (bitstrings).
    * **Values (1, 0, 500):** These are the *number of times* that specific result occurred (the "shots").
* **Strategy:**
    * If the total count is 1 (e.g., `{'0': 1}`), the circuit was run **once** (1 shot).
    * If you see two keys (e.g., `'0': 500` and `'1': 500`), the circuit was run **multiple times**.
    * The length of the key string (e.g., `'00'` vs `'0'`) tells you how many qubits were measured.

### **Q: Qiskit Visualization (`plot_bloch_vector`)**
**The Syntax:** `plot_bloch_vector([a, b, c], coord_type='...')`
* **The Logic:** The function plots a state on the Bloch Sphere. The list `[a, b, c]` represents the coordinates.
* **Strategy:** Check the `coord_type`.
    * `'cartesian'`: The list is `[x, y, z]`.
    * `'spherical'`: The list is `[r, theta, phi]` (Radius, Latitude, Longitude).
    * *Exam Tip:* If you see $\pi$ in the list (e.g., `[1, pi, 0]`), it's almost certainly **spherical** coordinates because $\pi$ is an angle.

---

## 📐 2. Linear Algebra & State Vectors

### **Q: Converting Ket to Bra**
**The Problem:** Given Ket $| \psi \rangle = |0\rangle|1\rangle$, find the Bra $\langle \psi |$.
* **The Logic:**
    1.  **Tensor Product:** First, expand the Ket. $|0\rangle|1\rangle$ means $|0\rangle \otimes |1\rangle$. In the standard basis:
        $$|0\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \quad |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix} \implies |01\rangle = \begin{pmatrix} 1 \\ 0 \end{pmatrix} \otimes \begin{pmatrix} 0 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix}$$
    2.  **Conjugate Transpose:** To get the Bra, turn the column into a row. Since the numbers are real (0 and 1), you don't need to change signs.
* **Strategy:** Look for the row vector with a "1" in the same position as the "1" in your calculated column vector.

### **Q: Calculating Probabilities from Coefficients**
**The Problem:** Given a state like $|00\rangle + |01\rangle + |10\rangle + |11\rangle$, what is the probability of measuring a specific state?
* **The Logic:** $P(x) = \frac{|\text{Coefficient of } x|^2}{\text{Normalization Factor}^2}$.
* **Strategy:**
    1.  Count the terms. Here we have 4 terms, each with an implicit coefficient of 1.
    2.  Calculate the sum of squares: $1^2 + 1^2 + 1^2 + 1^2 = 4$.
    3.  The normalization factor is $\sqrt{4} = 2$.
    4.  The probability for *any one* state is $(1/2)^2 = 1/4 = 0.25$.
    5.  *Trick:* If the coefficients are different (e.g., $0.374$ and $0.125$), pick the one that matches the specific state you are asked about.

### **Q: Identifying Matrix Types**
**The Problem:** Is Matrix $M$ Unitary? Hermitian? Both?
* **Hermitian Rule ($M = M^\dagger$):**
    * Diagonal elements *must* be real numbers.
    * Off-diagonal elements must be "mirror images" with opposite signs for imaginary parts ($a+bi$ reflects to $a-bi$).
* **Unitary Rule ($M^\dagger M = I$):**
    * Rows/Cols must be normalized (length 1).
    * Rows/Cols must be orthogonal (dot product 0).
* **Strategy:** Check the diagonal first. If you see $i$ on the diagonal, it's **not** Hermitian. If columns look orthogonal (like $[1, 1]$ and $[1, -1]$), check for Unitary.

---

## ⚡ 3. Circuits & Transpilation

### **Q: Circuit Equivalence**
**The Problem:** Which circuit matches the code `circuita.cx(q[1], q[0])`?
* **The Logic:** In Qiskit, `cx(control, target)`.
* **Strategy:**
    * First argument (`q[1]`) is the **Control** (Dot $\bullet$).
    * Second argument (`q[0]`) is the **Target** (Cross $\oplus$).
    * *Visual Check:* Look for the line labeled $q_1$. Does it have the dot? Look for $q_0$. Does it have the cross?

### **Q: What is Transpilation?**
**The Definition:** Real quantum computers don't support every gate you write in code. "Transpilation" converts your abstract circuit into the specific "basis gates" (like CNOT, ID, RZ, SX) that the actual hardware can execute.
* **Strategy:** Look for the answer describing "converting to a universal set of gates favored by the computer".

---

## 🔎 4. Grover's Algorithm

### **Q: The Oracle's Reflection**
**The Concept:** The Oracle $\hat{U}_f$ marks the solution $|a\rangle$ by flipping its phase.
* **Geometric Interpretation:** In the 2D plane spanned by the solution $|a\rangle$ and non-solutions $|a_\perp\rangle$, the Oracle performs a reflection across the axis of the **non-solutions** ($|a_\perp\rangle$).
* **Strategy:** Look for "reflection relative to... vector $|a_\perp\rangle$" or "perpendicular to the solution".

### **Q: The Diffusion Operator's Reflection**
**The Concept:** The Diffusion operator $\hat{W}$ amplifies probability.
* **Geometric Interpretation:** It reflects the state vector across the **initial uniform superposition state** $|\phi\rangle$ (the state before the algorithm starts searching).
* **Strategy:** Look for "reflection relative to... initial vector" or "uniform superposition".

### **Q: Optimal Iterations**
**The Formula:** $r \approx \frac{\pi}{4}\sqrt{N}$.
* **Strategy:**
    1.  If $n=2$ qubits, then $N = 2^2 = 4$.
    2.  Calculate: $\frac{\pi}{4}\sqrt{4} = \frac{\pi}{4}(2) = \frac{\pi}{2} \approx 1.57$.
    3.  Round to the nearest integer: **1 iteration** is usually sufficient for $N=4$.

---

## 🌊 5. QFT & Phase Estimation (QPE)

### **Q: 1-Qubit QFT**
**The Fact:** The Quantum Fourier Transform on a single qubit ($H$) transforms $|0\rangle \to |+\rangle$ and $|1\rangle \to |-\rangle$.
* **Strategy:** This is exactly the definition of the **Hadamard Gate**.

### **Q: Interpreting QPE Histograms**
**The Problem:** You see a peak at a binary value (e.g., `101`). What is the phase?
* **The Logic:** The QPE circuit stores the value $y = 2^m \cdot \theta$ in the register.
    * $\theta$ is the phase (a decimal between 0 and 1).
    * $m$ is the number of qubits in the counting register.
* **Strategy:**
    1.  **Identify the peak:** Which bitstring has the highest probability? (e.g., `10` corresponds to integer 2).
    2.  **Count the qubits:** How many bits in the string? (e.g., `10` is 2 bits, so $m=2$).
    3.  **Calculate Phase:** $\theta = \text{Integer Value} / 2^m$.
        * Example: $2 / 2^2 = 2/4 = 0.5$.
    4.  **Binary Fraction:** Write the bitstring after a decimal point: `0.10`.

---

## 🧮 6. HHL & VQE Algorithms

### **Q: HHL Matrix Requirements**
**The Rule:** HHL solves $Ax=b$. For the quantum simulation $e^{iAt}$ to work directly, $A$ must be **Hermitian** ($A=A^\dagger$). To solve it, $A$ must be **Invertible** ($\det(A) \neq 0$).
* **Strategy:** Look for the option satisfying both "Hermitian" and "Invertible" (Determinant $\neq 0$).

### **Q: Simulation Time 't' in HHL**
**The Concept:** We want to map the eigenvalues of $A$ to phases that fit nicely into our register.
* **The Logic:** If eigenvalues are integers (e.g., $\lambda=2$), we want the phase $\theta$ to be a useful fraction.
    * Phase $\theta = \frac{\lambda t}{2\pi}$.
* **Strategy:** If you are given eigenvalues and a target phase representation (e.g., trying to map $\lambda=1 \to |01\rangle$ and $\lambda=2 \to |10\rangle$), solve for $t$ such that $\frac{\lambda t}{2\pi}$ equals the desired binary fraction (like $1/4$ or $2/4$).
    * *Example:* If $\lambda=1$ needs to map to state `1` ($y=1$) in a 2-qubit register ($m=2$), then $\frac{1 \cdot t}{2\pi} = \frac{1}{2^2} \implies t = \frac{2\pi}{4} = \frac{\pi}{2}$.

### **Q: VQE Algorithm Type**
**The Fact:** VQE does the heavy lifting (state prep, measurement) on a Quantum computer, but the optimization (finding the next parameters) on a Classical computer.
* **Strategy:** Select **Hybrid** (partly quantum, partly classical).

# Solving the Problem Tasks

---

## 🧩 Task 1: Tracing Quantum Circuits
*Goal: Determine the final state vector of a register after passing through a series of gates.*

### The Problem Type
You are given an initial state (usually $|00\rangle$) and a circuit diagram with gates like **Hadamard (H)** and **CNOT**. You need to calculate the state at specific "barriers" or at the very end.

### Step-by-Step Method

**1. Start with the Initial Vector**
For a 2-qubit system starting at $|00\rangle$, your vector is:
$$|\psi_0\rangle = \begin{pmatrix} 1 \\ 0 \\ 0 \\ 0 \end{pmatrix}$$
*(Note: The indices represent states $|00\rangle, |01\rangle, |10\rangle, |11\rangle$)*.

**2. Apply Single Qubit Gates (Tensor Product)**
If a gate (like **H**) is applied to **one** qubit, you must calculate the matrix for the **whole** system using the Tensor Product ($\otimes$).
* **Rule:** If H is on Qubit 0 and nothing (Identity $I$) is on Qubit 1:
    $$M = H \otimes I$$
* **Rule:** If H is on Qubit 1 and nothing is on Qubit 0:
    $$M = I \otimes H$$
* **Calculation:** Multiply this large $4 \times 4$ matrix by your current state vector.

> **Shortcut for Exam:**
> If you apply H to the first qubit of $|00\rangle$, just change that specific qubit's state:
> $$H|0\rangle \otimes |0\rangle = |+\rangle \otimes |0\rangle = \frac{|00\rangle + |10\rangle}{\sqrt{2}}$$

**3. Apply Two-Qubit Gates (CNOT)**
The CNOT (or CX) gate entangles qubits.
* **Identify Control & Target:** In Qiskit/diagrams, the dot is Control, the cross ($\oplus$) is Target.
* **Logic:** "If Control is 1, flip Target."
* **Vector Operation:** Swap the coefficients of the states where the control bit is 1.
    * If Control is Qubit 0 (Left bit in binary): Swap coefficients of $|10\rangle$ and $|11\rangle$.
    * If Control is Qubit 1 (Right bit in binary): Swap coefficients of $|01\rangle$ and $|11\rangle$.

**4. Final State**
Write down the resulting vector.
* Example Result: $\frac{1}{\sqrt{2}}(|00\rangle + |11\rangle)$ (This is a Bell State).

---

## 📊 Task 2: Observables & Eigenvalues
*Goal: Analyze a matrix $M$ representing a physical observable (like Energy).*

### Part A: Eigenvalues and Eigenvectors
**1. Find Eigenvalues ($\lambda$)**
You solve the "Characteristic Equation":
$$\det(M - \lambda I) = 0$$
* Subtract $\lambda$ from the diagonal elements of matrix $M$.
* Calculate the determinant ($ad - bc$ for a $2 \times 2$ matrix).
* Solve for $\lambda$. These are your possible measurement outcomes.

**2. Find Eigenvectors ($|v\rangle$)**
For each $\lambda$ you found, solve the equation:
$$(M - \lambda I)|v\rangle = 0$$
* This gives you a system of linear equations. Solve for the components of $|v\rangle$.
* **Crucial Step:** **Normalize** the vector! Divide by its length so that $\langle v | v \rangle = 1$.

### Part B: Expected (Average) Value
**Method:** Calculate the "sandwich":
$$\langle M \rangle = \langle \psi | M | \psi \rangle$$
1.  **Ket:** Start with your state column vector $|\psi\rangle$.
2.  **Operation:** Multiply the matrix $M$ by the vector $|\psi\rangle$. This gives you a new vector, let's call it $|\psi'\rangle$.
3.  **Bra:** Take the conjugate transpose of your original state ($\langle \psi |$).
4.  **Result:** Multiply the row vector $\langle \psi |$ by the new column vector $|\psi'\rangle$. The result is a single number (scalar).

---

## 🧱 Task 3: Decomposition Coefficients
*Goal: Break down a complex matrix $H$ into simple Pauli components ($I, X, Y, Z$).*

### The Concept
Any $2 \times 2$ matrix can be written as a sum:
$$H = aI + bX + cY + dZ$$
We need to find the numbers $a, b, c, d$.

### The "Trace Trick" Method
There is a magic formula to find these coefficients using the **Trace** (sum of diagonal elements):
$$c_k = \frac{1}{2} \text{Tr}(H \cdot \sigma_k)$$
* Where $\sigma_k$ is one of the Pauli matrices ($I, X, Y, Z$).

**Step-by-Step:**
1.  **Find coefficient for I:** Calculate $\text{Tr}(H \cdot I) = \text{Tr}(H)$. Divide by 2.
2.  **Find coefficient for X:** Multiply $H \cdot X$. Calculate the Trace. Divide by 2.
3.  **Find coefficient for Y:** Multiply $H \cdot Y$. Calculate the Trace. Divide by 2.
4.  **Find coefficient for Z:** Multiply $H \cdot Z$. Calculate the Trace. Divide by 2.

*Example:*
To find the $Z$ coefficient for matrix $H$:
$$d = \frac{1}{2} \text{Tr}\left( \begin{pmatrix} H_{00} & H_{01} \\ H_{10} & H_{11} \end{pmatrix} \cdot \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix} \right) = \frac{1}{2} (H_{00} - H_{11})$$

---

## 🔄 Task 4: Trace of a Product
*Goal: Calculate $\text{Tr}(AB)$ without doing full matrix multiplication.*

### The Definition
The trace is the sum of the diagonal elements:
$$\text{Tr}(M) = \sum M_{ii}$$

### The Shortcut Method (Inner Product)
The trace of a product $\text{Tr}(A^\dagger B)$ is actually defined as the **Hilbert-Schmidt Inner Product** of the two matrices.

If you are asked for $\text{Tr}(AB)$:

1.  You *could* multiply matrix $A$ and matrix $B$ fully, then sum the diagonal.
2.  **Faster Way:** Sum the products of rows of $A$ and columns of $B$ corresponding to diagonal positions only.

$$(AB)_{00} = (\text{Row } 0 \text{ of } A) \cdot (\text{Col } 0 \text{ of } B)$$

$$(AB)_{11} = (\text{Row } 1 \text{ of } A) \cdot (\text{Col } 1 \text{ of } B)$$

$$\mathrm{Trace} = (AB)_{00} + (AB)_{11} + \dots$$

### Important Properties to Remember
* **Cyclic Property:** $\text{Tr}(ABC) = \text{Tr}(BCA) = \text{Tr}(CAB)$. You can cycle the matrices, but you cannot swap them arbitrarily.
* **Linearity:** $\text{Tr}(A + B) = \text{Tr}(A) + \text{Tr}(B)$.