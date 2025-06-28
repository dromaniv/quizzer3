# Lecture 1: Introduction to Process Models

## Normative vs Descriptive Models
- **Normative model**  
  - A handcrafted, idealized representation of “how things should work.”  
  - Applications: process design, specification, certification (e.g., ISO 9001), contract between stakeholders.
- **Descriptive model**  
  - A data-driven representation of “what actually happens.”  
  - Applications: operational audits, conformance checking, root-cause analysis.

## Six Sigma Management Methodology
- **Goal:** Reduce process variation so that outcomes fall within ±6 standard deviations (σ) of the mean.  
- **Key concepts:**  
  1. **σ (standard deviation)** – measure of variation.  
  2. **1.5σ shift**  
     - Empirical mean drift over time in long-running processes.  
     - Causes asymmetric defect rates but still maintains very low defect probability (≈3.4 PPM).  
  3. **DMAIC cycle:**  
     - Define → Measure → Analyze → Improve → Control

## Lean Management Methodology
- **Origin:** Toyota Production System.  
- **Principles:**  
  1. **Eliminate waste:** reduce inventory, unnecessary transport, overprocessing.  
  2. **Minimize waiting:** synchronize activities to avoid idle time.  
  3. **Prevent defects:** build quality in; avoid rework and correction steps.

---

# Lectures 2–3: Process Model Representations & Workflow Systems

## Control-Flow Representations
1. **Transition Systems (State-Transition Graphs)**
   - **Syntax:** States (nodes) and transitions (edges).
   - **Semantics:** The system moves from one state to another by executing transitions.
   - ➜ Good for representing all possible sequences but gets large quickly.

2. **Petri Nets**
   - **Syntax:**
     - Places: Represent conditions or states.
     - Transitions: Represent activities/events.
     - Arcs: Connect places and transitions.
     - Tokens: Mark the current state.
   - **Execution Semantics:**
     - A transition is enabled if all input places have tokens.
     - When it fires, it consumes tokens from input places and produces tokens in output places.

3. **Workflow Nets (WF-nets)**
   - **Syntax:** A specialized Petri net with:
     - One unique start place (no input transitions).
     - One unique end place (no output transitions).
   - **Execution Semantics:** Same as Petri nets, but with additional soundness criteria:
     - Safeness: Places hold at most one token.
     - Proper completion: All tokens reach the end place.
     - Option to complete: It’s always possible to reach completion.
     - No dead parts: All parts are reachable.

4. **BPMN (Business Process Model and Notation)**
   - **Syntax:**
     - Events: Start, intermediate, end.
     - Tasks: Activities.
     - Gateways: AND, XOR, OR splits/joins to control flow.
   - **Execution Semantics:**
     - Token flow semantics: tokens move along sequence flows, guided by gateway logic.

5. **YAWL (Yet Another Workflow Language)**
   - **Syntax:**
     - Conditions: Like Petri net places.
     - Tasks: Basic or composite.
     - Multi-instance tasks: Tasks that repeat multiple times.
     - Cancellation regions: Subflows that can cancel other tasks.
     - Gateways: AND/XOR/OR splits and joins, attached to tasks.
   - **Execution Semantics:** Combines Petri net semantics with advanced constructs for workflow patterns.

6. **Process Trees**
   - **Syntax:**
     - Leaf nodes: Activities.
     - Internal nodes: Operators:
       - Sequence (→): Activities happen in order.
       - XOR (⨉): Only one branch taken.
       - AND (∧): Parallel execution.
       - Redo loop (↺): Loops.
   - **Execution Semantics:** Block-structured — always sound by construction (e.g., no deadlocks).

7. **Causal Nets (C-nets)**
   - **Syntax:**
     - Dependency graph of activities.
     - Input/output bindings: Sets of conditions needed to execute an activity.
     - Obligation tokens: Mark which bindings are required.
   - **Execution Semantics:** Defines valid sequences based on bindings.
     - Very expressive, but verifying correctness is NP-complete, so computationally hard.

### Formal Definitions (Petri nets)

