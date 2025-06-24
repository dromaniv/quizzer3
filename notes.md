# Multivariate Statistics Notes

---

## 1  Why multivariate statistics?

* Real-world studies usually record **several correlated measurements per unit** (e.g. soil nutrients, exam sub-scores, cranial dimensions).  
* Goals:  
  1. **Describe dependence** (covariance, correlation, scatter-plot ellipsoids).  
  2. **Reduce dimension** without throwing information away (PCA, factor analysis).  
  3. **Test hypotheses** on several means/relationships **while controlling the family-wise error rate**.  
  
Typical designs and their classical tools:

| Design | Multivariate tool |
|-------------------------------------|-------------------------------------------------|
| One sample, many variables          | PCA, factor analysis, Mardia normality tests    |
| Two samples                         | Hotelling T², Linear Discriminant Analysis      |
| $k\ge 3$ samples                    | MANOVA, multiple discriminant functions         |
| Two *sets* of variables (same units)| Canonical correlation, multivariate multiple regression |

---

## 2  Matrix toolbox (speak it fluently!)

### 2.1  Building blocks
* Vectors/matrices/scalars: $A\in\mathbb{R}^{p\times q}$, $x\in\mathbb{R}^p$.  
* Special matrices: identity $I_p$, zero $0_{p\times q}$, ones $1_p1_p^{\top}$, diagonal,  
  *orthogonal* ($UU^{\top}=U^{\top}U=I$), *idempotent* ($P^{2}=P$).

### 2.2  Operations & identities
* $\det(AB)=\det A\;\det B$, $\operatorname{tr}(AB)=\operatorname{tr}(BA)$.
* Rank: $\operatorname{rank}(AA^{\top})=\operatorname{rank}A$.

### 2.3  Projectors
Orthogonal projector onto the column space of $A$:

$$
P_A \;=\; A\,(A^{\top}A)^{-1}\,A^{\top}.
$$

Used everywhere: sample mean, residual maker $Q = I - P$, etc.

### 2.4  Eigen- & singular-value machinery
* **Spectral decomposition** (symmetric $A$): $A = UDU^{\top}$ with eigenvalues on $D$.
* **Positive definite** $\Leftrightarrow$ eigenvalues $>0$ $\Leftrightarrow$ leading principal minors $>0$ (Sylvester). 
* **Singular Value Decomposition (SVD)** for any $A$: $A = U\,\Sigma\,V^{\top}$.  
  Geometric sequence of actions: **rotation → scaling → rotation**. 

> Take-away: every multivariate statistic is a trace, determinant, or eigenvalue of a projector or scatter matrix.

---

## 3  Random vectors and their moments

Let $X = (X_1,\dots,X_p)^{\top}$.

* Mean $\mu = E(X)$.  
* Covariance matrix  
  $$
    \Sigma \;=\; E\!\bigl[(X-\mu)(X-\mu)^{\top}\bigr].
  $$

* **Correlation matrix**  
  $$
  R \;=\; D_{\Sigma}^{-1}\,\Sigma\,D_{\Sigma}^{-1},\quad
  D_{\Sigma}=\operatorname{diag}(\sigma_1,\dots,\sigma_p).
  $$

### Linear transforms
If $Y = AX + b$ (constant $A$, $b$):
$$
E(Y)=A\mu+b, \qquad
\operatorname{Cov}(Y)=A\Sigma A^{\top}.
$$

---

## 4  The multivariate normal distribution $N_p(\mu,\Sigma)$

* Definition: **all** linear combinations $a^{\top}X$ are univariate normal.  
* Density  

  $$
  f(x) \;=\; (2\pi)^{-p/2}\,|\Sigma|^{-1/2}
             \exp\!\Bigl[-\tfrac12\,(x-\mu)^{\top}\Sigma^{-1}(x-\mu)\Bigr].
  $$

* Key facts  
  * Linear transform: $AX+b \sim N_p(A\mu+b,\;A\Sigma A^{\top})$.  
  * Standardise: $Z=\Sigma^{-1/2}(X-\mu)\sim N_p(0,I)$.  
  * Independence $\Leftrightarrow$ $\Sigma$ is diagonal.  
  * Density contours are ellipsoids with axes $\pm c\sqrt{\lambda_i}\,v_i$ (eigen-pairs of $\Sigma$). 
  
