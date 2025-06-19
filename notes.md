# Biologically Inspired Algorithms

## 1  Evolutionary Algorithms (EA): the big picture  
- **Definition.** Population-based, stochastic search that iteratively **selects**, **recombines**, and **mutates** candidate solutions, guided by a fitness/objective function.  
- **Typical workflow.**  
  ```  
  initialise P(0)  
  evaluate P(0)  
  while !stop:  
      P' ← select(P)                # fitness-biased sampling  
      P'' ← crossover(P')           # recombination  
      P(t+1) ← mutate(P'')          # variation  
      evaluate P(t+1)  
      t ← t+1  
  return best ever﻿  
  ```  
- **Off-line vs. on-line use.** Off-line cares only about the best-found solution; on-line cares about solution quality at *every* generation.  

---

## 2  Solution representation & genotype → phenotype mapping  
| Encoding | Typical use | Notes |
|----------|-------------|-------|
| Binary bit-strings | classic GA | compact; easy crossover/mutation |
| Real-valued vectors | ES/DE | natural for continuous design variables |
| Variable-length *messy* strings | messy GA | allows underspecified or redundant genes |
| Trees/graphs | Genetic Programming, neural topologies | enables program and structure evolution |

Key concept: **genotype ≠ phenotype** (decoding may involve redundancy, pleiotropy, or polygenicity).  

---

## 3  Variation & selection mechanisms  
### 3.1 Selection  
- Fitness-proportionate, rank, tournament, truncation.  
- Strength of **selection pressure** trades exploration vs. exploitation.  

### 3.2 Crossover  
One-point, two-point, uniform, order-based, *shuffle* (preserves gene semantics), plus advanced operators that respect inversions or linkage.  

### 3.3 Mutation  
Bit-flip, Gaussian, Cauchy, rotation/scale specialised mutations for geometric encodings, *partial complement* to re-introduce diversity.  

### 3.4 Architectures & parameters  
- **Generational** vs. **steady-state** replacement; elitism to retain the best.  
- Core parameters: population size N, crossover rate p_c, mutation rate p_m, termination criterion.  

---

## 4  Theoretical pillars  
| Concept | Essence | Implication |
|---------|---------|-------------|
| **Schema theorem** | Short, low-order, above-average schemata receive exponential trials. | Justifies crossover. |
| **Building-block hypothesis** | Good partial solutions combine to form better ones. | Drives linkage learning. |
| **No-Free-Lunch (NFL)** | Averaged over all problems, no optimiser is superior. | Importance of problem-specific bias. |
| **Deceptive problems** | Local building blocks mislead search; minimal deceptive problem of order 2 illustrates. | Need diversity & linkage adaptation. |
| **Epistasis** | Gene interactions; high epistasis → rugged landscape. | Motivates inversion, messy GA, linkage detection (e.g.\ DLED). |
| **Neutrality & genetic drift** | Flat fitness regions allow drift; may aid exploration. | Neutral networks in GP/ES. |

---

## 5  Local search & classical meta-heuristics (bio-inspired antecedents)  
### 5.1 Neighbourhood-based search  
- **Neighbourhood** N(x): solutions within distance ε of x. Must be connected, limited, meaningful.  
- **Greedy vs. Steepest ascent**: choose first-improving or best-improving neighbour.  

### 5.2 Tabu Search  
Adds *memory*: a **tabu list** forbids recently visited moves; **aspiration** can override bans; **tenure** tunes diversification.  

### 5.3 Simulated Annealing  
Physical analogy: Metropolis acceptance with temperature-controlled probability exp(–ΔE / kT). Slow cooling → global-optimum guarantee in limit.  

These single-solution heuristics later reappear inside hybrid EAs (e.g.\ memetic algorithms).

---

## 6  Key takeaways for Part 1  
1. **Population, variation, selection** form the universal EA loop.  
2. Representation matters: it shapes epistasis and operator design.  
3. Theory (schema, NFL, deception) explains *why* and *when* EAs work.  
4. Classic local-search heuristics supply ideas for intensification/diversification that EAs inherit.  


## 7  Modern evolutionary-algorithm families  