- **k‑bounded** – A Petri net is *k‑bounded* if, in every reachable marking, **each** place contains at most *k* tokens.
- **safe** – A special case of k‑boundedness with *k = 1*; no place ever contains more than one token (≡ 1‑bounded).
- **bounded** – A Petri net is *bounded* if there exists a finite *k* such that the net is *k‑bounded*.
- **free‑choice** – A Petri net is *free‑choice* if any two transitions that share an input place have **exactly the same set** of input places.
- **deadlock‑free** – From every reachable marking, at least one transition is enabled; the net can always proceed.
- **live / liveness** – A transition *t* is *live* if, from every reachable marking, it is possible to reach **some** marking in which *t* is enabled.  
  A Petri net is *live* when **all** its transitions are live.
- **proper completion** – In a Workflow net, every token produced during execution is ultimately consumed, leaving exactly one token in the final place and none elsewhere.
- **option‑to‑complete** – From the initial marking, it is always possible to reach the final marking (i.e., the model can finish).
- **sound** – A Workflow net is *sound* if it is bounded, has option‑to‑complete, and guarantees proper completion; additionally, there are no dead transitions (every transition can occur in some execution).

> **Tip for exams:** To *derive* these properties for a given example model, construct its reachability graph (or coverability tree) and inspect:
> 1. Maximum tokens per place → k‑bounded / safe / bounded  
> 2. Enabled transitions at each marking → deadlock‑freedom  
> 3. Ability to re‑enable each transition → liveness  
> 4. Reachability of the unique final marking and residual tokens → proper completion & option‑to‑complete  
> 5. Combine the three Workflow‑net criteria to conclude *soundness*.

## Semantic & Structural Properties

**Structural properties:**
- k-bounded / safe / bounded (Petri nets)
- Free-choice nets

**Semantic properties:**
- Deadlock-free / live
- Proper completion / option to complete / sound (WF-nets)

### Split/Join Support & Soundness Guarantees
  
| Representation | AND Split / Join | XOR Split / Join | OR Split / Join | Sound *by Structure* | Sound *needs Semantic Check* |
| -------------- | --------------- | ---------------- | --------------- | -------------------- | ---------------------------- |
| **Transition system** | — (implicit interleavings) | — | — | — | ✔︎ (well‑defined reachable states) |
| **Petri net** | ✔︎ (parallel branches) | ✔︎ (conflict) | ✖︎ (native) | — | ✔︎ (via reachability analysis) |
| **Workflow net (WF‑net)** | ✔︎ | ✔︎ | ✖︎ | — | ✔︎ (soundness ≡ bounded ∧ option‑to‑complete ∧ proper completion) |
| **BPMN** | ✔︎ (Parallel gateway) | ✔︎ (Exclusive gateway) | ✔︎ (Inclusive gateway) | — | ✔︎ (must check token flow) |
| **YAWL** | ✔︎ | ✔︎ | ✔︎ (advanced OR‑join semantics) | — | ✔︎ (runtime OR‑join check) |
| **Process tree** | ✔︎ (AND node) | ✔︎ (XOR node) | ✖︎ | ✔︎ (block‑structured) | — |
| **Causal net (C‑net)** | ✔︎ (bindings) | ✔︎ (bindings) | ✔︎ (bindings) | — | ✔︎ (NP‑complete soundness test) |

> **Reading the table:**  
> • A **✔︎** means the construct is directly available; **✖︎** means it must be encoded indirectly or is not expressible.  
> • *Sound by structure* means every syntactically valid model is guaranteed to be sound (e.g., process trees).  
> • *Sound needs semantic check* means additional analysis (reachability graph, static verification, runtime checking, etc.) is required.


## Workflow Systems
- Software to execute models:  
  - jBPM, YAWL (open-source), ProM, Celonis, Disco, ProcessM, PM4Py.  
- Key features: model editor, execution engine, monitoring, integration with enterprise systems.

---

# Lecture 3: Performance Metrics & KPIs