---

## 5  Sampling from $N_p$: statistics & distributions

Let $X_1,\dots,X_n \stackrel{\text{iid}}{\sim} N_p(\mu,\Sigma)$ and stack rows into $X$ ($n\times p$).

| Statistic | Formula | Sampling distribution |
|-----------|---------|-----------------------|
| Sample mean | $\displaystyle \bar X = \frac{1}{n}\sum_{i=1}^n X_i \;=\; \frac{1}{n}X^{\top}1_n$ | $N_p\!\bigl(\mu,\frac{1}{n}\Sigma\bigr)$ |
| Sample covariance | $\displaystyle S = \frac{1}{n-1}\,X^{\top}Q_{1_n}X,\quad Q_{1_n}=I-\frac{1}{n}1_n1_n^{\top}$ | Wishart $W_p(\Sigma,n-1)$ |
| Independence fact | $\bar X$ and $S$ are independent | (true only under normality) |

MLEs: $\hat\mu=\bar X,\;\hat\Sigma_{\text{ML}}=\frac{1}{n}X^{\top}Q_{1_n}X=\frac{n-1}{n}S$. 

---

## 6  Assessing multivariate normality

### 6.1  Graphical method  
* **Q–Q plot** of squared Mahalanobis distances $d_i^{\,2}$ versus $\chi^2_p$ quantiles. Straight line ⇒ nearly normal. 

### 6.2  Mardia’s skewness & kurtosis

* Skewness  

  $$
  b_{1,p} \;=\; \frac{1}{n^2}\sum_{i,j}
                \Bigl[(x_i-\bar x)^{\top}S^{-1}(x_j-\bar x)\Bigr]^3,
  \qquad
  z_1 \;=\; \frac{n}{6}b_{1,p}\sim\chi^2_{p(p+1)(p+2)/6}.
  $$

* Kurtosis  

  $$
  b_{2,p} \;=\; \frac{1}{n}\sum_i d_i^{\,4},
  \qquad
  z_2 \;=\; \frac{b_{2,p}-p(p+2)}{\sqrt{8p(p+2)/n}}\sim N(0,1).
  $$ 

### 6.3  Univariate normality tests  
Shapiro-Wilk, Lilliefors, etc. on each variable are *necessary but not sufficient*; joint normality is stricter.  

> **Exam tip:** “the same decision will be made as in univariate tests” is **false**.

---

## 7  Mahalanobis distance & outliers

$$
d(X,\mu) \;=\; \sqrt{(X-\mu)^{\top}\Sigma^{-1}(X-\mu)}.
$$

Replacing $(\mu,\Sigma)$ with $(\bar X,S)$ gives

$$
\frac{p(n-1)}{n-p}\,d^2 \;\sim\; F_{p,\;n-p}.
$$

Large $d^2$ flags multivariate outliers.

---

## 8  Principal Component Analysis (PCA)

### 8.1  Purpose  
* Produce **uncorrelated linear combos** capturing maximum variance.  
* Useful for **dimension reduction, visualisation, multicollinearity relief**. 

### 8.2  Algebra  
Solve the eigenproblem of $S$ (or $R$):

$$
S\,a_j \;=\; \lambda_j\,a_j,
\quad
\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_p.
$$

PC scores: $Z_j = a_j^{\top}Y$ where $Y$ is centred data,  
$\operatorname{Var}(Z_j)=\lambda_j$.

Matrix form: $Z = A^{\top}Y,\;A=[a_1,\dots,a_p],\;A^{\top}SA=\operatorname{diag}(\lambda_1,\dots,\lambda_p)$. 

### 8.3  Choosing the number of PCs  
* **Variance rule:** keep first $k$ PCs giving ≥ 80 % of total variance.  
* **Kaiser rule:** retain PCs with $\lambda_i > \bar\lambda$.  
* **Scree plot:** look for the “elbow”.  

### 8.4  Geometry  
PCA rotates the sample ellipsoid so its axes align with coordinate axes; PCs are those axes.