| Family | Core mechanism | Strengths & notes |
|--------|----------------|-------------------|
| **CMA-ES** | Samples a multivariate Gaussian; updates mean & full covariance, shrinking step size when progress slows. | Near-derivative search in continuous, rugged landscapes. |
| **Differential Evolution (DE)** | *Differential mutation*: ω = β + F · δ built from random population vectors; then binomial/exp crossover. | Parameter-light, fast global convergence. |
| **Gene-pool Optimal Mixing EA (GOMEA)** | Learns linkage model (mutual-information clusters) → applies donor-based *optimal mixing* that only accepts fitness-neutral/improving swaps. | Exploits epistasis systematically; often beats canonical GA. |
| **Linkage-learning GA (messy GA, DLED, etc.)** | Detects co-adapted gene groups to protect through operators. | Reduces building-block disruption. |
| **Multi-parent & simplex crossover** | Recombination draws genes from ≥3 parents or convex hull (simplex). | Injects rich variation without high mutation rates. |
| **Genetic Programming (tree/linear)** | Evolves program trees with primitive/terminal sets. | Symbolic regression, controller synthesis. |

---

## 8  Why diversity matters (again)  
Standard tournament or roulette selection collapses populations on deceptive or multi-modal landscapes. Advanced schemes explicitly *shape* the fitness landscape to **flatten peaks**, **isolate niches**, or **archive coverage**.

---

## 9  Diversity-in-fitness methods  

| Technique | Idea (fitness space) | Key trait |
|-----------|---------------------|-----------|
| **FUSS** | Sample a random fitness value in [f_min,f_max]; pick individual closest to it → implicit pressure. | Uniform fitness histogram. |
| **FUDS** | Delete individuals so the *survivors'* fitnesses stay uniformly distributed. | Works when selection is external. |
| **Convection Selection** | Chain of sub-populations; offspring of weak groups can "float up" the hierarchy. | Combines exploration & gradual elitism. |
| **HFC** | Hierarchical buffers with admission/export thresholds prevent "champions" dominating lower tiers. | Sustainable long runs. |

> **Empirical tip.** On rugged design benchmarks, FUSS & ConvSel > FUDS > plain tournament.

---

## 10  Niches, novelty & quality–diversity  

### 10.1 Speciation & Niching  
Fitness is rescaled by similarity denominator:  
new_fitness = orig_fitness / Σ similarity
(or multiplied by 1 + distance). This keeps sub-populations around multiple optima and aids multi-objective fronts.

### 10.2 Novelty Search  
Drops objective altogether; rewards behavioural distance (global or k-nearest). Useful for deceptive tasks and test-set generation.

### 10.3 NSLC  
Novelty Search with Local Competition adds a *second* criterion: novelty **and** local fitness ranking.

### 10.4 MAP-Elites (QD)  
*Archive grid across behavioural features* → keep best elite per cell; mutate elites from random cells. Generates a repertoire of high-performing yet diverse solutions.

---

## 11  Population & evaluation tricks  

- **Dynamic pop-size, aging, regularised evolution** reduce premature convergence under noisy fitness.
- **Cultural algorithms** add a secondary *belief space* that guides variation – "evolution of evolution".
- **Cloning under noise**: replicate promising individuals proportionally, average fitness of clones.

---

## 12  Cooperative & competitive co-evolution  

| Mode | Architecture | Challenge |
|------|--------------|-----------|
| **Cooperative** | Decompose solution into components, each in its own population; assemble team for evaluation. | Credit assignment; maintaining intra-species diversity. |
| **Competitive** | Predator–prey, host–parasite arms race reshapes landscape dynamically (Lotka–Volterra analogue). | Red Queen effect, cycling. |

**Adaptive species count**: spawn a new species on stagnation; cull ones with low marginal contribution.

```
# pseudo-loop for cooperative EA
initialise k species  
while not stop:  
  for each species s: evolve s for g gens  
  assemble representatives → evaluate team fitness  
  update credits & decide on adding/removing species  
```

---

## 13  Take-home insights for practitioners  

