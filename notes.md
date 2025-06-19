# Biologically-Inspired Algorithms & Models

## 1. Tabu Search
- **Tabu list**: stores recent moves/solutions to avoid cycles  
- **Tabu tenure**: number of iterations a move remains forbidden  
- **Aspiration**: override tabu if solution is sufficiently good  
- **Candidate list**: subset of neighborhood for efficiency  
- **Intensification vs. Diversification**

---

## 2. Simulated Annealing
- **Temperature** and **cooling schedules** (geometric, logarithmic, adaptive)  
- **Metropolis criterion**: \(P = e^{(f_{\rm old}-f_{\rm new})/T}\)  
- Balances exploration (high \(T\)) with exploitation (low \(T\))

---

## 3. Evolutionary Algorithms
- **Genotype vs. Phenotype**  
- **Selection**: roulette-wheel, tournament, rank-based, lexicase  
- **Crossover** and **mutation** operators  
- **Fitness scaling**: linear, power-law, σ-truncation  
- **Takeover time** and **selective pressure**  

---

## 4. Advanced Topics
- **Genetic drift** & **founder effect**  
- **Punctuated equilibria**  
- **GOMEA**: linkage trees & optimal mixing  
- **Quality-Diversity**: MAP-Elites, Novelty Search, NSLC  
- **Coevolution**: cooperative vs. competitive  

*(Extend these notes as you like; they’ll render nicely in the “Notes” tab.)*