### 8.5  Links  
* First PC often approximates the **major regression line** when variables are highly correlated.  
* PC1-PC2 plots reveal clusters and multivariate outliers.

---

## 9  Hotelling $T^{2}$

### 9.1  One-sample mean

* Hypotheses  
  $H_0:\ \mu = \mu_0$   vs   $H_1:\ \mu \neq \mu_0$

* Statistic  
  $$
    T^2
      = n\,(\bar X - \mu_0)^{\top} S^{-1} (\bar X - \mu_0),
  $$  
  transform with  
  $$ 
    F = \frac{n-p}{p(n-1)}\,T^2 \sim F_{p,\;n-p}\quad(H_0).
  $$

### 9.2  Two independent samples

* Pooled covariance  
  $$ 
    S_p = \frac{(n_1-1)S_X + (n_2-1)S_Y}{n_1+n_2-2}.
  $$

* Statistic  
  $$
    T^2
      = \frac{n_1 n_2}{n_1+n_2}
        (\bar X - \bar Y)^{\top}
        S_p^{-1}
        (\bar X - \bar Y).
  $$

* Convert to $F_{p,\;n_1+n_2-p-1}$ the same way.

### 9.3  Paired version

Take differences $D_i = X_i - Y_i$ and apply the one-sample $T^2$.

---

## 10  MANOVA ( $g\ge 2$ groups )

### 10.1  Sum-of-squares matrices

Let $Y_{ij}$ be the $p$-vector for subject $i$ in group $j$.

* **Error (within)**  
  $$
    E = \sum_{j=1}^{g}\sum_{i=1}^{n_j}
        (Y_{ij}-\bar Y_{j\cdot})(Y_{ij}-\bar Y_{j\cdot})^{\top}.
  $$

* **Hypothesis (between)**  
  $$
    H = \sum_{j=1}^{g}
        n_j(\bar Y_{j\cdot}-\bar Y_{\cdot\cdot})
        (\bar Y_{j\cdot}-\bar Y_{\cdot\cdot})^{\top}.
  $$

### 10.2  Four omnibus statistics (functions of eigenvalues $\lambda_i$ of $E^{-1}H$)

| Name | Formula | Range | Best for |
|------|---------|-------|----------|
| Wilks $\Lambda$ | $\displaystyle \Lambda=\frac{|E|}{|E+H|}$ | $0\!-\!1$ (smaller → evidence) | general use |
| Pillai $V$ | $V=\text{tr}\,[H(H+E)^{-1}]$ | $0\!-\!s$ ($s=\min(p,g-1)$) | robust |
| Hotelling–Lawley $T$ | $T=\text{tr}(E^{-1}H)$ | $0\!-\!\infty$ | power vs big effects |
| Roy’s root $\Theta$ | $\Theta=\lambda_{\max}(E^{-1}H)$ | $0\!-\!\infty$ | one dominant effect |

Each has an $F$ (or $\chi^2$) approximation; the constants are on your formula sheet.

---

## 11  Box’s $M$ (test equality of covariance matrices)

* Hypotheses   $H_0:\ \Sigma_1=\Sigma_2=\dots=\Sigma_g$

* Statistic  
  $$
    M = \Bigl[ \sum_{j=1}^g (n_j-1)\ln|S_j|
          - (N-g)\,\ln|S_p| \Bigr].
  $$

* After correction factor $C$, use  
  $$
    \chi^2 = C\,M \approx \chi^2_{\;\nu},\quad
    \nu=\tfrac12 p(p+1)(g-1).
  $$  
  *Very* sensitive to non-normality; treat as a diagnostic.

---

## 12  Multivariate multiple regression

### 12.1  Model
$$
  Y = X B + U,\qquad
  U \sim N_{n,p}(0,\;I\otimes\Sigma).
$$

### 12.2  Estimates
$$
  \hat B = (X^{\top}X)^{-1}X^{\top}Y,\qquad
  \hat\Sigma = \frac{1}{n-q}(Y-X\hat B)^{\top}(Y-X\hat B).
$$

### 12.3  Testing $H_0: L B = 0$