1. **Modern EAs specialise**: CMA-ES for continuous, DE for derivative-free global, GOMEA for linkage-heavy, GP for program space.  
2. **Diversity is controllable** – pick a scheme that matches *where* you need spread (fitness vs phenotype).  
3. **Quality–diversity** reframes search as *illumination*: you get a map, not a single hero.  
4. **Interaction matters**: co-evolution or culture can reshape the landscape itself, often yielding solutions unreachable by isolated evolution.  

---

### 13.1 Maintaining Diversity and Niching  

| Goal | Key ideas | Typical knobs |
|------|-----------|---------------|
| **Keep the search global** so evolution does not collapse into a single basin. | *Speciation* (crowding, fitness sharing) partitions the population into quasi-independent niches so that multiple optima can coexist | distance metric, similarity threshold |
| **Uniformly explore the fitness axis.** | *FUDS* (Fitness-Uniform Deletion Scheme) removes individuals from the most crowded fitness sub-interval, while *FUSS* applies the same principle to selection | sub-interval width ε |
| **Reward behavioural novelty, not objective value.** | *Novelty Search* (and NSLC: novelty search with local competition) tracks how different an individual's behaviour descriptor is from its nearest neighbours; selection is driven by novelty score, optionally combined with fitness | k-nearest radius, archive size |
| **Illuminate large feature spaces.** | *MAP-Elites* bins solutions by user-chosen features (e.g. gait speed × energy in legged robots) and tries to find a high-fitness exemplar for every bin, turning optimisation into illumination | bin resolution, mutation rate |

---

### 13.2 Coevolutionary Algorithms  

| Flavour | How it works | Typical pitfalls / fixes |
|---------|--------------|--------------------------|
| **Co-operative coevolution (CCEA)** | Decompose a solution into _m_ components, evolve each in its own sub-population, and evaluate individuals by assembling them with collaborators sampled from the other species  | credit assignment, maintaining partner diversity |
| **Competitive coevolution** | Fitness is relative: predators vs. prey, host vs. parasite, or player vs. environment curricula. The goal is an arms race that drives continual innovation. | cyclic dominance, disengagement; often mitigated by Hall-of-Fame archives |
| **Dynamic number of species** | A stagnating run can _spawn_ a new species; species whose contribution falls below a threshold are deleted  | stagnation detector, contribution metric |

---

### 13.3 Nature-Inspired Mechanisms inside Evolutionary Algorithms  

* **Diploidy & dominance** – storing several chromosomes per individual acts as a latent memory that lets the EA respond faster to changing environments
* **Gene duplication, pleiotropy & redundancy** – introduce extra copies of genes, allow one gene to affect many traits, or let a trait depend on many genes; useful when modelling complex genotype–phenotype maps
* **Structural operators** – inversion, partial complement, adaptive crossover probabilities, self-adaptive mutation step sizes; all aim to protect useful building blocks or adapt operator strength on-line

---

### 13.4 Hybrid / Memetic Approaches  


---

### 13.5 Beyond Classical EA: Modern Hybrids & Representations  

* **CMA-ES & Differential Evolution** – state-of-the-art real-valued optimisers that self-adapt covariance (CMA-ES) or use differential mutation vectors (DE) for robust, rotation-invariant search   
* **Genetic Programming (tree & linear forms)** – evolves executable program trees; well-suited for symbolic regression, controller synthesis and automatic algorithm design
* **Regularised evolution for Neural Architecture Search (NAS)** – applies age-based removal (aging) and simple mutations to discover CNN topologies; combines diploidy-style diversity with deep-learning objectives

---

### 13.6 Open Research Directions  

* **Quality-diversity in high-dimensional descriptor spaces** (scalable MAP-Elites)  
* **Coevolutionary curriculum learning** for reinforcement-learning agents  
* **Parameterless gene-pool optimal mixing** to eliminate manual tuning  
* **Integration with deep generative models** for surrogate-assisted evolution  
* **Responsible & interpretable bio-inspired AI** (preventing pathological behaviours in competitive settings)

---

### Quick Recap  

1. **Diversity mechanisms** keep evolution exploratory.  
2. **Coevolution** turns optimisation into a multi-agent ecosystem.  
3. **Biological metaphors** such as diploidy or gene duplication enrich representation and resilience.  
4. **Hybridisation** (local search, surrogate models, deep nets) brings the best of multiple worlds.