## Key Performance Indicators (KPIs)
- **Definition:** Quantitative/qualitative measures of process performance.  
- **Types:**  
  1. **Quantitative:** objective, numeric (e.g., throughput, defect rate).  
  2. **Qualitative:** categorical (e.g., satisfaction levels).  
- **Levels:**  
  - **Activity‐level:** input/output data, resource utilization.  
  - **Process‐level:** lead time, cycle time, cost, quality metrics.

## Overall Equipment Effectiveness (OEE)
- **Formula**: OEE = Availability × Performance × Quality
- **Calculations:**  
  - Availability = operating time / scheduled time  
  - Performance = actual output / theoretical max output  
  - Quality = good units / total units

## Exemplary KPIs
- **Manufacturing:** throughput, cycle time, reject rate, utilization.  
- **Process execution cost:** labor cost per case, material cost, rework cost.

## Time Metrics
- **Lead time:** total elapsed time from start to finish of a case.  
- **Service time:** active processing time for an activity.  
- **Waiting time:** idle time before processing starts.  
- **Synchronization time:** waiting for parallel branches to rejoin.  
- **Example calculation:** given timestamps in a trace, compute each time component.

---

# Lecture 4: Statistical Hypothesis Testing & Data Mining Basics

## Purpose of Statistical Hypothesis Verification
- To assess whether sample data support a given assumption about population parameters.

## Hypothesis Test for Mean
1. **Define H₀ (e.g., μ = μ₀) and H₁.**  
2. **Compute t‐statistic:**  
   t = (x̄ − μ0) / (s / sqrt(n))
3. **Compare to critical t (via quantile).**  
4. **Decide to reject or not reject H₀.**

## Error Types & Multiple Testing
- **Type I error (α):** rejecting true H₀.  
- **Type II error (β):** failing to reject false H₀.  
- **Multiple tests:** inflates overall α (family-wise error) unless corrected (e.g., Bonferroni).

## Bias & Variance in Algorithms
- **Bias:** systematic error due to assumptions; low bias → more flexible model.  
- **Variance:** sensitivity to data fluctuations; high variance → overfitting.  
- **Trade-off:** increasing model complexity lowers bias but raises variance.

## Explaining Decisions & Classifier Evaluation
- **Decision explanation:** use decision trees or rule lists to interpret branching in process.  
- **Evaluation measures:** accuracy, precision, recall, F₁/F_β score, Matthews correlation coefficient (MCC), ROC/AUC.

## Bias of Representation
- Causes: chosen modeling formalism restrictions (e.g., α-algorithm’s no silent/duplicate transitions).

## Apriori Algorithm for Association Rule Discovery
1. **Find frequent itemsets:** support ≥ supₘᵢₙ using iterative k→k+1 generation.  
2. **Generate rules:** for each frequent set, split into X⇒Y with confidence ≥ confₘᵢₙ.  
3. **Quality metrics:** support, confidence, lift.

### Example (abstract)
- supₘᵢₙ=0.3, dataset of 14 transactions:  
  - L₁‐item frequent: {coffee, tea, muffin, bagel}.  
  - L₂‐item frequent: {tea∧muffin, muffin∧bagel, …}.  
  - Derive rules with confₘᵢₙ=0.75.

## Apriori for Sequence Discovery
- Treat each time‐ordered basket as sequence of itemsets.  
- Extend Apriori to sequences: subsequence support ≥ supₘᵢₙ.

## Episode Mining
- **Episode:** DAG of events within sliding time window.  
- **Mining:** slide window over event stream, count episode occurrences; support ≥ supₘᵢₙ.  
- **Difference from process model:** local patterns only; no global start/end, loops, choices, or full concurrency.

---

# Lecture 5: Event Logs & Data Preparation

## Event Log Structure Requirements
- **Case ID** – unique identifier for each process instance (trace).  
- **Activity Name** – label of the executed step.  
- **Timestamp** – precise time of occurrence.  
- **Optional Attributes** – resource, cost, lifecycle transition.  
- **Well‐formedness:** each row = one event; sorted by timestamp per case.