Compute $E$ from residuals; compute $H$ from the reduced model fit; plug into Wilks/Pillai/etc.

---

## 13  Discriminant analysis

### 13.1  Linear DA (LDA)

Assume class densities $N_p(\mu_k,\Sigma)$, common $\Sigma$.

Discriminant score  
$$
  \delta_k(x)=x^{\top}\Sigma^{-1}\mu_k
              -\tfrac12\mu_k^{\top}\Sigma^{-1}\mu_k
              +\ln\pi_k.
$$

Classify to the largest $\delta_k$.

### 13.2  Quadratic DA (QDA)

Drop the equal-$\Sigma$ assumption; discriminant becomes quadratic.

### 13.3  Error rate estimates

* Apparent (resubstitution)  
* Leave-one-out / $k$-fold CV  
* Confusion matrix, ROC for binary

### 13.4  Number of functions

Up to $\min(p,\,g-1)$ canonical discriminant functions for plotting.

---

## 14  Canonical correlation (30-second view)

Find $a^{\top}X$ and $b^{\top}Y$ that maximise $\operatorname{corr}(a^{\top}X,\;b^{\top}Y)$.  
Solve eigenproblem of $\Sigma_{XX}^{-1}\Sigma_{XY}\Sigma_{YY}^{-1}\Sigma_{YX}$.

---

## 15  Tolerance regions & $T^2$ control chart

* Ellipsoidal tolerance for proportion $P$:
  $$ 
    (x-\bar X)^{\top} S^{-1} (x-\bar X) \le
    \frac{p(n-1)}{n-p}\,F_{p,\;n-p;\;P}.
  $$

* Hotelling $T^2$ chart compares each new sample’s distance to the in-control limit.

---

## 16  Cheat-sheet of crucial formulas

| Situation | Test | Statistic | Ref. dist. |
|-----------|------|-----------|------------|
| One-sample mean | Hotelling $T^2$ | $n(\bar X-\mu_0)^{\top}S^{-1}(\bar X-\mu_0)$ | $F_{p,\;n-p}$ |
| Two-sample means | Hotelling $T^2$ | $\dfrac{n_1 n_2}{n_1+n_2}(\bar X-\bar Y)^{\top}S_p^{-1}(\bar X-\bar Y)$ | $F_{p,\;n_1+n_2-p-1}$ |
| MANOVA | Wilks | $\Lambda=|E|/|E+H|$ | $\approx F$ |
| MANOVA | Pillai | $V=\text{tr}[H(H+E)^{-1}]$ | $\approx F$ |
| Covariance equality | Box’s $M$ | see §11 | $\chi^2$ |
| Classification | LDA rule | $\delta_k(x)$ | — |

---

# Questions from last year

### **1. Let $A$ be a symmetric matrix.  Which property *does not* hold?**

| Option | Statement | Verdict | Reason |
|--------|-----------|---------|--------|
| (a) | If all leading minors of $A$ are positive then $A$ is positive-definite | ✘ | Sylvester’s criterion – this is true. |
| (b) | If all eigenvalues of $A$ are positive then $A$ is positive-definite | ✘ | Positive-definite *means* all eigenvalues $>0$. |
| **(c)** | If $A$ has *all entries* positive, then all eigenvalues of $A$ are positive | ✔ | Entry-wise positivity does **not** imply positive eigenvalues (e.g. matrix of all 1’s). |

---

### **2. SVD corresponds to which sequence of transformations?**

| Option | Sequence | Verdict | Reason |
|--------|----------|---------|--------|
| (a) | scaling horizontally → rotation → scaling vertically | ✘ | SVD has one scaling phase, not two. |
| (b) | scaling vertically → rotation → scaling horizontally | ✘ | Same issue. |
| **(c)** | rotation → scaling → rotation | ✔ | $A = U\,\Sigma\,V^{\top}$ ($V^{\top}$ rotates, $\Sigma$ scales, $U$ rotates). |

---

### **3. For the linear transform $y = A\,x + b$ (nonsingular $A$)**

