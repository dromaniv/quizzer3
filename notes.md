# Last year's

**1. In some population, each individual can possess one of three traits (gene values): A, B, or C.  
In this population, the proportion is A (30 %), B (60 %), and C (10 %).  
Assume no selective pressure and no mutations – i.e. pure genetic drift.  
What is the probability that after a sufficiently large number of generations, when one trait takes over the entire population, this dominating trait will be C?**

Because only random genetic drift is acting, the chance that a neutral trait finally fixes equals its current frequency. Trait C starts at 10 %, so its fixation probability is **0.10 (10 %)**.

---

**2. Describe the risk of using an overly simple pseudorandom number generator in an optimisation algorithm (compared to a sufficiently complex pseudorandom number generator).**

A very simple PRNG can have a short period, noticeable correlations and poor coverage of the search space. These artefacts may force the algorithm to revisit the same points, follow hidden cycles or synchronise with genetic operators, lowering diversity and causing premature or misleading convergence. A long-period, statistically well-tested PRNG avoids these pitfalls and gives more reliable optimisation behaviour.

---

**3. Describe σ-truncation scaling used in selection (just the formula and the meaning of variables).**

The rescaled fitness is

f′ = max { 0 ,  f − ( µ − c·σ ) }

* **f** – original fitness of the individual  
* **µ** – mean fitness in the population  
* **σ** – standard deviation of fitness values  
* **c** – user-chosen constant (typically 2–3)

All fitness values below the threshold \(µ − cσ\) are set to 0, which keeps selection pressure stable even when the population becomes too uniform or too diverse.

---

**4. Describe the procedure to adjust the initial temperature in simulated annealing (a few precise sentences/formulas).**

1. Generate several random uphill moves and measure their average cost increase Δf.  
2. Choose a desired initial acceptance probability \(P₀\) (commonly 0.8–0.99).  
3. Compute the starting temperature with  

T₀ = − Δf / ln P₀

This gives a temperature high enough that almost all worsening moves are accepted at the beginning, ensuring thorough exploration before cooling starts.

---

**5. In competitive coevolution, there were 3 individuals (A, B, C), and each played competitively against each other individual.  
To evaluate them, CFS (competitive fitness sharing) was used.  
A won against B and C; B didn’t win against anyone; C won against B.  
The fitness based on the CFS technique is: A ________  B ________  C ________.**

| Opponent beaten | Beaten by | Credit to each winner |
|-----------------|-----------|-----------------------|
| B               | A, C      | ½                    |
| C               | A         | 1                    |

* **Fitness(A) = 1 + ½ = 1.5**  
* **Fitness(B) = 0**  
* **Fitness(C) = ½ = 0.5**

CFS rewards victories that few others achieve, so A gains full credit for the unique win over C and splits the credit for beating B with C.


---

**6. You have a solution X with 10 binary variables \(x₁…x₁₀\) (e.g. X = 0101011010) and fitness f(X)=5.  
– You flip variable \(x₆\) in X: fitness increases by 5.  
– You flip variable \(x₇\) in X: fitness decreases by 3.  
– You flip both variables \(x₆\) and \(x₇\) in X: fitness increases by 2.  
Are these two variables in epistatic relationship in X? (underline your answer): YES / NO  
If the same relationship between \(x₆\) and \(x₇\) holds for all possible solutions, is this good for optimisation? YES / NO and why?**

The single effects are +5 and –3, so an additive prediction for flipping both is +2, which matches the observed change. Therefore **NO**, the variables are not epistatically linked here.  
If this additive relationship holds everywhere, it is **good** for optimisation, because each variable can be optimised independently, simplifying the landscape.

---

**7. In competitive coevolution, we want the number of necessary species to be established automatically.  
A new species is introduced when …**

…evolution stagnates, typically detected when the best fitness has not improved for a predefined number of generations. Introducing a fresh, randomly initialised species injects new genetic material and can restart progress; redundant or unproductive species may later be removed to keep the system efficient.

---
# What he told us

**1. Is Tabu Search with candidate selection broken?**  
No. The candidate-list idea just tells Tabu Search to inspect only a smart subset of the current neighbourhood, so we save time but still follow the same “best non-tabu (or aspirated) move” logic. If the subset is built sensibly (e.g. elite list, aspiration-plus), the search keeps its determinism and performance; it is therefore not “broken”.