## ETL Process
1. **Extract** – read raw data from source systems (ERP, CRM).  
2. **Transform** – filter, clean, normalize timestamps, derive case IDs.  
3. **Load** – write into event log repository (flat file, XES format).

## Data Warehouse vs. Relational Database
- **Relational DB:** optimized for transactional OLTP; normalized; many small reads/writes.  
- **Data Warehouse:** optimized for OLAP; denormalized star/snowflake schema; historical, aggregated data.

## Scoping
- Define process boundaries: start/end events, included variants, relevant data sources.

## Atomic Activities & Parallelism
- **Atomic activity:** indivisible unit of work in the log.  
- **Parallel atomic activities:** two events with same timestamp or overlapping intervals; represented as separate events with identical timestamps or explicit start/end lifecycle attributes.

## XES Standard
- **Basic Structure:**  
  - **Log** element containing global extensions and traces.  
  - **Trace** element containing events.  
  - **Event** element with attributes (string, date, id, int, float, boolean).  
- **Extensions:** plug‐in modules adding semantics (e.g., Organizational, Time, Lifecycle, Cost).

## Creating an Event Log from a Relational DB
1. Query table(s) to extract case ID, activity, timestamp, and optional fields.  
2. Order by case ID and timestamp.  
3. Export to CSV.  
4. Convert CSV to XES (e.g., using ProM import or PM4Py).

---

# Lecture 6: Process Model Evaluation & Discovery (α-Algorithm)

## Four Measures of Model Evaluation
1. **Fitness:** how much of the log is explained by the model.  
2. **Precision:** extent to which model allows only observed behavior.  
3. **Generalization:** model’s ability to allow unseen but likely behavior.  
4. **Simplicity:** parsimony of model structure (Ockham’s razor).

## Ockham’s Razor
- Prefer the simplest model that adequately fits the data to avoid overfitting.

## α-Algorithm Steps
1. Compute directly‐follows relation from log.  
2. Identify sets of input/output for each task.  
3. Build Petri net places for each pair of input/output sets.  
4. Connect tasks and places to form WF-net.

## Properties & Limitations of α-Algorithm
- **Guarantees:** sound WF-net if no loops of length 1/2 and log complete.  
- **Limitations:**  
  - Fails on short loops (length 1 or 2).  
  - Sensitive to noise and infrequent behavior.  
  - Cannot detect invisible tasks or duplicate activities.

## Handling Loops & Noise
- **Short loops:** detect and merge via specialized heuristics.  
- **Noise filtering:** remove infrequent relations below support threshold.

## Bisimilarity Concepts
- **Bisimilar:** two models exhibit exactly the same possible firing sequences.  
- **Branching bisimilar:** preserve branching structure (choices) up to silent transitions.

---

# Lecture 7: Advanced Discovery & Bias of Representation

## Sources of Representation Bias
- Formalism restrictions (e.g., no OR-joins in basic Petri nets).  
- Algorithmic design choices (e.g., block‐structured assumption in process trees).  
- Pre‐ and post‐processing filters.

## Completeness of Event Log
- **Complete w.r.t. directly‐follows:** all possible directly‐follows pairs appear.  
- **Affecting factors:** process variability, data volume, filtering, parallelism.  
- **Challenges:** highly parallel systems, rare behavior; impractical to observe all variants.  
- **Importance:** incomplete logs lead to incomplete or incorrect models.

## State-Based Regions Algorithm
1. Build transition system from log.  
2. Identify regions (sets of states with consistent transitions).  
3. Convert regions into places to form a Petri net.

## Language-Based Regions Algorithm
1. Derive language (set of traces) from log.  
2. Compute separation pairs for events not co-occurring in traces.  
3. Construct places for each separating pair, connect to transitions.

## Evolutionary Computing for Discovery
- **Approach:** encode candidate models as genotypes; evaluate fitness against log; apply genetic operators.  
- **Representation Features:** variable‐length genomes, operators for crossover/mutation on graph structures, genotypephenotype mapping to valid WF-nets.

---