| Option | Claim about $\operatorname{Cov}(A^{-1}y)$ | Verdict | Reason |
|--------|-------------------------------------------|---------|--------|
| (a) | $A\,\Sigma\,A^{\top}$ | ✘ | Forgot the two $A^{-1}$ factors. |
| (b) | $I_p$ | ✘ | Only if $A$ orthogonal **and** $\Sigma = I_p$. |
| **(c)** | $\Sigma$ | ✔ | $A^{-1}y = x + A^{-1}b$ so the covariance is $\Sigma$. |

---

### **4. Which $\Sigma$ matches the horizontally elongated contours?**

| Option | $\Sigma$ | Verdict | Reason |
|--------|----------|---------|--------|
| (a) | $\begin{pmatrix}4&0\\0&1\end{pmatrix}$ | ✘ | Large variance on $x$-axis would stretch *vertically*. |
| **(b)** | $\begin{pmatrix}1&0\\0&4\end{pmatrix}$ | ✔ | Larger variance in $y$ gives horizontal stretch; zero covariance keeps axes aligned. |
| (c) | $\begin{pmatrix}2&1\\1&2\end{pmatrix}$ | ✘ | Positive covariance would tilt the ellipse. |

---

### **5. With $x_i \sim N_p(\mu,\Sigma)$ and $X = (x_1,\dots,x_n)^{\top}$, which statement is *false*?**

| Option | Statement | Verdict | Reason |
|--------|-----------|---------|--------|
| (a) | $\displaystyle \bar x = \frac1n X^{\top}1_n \sim N_p\!\bigl(\mu,\tfrac1n\Sigma\bigr)$ | ✔ | True. |
| (b) | $(n-1)S = X^{\top}Q_{1_n}X \sim W_p(\Sigma,n-1)$ | ✔ | True. |
| **(c)** | $\bar x$ and $(n-1)S$ are *dependent* | ✘ | They are independent under multivariate normality. |

---


### **6. When testing multivariate normality**

| Option | Statement | Verdict |
|--------|-----------|---------|
| **(a)** | Use Q-Q plot of Mahalanobis distances (beta envelope) | ✔ |
| **(b)** | Use a test based on skewness × kurtosis | ✔ |
| (c) | The same decision is reached by testing each variable univariately | ✘ |

---

### **7. Which sentence is true?**

| Option | Statement | Verdict | Reason |
|--------|-----------|---------|--------|
| (a) | Wilks’ $\Lambda$ and Lawley–Hotelling test several sample means | ✘ | Wording imprecise; they test population mean vectors. |
| (b) | Pillai’s and $T^2$ test $>\!2$ population means | ✘ | Hotelling $T^2$ works only for two means. |
| **(c)** | Box’s test compares several covariance matrices | ✔ | Exactly its purpose. |

---

### **8. In the multivariate multiple regression model**

| Option | Description | Verdict |
|--------|-------------|---------|
| (a) | Many $Y$ on a single predictor | ✘ |
| (b) | Single $Y$ on many predictors | ✘ |
| **(c)** | Many $Y$ on many predictors | ✔ |

---

### **9. PCA decreases dimensionality by**

| Option | Mechanism | Verdict |
|--------|-----------|---------|
| (a) | Removing non-significant variables | ✘ |
| **(b)** | Taking linear combinations of variables | ✔ |
| (c) | Removing strongly correlated variables | ✘ |

---

### **10. Discriminant analysis finds a vector corresponding to the largest eigenvalue of**

| Option | Matrix | Verdict |
|--------|--------|---------|
| (a) | Sample correlation matrix $R$ | ✘ |
| (b) | Sample covariance matrix $S$ | ✘ |
| **(c)** | $E^{-1}H$ from MANOVA | ✔ |

The generalized eigenproblem for LDA is $E^{-1}H\,a = \lambda\,a$.

---

## Quick answer key

| Q | Correct option(s) |
|---|-------------------|
| 1 | **(c)** |
| 2 | **(c)** |
| 3 | **(c)** |
| 4 | **(b)** |
| 5 | **(c)** *(false)* |
| 6 | **(a)**, **(b)** |
| 7 | **(c)** |
| 8 | **(c)** |
| 9 | **(b)** |
|10 | **(c)** |