---

**2. How can we measure selective pressure as a single number?**  
A popular metric is **take-over time** – the expected number of generations for one best individual to fill the whole population when only selection operates. Shorter take-over time → stronger pressure. Another common measure is **selection intensity**, the standardized difference between the population’s mean fitness before and after selection.

---

**3. Why is randomness used in optimisation?**  
Random decisions let an algorithm explore many regions of the search space, escape deterministic cycles and break ties between equally good moves. Stochasticity also makes it easier to jump out of local optima (e.g. SA’s uphill moves) and helps when the landscape or the initial ordering of solutions is unknown.

---

**4. Implementation of random**  
Use a high-quality pseudo-random number generator (long period, good equidistribution) or, when strict unpredictability is essential, hardware noise. Avoid simple LCGs except for teaching. Always document the seed to make experiments reproducible; change seeds across runs to estimate variability.

---

**5. Which fitness-scaling methods did we discuss?**  
* **Linear scaling** – \( f' = a f + b \) to keep best:mean ratio constant.  
* **Power-law scaling** – \( f' = f^k \) (rarely used, problem-dependent).  
* **σ-truncation scaling** – subtract \( (µ - cσ) \) so pressure adapts to spread.  
* **Sigmoid/logistic scaling** – converts fitness to probabilities with a smooth S-curve centred on the population median.  
These aim to hold selection pressure at a useful level throughout evolution.

---

**6. What are punctuated equilibria?**  
Long stretches where the population’s best fitness hardly changes are interrupted by short, sharp improvements. The pattern mirrors the palaeontological theory of punctuated equilibrium and often appears in EAs when the search waits for a rare combination (or parameter change) that unlocks a big jump in quality.

---

**7. What is a founder effect?**  
When a new sub-population is started by a few individuals, only their alleles are present, so genetic diversity shrinks and their frequencies can drift to fixation even if they were rare in the original population. This random bottleneck is called the founder effect.

---

**8. Will genetic drift alone (no mutation/crossover) lead to loss of diversity?**  
Yes. Without variation operators every allele performs a random walk in frequency; sooner or later one of them reaches 100 %, leaving the population genetically uniform.

---

**9. What is the only operator of the GOMEA algorithm?**  
GOMEA relies on **Gene-pool Optimal Mixing (GOM)**: for each linkage group, it tries donor gene fragments from the population and keeps the first replacement that improves (or at least does not harm) fitness. No traditional crossover or mutation is used.

---

**10. What is lexicase selection?**  
Parents are chosen by filtering the population through training cases in a random order. On the first case, keep only those with the best error, then test the survivors on the next case, and so on until one individual remains (or ties are broken randomly). This favours specialists that excel on different subsets of tasks.

---

**11. Plot (Fitness vs Probability of selection)**  
For roulette selection the plot is a straight line through the origin: probability rises linearly with fitness. After σ-truncation the curve is zero below the threshold and linear above it, while logistic scaling yields an S-shaped curve steepest around the median and flattening towards 0 and 1 at the extremes.

---

**12. MAP-Elites algorithm**  
* Discretise one or more behaviour descriptors into a grid of cells.  
* Evaluate random individuals and place the best (elite) seen so far in each cell.  
* Repeatedly pick an elite, mutate it, and insert the offspring if it either discovers a new empty cell or outperforms the current elite of that cell.  
The archive gradually “illuminates” the map, giving a collection of high-quality and diverse solutions.

---

**13. Novelty Search**  
Instead of rewarding objective fitness, each individual gets a score equal to how behaviourally different it is from its nearest neighbours (archive + current population). The search thus pushes outward into unexplored behaviours, often finding creative or stepping-stone solutions that pure objective optimisation would miss.

---

**14. Coevolution**  
Multiple populations (or one population with interacting roles) evolve together, and each individual’s fitness depends on others.  
* **Competitive** coevolution (predator-prey, game strategies) can create an arms race but risks cycles or mediocre stable states.  
* **Cooperative** coevolution splits a large problem into species that must collaborate for overall fitness.  
Key issues include maintaining diversity, balanced progress and meaningful evaluation across populations.