# Lecture 8: Heuristic & Inductive Miners

## Heuristic Miner
- **Heuristic variant:**  
  - Compute dependency/frequency metrics.  
  - Select arcs above thresholds to build causal graph.  
- **Optimization variant:**  
  - Define objective combining fitness, simplicity, precision.  
  - Use optimizer (e.g., ILP) to select consistent subset of arcs.

## Inductive Miner
1. Discover process tree by recursively splitting log based on cut detection (sequence, XOR, parallel, loop).  
2. Guarantees block-structured, sound models.  
3. **Advantages:** handles noise, guarantees soundness.  
4. **Disadvantages:** may over-split; results depend on cut detection thresholds; less flexible for unstructured processes.

---

# Lecture 9: Linear Programming Models

## LP Model Components
- **Decision variables** x₁, x₂, …  
- **Objective function** (linear): e.g., maximize cᵀx.  
- **Constraints:** A x ≤ b, x ≥ 0.

## Optimal Solution
- Located at a vertex of the feasible polyhedron.

## Linearization Techniques
- **Ratio objective:** maximize x/y → introduce t, constraints x – t y ≥ 0, y t = 1 (approximate via piecewise linear).  
- **Minimax objective:** minimize maxᵢ{aᵢᵀx + bᵢ} → introduce t, constraints aᵢᵀx + bᵢ ≤ t ∀i.  
- **Logical relationships:**  
  - If y = 0 ⇒ x ≤ M·y (big-M method).  
- **Absolute value:** |x| ≤ t → −t ≤ x ≤ t.  
- **Alternative constraints:** represent “either-or” via binary variables and big-M.

## Example Model (ZIMPL)
```zimpl
param n := 3;
set I := {1..n};
param c[I];
param A[I,I];
param b[I];
var x[I] >= 0;
maximize obj: sum <i> c[i] * x[i];
subto cons[j in I]: sum <i> A[j,i] * x[i] <= b[j];
```
(Adjust to specific natural‐language problem.)

---

# Lecture 10: Constraint Synthesis & Grammatical Evolution

## Constraint Synthesis Problem
- **One-class variant:** infer constraints that capture all positive examples without negative samples.  
- **Two-class variant:** infer constraints that separate positive from negative examples.  
- **Properties:** underspecified problem; combinatorial search space; multiple valid solutions.

## Curse of Dimensionality
- As the number of features increases:  
  - Data sparsity grows exponentially.  
  - Distance metrics become less meaningful.  
  - Model training and search become computationally infeasible.

## Grammatical Evolution
- **Mechanism:** evolve integer genome (codon list) interpreted via a predefined grammar to produce candidate solutions (e.g., mathematical expressions, rule sets).  
- **Genotype representation:** sequence of integers; each codon selects a production rule:  
    <Expr> ::= <Expr> "+" <Term> | <Term>  
    <Term> ::= <Term> "*" <Factor> | <Factor>  
    <Factor> ::= "(" <Expr> ")" | <Number>  
  - Each codon value modulo the number of alternatives at a grammar node selects the rule.  
- **Evolutionary process:**  
  1. **Initialization:** random genomes.  
  2. **Mapping:** codons → syntax tree via grammar.  
  3. **Evaluation:** fitness of mapped solution on data.  
  4. **Selection:** choose high-fitness genomes.  
  5. **Crossover/Mutation:** produce new genomes.  
  6. **Iteration** until convergence.

---

# Lecture 11: Conformance Checking & Auditing

## Objectives of Conformance Checking
- Measure alignment between process model and execution log.  
- Detect deviations for compliance and improvement.

## Interpreting Deviations
- **Missing tokens:** model expects activity not executed (omissions).  
- **Extra tokens:** executed activity not allowed by model (insertions).  
- **Timing deviations:** execution order or timing conflicts.

## Audits
- **Financial audit:** verify transactions adhere to financial regulations.  
- **Operational audit:** assess process efficiency and adherence to operational standards.

## Conformance Metrics
1. **Fitness:** degree to which log traces fit the model.  
    $\text{fitness} = 1 - \frac{\text{cost_non_sync}}{\text{cost_all_moves}}$
2. **Precision:** extent model prohibits unobserved behavior.  
3. **Generalization:** ability to allow unseen but plausible behavior.  
4. **Simplicity:** parsimony of model structure (apply Ockham’s razor).

## Token-Based Replay Algorithm
1. Initialize tokens at start place.  
2. For each event in trace:  
   - Fire corresponding transition if enabled.  
   - Record missing tokens (if transition not enabled).  
3. At end, count remaining tokens.  
4. Compute fitness:  
   fitness = 1 − (missing_tokens + remaining_tokens) ÷ produced_tokens

## Footprint Matrix
- Captures directly-follows relations for each pair (a,b):  
  - “→” if a directly followed by b.  
  - “←” if b directly followed by a.  
  - “||” if concurrent.  
  - “#” if unrelated.  
- **Differential footprint:** compare model vs log matrices to highlight discrepancies.

## Linear Temporal Logic Specification & Verification
- **LTL rule preparation:** translate natural language constraint into LTL formula, e.g.:  
  G ( order_placed → F order_paid )  
- **Verification:** check each trace against the formula using model checking or log replay.

---

# Lecture 12: Alignments & Advanced Conformance

## Optimal Alignments
- Align each trace to the model with minimal cost:  
  - **Synchronous move:** event matches transition (cost 0).  
  - **Model move:** transition without event (insert, cost >0).  
  - **Log move:** event without transition (delete, cost >0).  
- **Fitness via alignments:**  
  $\text{fitness} = 1 - \frac{\text{sum(cost_non_sync)}}{\text{sum(cost_all_moves)}}$

## Properties of the Alignment Method
- **Advantages:**  
  - Handles invisible transitions.  
  - Computes precise minimal deviations.  
  - Supports customizable cost functions.  
- **Disadvantages:**  
  - Computationally expensive (search through state space).  
  - May not scale to very large logs/models without optimizations.

### Alignment Procedure
1. **Construct an alignment matrix γ** for every trace σ (length *n*) and model *M*.
2. **Define the move‑cost function δ(aᵢ, bᵢ)**  
   - Synchronous move *(aᵢ = bᵢ ≠ »)* → 0  
   - Model‑only **silent** move *(aᵢ = » ∧ bᵢ = τ)* → 0  
   - Model‑only **visible** move *(aᵢ = » ∧ bᵢ ≠ τ)* → 1  
   - Log‑only move *(aᵢ ≠ » ∧ bᵢ = »)* → 1
3. **Search for optimal alignments** (minimal total cost) using A*, Dijkstra, or ILP.  
   Several optimal alignments may exist for the same trace.

### Fitness Formulas
- **Trace‑level fitness**

  fitness = 1 − cost of optimal alignment / cost of worst alignment,  
  where *γ*<sub>opt</sub> is an optimal alignment and *γ*<sub>worst</sub> is the anti‑optimal alignment of only asynchronous moves.
- **Log‑level fitness** – weight the trace‑level fitness by trace frequencies and average over the entire log.

### Precision via Alignments
After replacing each trace by the bottom row of its optimal alignment (log L′):

$\text{precision}(L, M) = \frac{1}{|E|} \sum_{e \in E} \frac{|en_L(e)|}{|en_M(e)|}$

where *en_L(e)* is the set of activities actually observed after the same history and *en_M(e)* the set enabled in the model.  
Precision ∈ [0, 1]; low precision indicates under‑fitting.

### Alignments vs. Token‑Based Replay

| Aspect | **Alignments** | **Token‑based replay** |
| --- | --- | --- |
| Diagnostic detail | Exact end‑to‑end path, identifies every insertion/omission | Local token counts; later deviations may be hidden |
| Handling duplicates & τ | Natural; τ‑moves cost 0 | Complicates token bookkeeping |
| Quality measures | Fitness, precision, generalization from same alignments | Mainly fitness |
| Complexity | Potentially exponential in model states | Linear; scales better but less precise |
