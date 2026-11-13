# Travelling Salesman Problem: A Comparative Study of Genetic Algorithm Crossover Operators

A reproducible experimental study of how **Partially Matched Crossover (PMX)**, **Order Crossover (OX)**, **Edge Recombination (ER)**, and a **no-crossover baseline (NO-CX)** affect solution quality, convergence, and population diversity when solving the Travelling Salesman Problem (TSP).

The study uses the 22-city **ulysses22** benchmark from TSPLIB. It combines evolutionary computation with statistical analysis to examine not only which configurations find shorter tours, but also how crossover operators influence the search process.

**Authors:** Fardin Ahmed and Sparsh Dubey

---

## Research question

**How do different permutation crossover operators affect the quality and search dynamics of a Genetic Algorithm for the Travelling Salesman Problem?**

The TSP asks for the shortest possible tour that visits every city exactly once and returns to the starting city. A Genetic Algorithm (GA) can represent each candidate tour as a permutation of city identifiers and evolve a population of tours through selection, crossover, and mutation.

However, permutation representations introduce an important constraint: offspring must remain valid tours. Crossover operators therefore differ in how they preserve information from their parent tours.

This project compares three established permutation crossover operators against a no-crossover baseline:

| Configuration | Purpose |
|---|---|
| **PMX** — Partially Matched Crossover | Preserves positional information through a mapping mechanism |
| **OX** — Order Crossover | Preserves a subsequence from one parent and relative ordering from the other |
| **ER** — Edge Recombination | Builds offspring using adjacency information from the parent tours |
| **NO-CX** — No crossover | Baseline used to examine performance without recombination |

The comparison considers final tour quality, convergence behaviour, edge inheritance, and population diversity.

## Key findings

The full experiment contains **120 runs**: 30 matched-seed runs for each of the four configurations.

The main findings include:

- **OX and ER outperformed PMX** in the paired comparisons reported by the study.
- The adjusted pairwise comparison was statistically significant for OX versus PMX (*p* = 0.0341) and ER versus PMX (*p* = 0.0034).
- PMX was not shown to outperform the NO-CX baseline in the reported comparison.
- ER also differed significantly from NO-CX in the reported pairwise analysis.
- OX achieved a 60.0% success rate and ER achieved a 56.7% success rate in the full experiment.
- The diversity and edge-inheritance measurements help explain how the operators explore the search space, beyond comparing final tour lengths alone.

The best-known optimum for `ulysses22` used as the benchmark reference is **7,013**. The study also reports an illustrative best-found tour of **7,142**. These are distinct values: the latter is an experimental result, not the benchmark optimum.

> Results are specific to the `ulysses22` instance and the experimental settings in this repository. They should not be interpreted as a universal ranking of crossover operators for every TSP instance.

## Experimental design

The experiments use a controlled setup so that the crossover configurations can be compared under matched random seeds.

| Parameter | Setting |
|---|---:|
| Problem instance | `ulysses22` |
| Number of cities | 22 |
| Population size | 200 |
| Generations | 1,000 |
| Crossover probability | 0.9 |
| Mutation probability | 0.2 |
| Pilot experiment | 10 seeds per operator |
| Full experiment | 30 matched seeds per configuration |
| Full-experiment seeds | 2000–2029 |
| Number of configurations | 4 |
| Total full-experiment runs | 120 |

A separate pilot experiment used seeds 1000–1009 to check the experimental setup before the full matched-seed comparison.

### Why matched seeds?

Genetic Algorithms are stochastic. Different random initial populations and random operations can produce different outcomes.

Using the same set of seeds across configurations reduces variation caused by random initialisation and supports paired statistical comparisons. Each configuration is therefore evaluated under the same set of full-experiment seeds.

### Measurements

The project records several complementary measures:

- **Final tour length:** the objective value at the end of a run. Lower is better.
- **Success rate:** the proportion of runs that reach the benchmark optimum.
- **Convergence history:** how solution quality changes during the run.
- **Edge inheritance:** how parent-tour edges are represented in offspring.
- **Edge diversity:** the number of unique undirected edges in the population, normalised by the number of edges in one tour.
- **Statistical comparisons:** non-parametric tests are used to compare results across configurations and paired runs.

The edge-diversity measure is:

\[
D_e(t) = \frac{\left|\bigcup_{T \in \mathcal{P}_t} E(T)\right|}{n}
\]

where \(\mathcal{P}_t\) is the population at generation \(t\), \(E(T)\) is the set of undirected edges in tour \(T\), and \(n\) is the number of cities. A higher value means the population contains a wider variety of edges.

## Statistical analysis

The analysis does not rely only on comparing average tour lengths. It uses statistical tests to assess whether observed differences are supported by the experimental results.

The report includes:

- A Friedman test for an overall comparison across the four configurations.
- Pairwise comparisons between configurations.
- Holm adjustment to account for multiple pairwise comparisons.
- Effect-size and confidence-interval reporting for selected paired comparisons.

Selected adjusted pairwise results:

| Comparison | Adjusted *p*-value | Interpretation |
|---|---:|---|
| OX vs PMX | 0.0341 | Statistically significant |
| ER vs PMX | 0.0034 | Statistically significant |

The full statistical outputs are saved in the `results/` directory. See `report.pdf` for the complete methodology, tables, and interpretation.

## Visual results

The repository includes generated figures for performance, convergence, operator dynamics, edge diversity, and example tours.

Relevant files are stored in `figures/`. Open the figures directly in the repository to inspect the experimental results. The complete discussion and figure interpretations are provided in `report.pdf`.

## Repository structure

```text
tsp-crossover-study/
├── data/
│   └── ulysses22.tsp
├── figures/
│   ├── performance and convergence figures
│   ├── edge inheritance and diversity figures
│   └── best-tour visualisations
├── results/
│   ├── ulysses22_pilot10_summary.csv
│   ├── ulysses22_runs.csv
│   ├── ulysses22_history.csv
│   ├── ulysses22_summary.csv
│   ├── ulysses22_friedman_test.csv
│   └── ulysses22_pairwise_tests.csv
├── tsp_study.ipynb
├── report.pdf
└── README.md
```

- `tsp_study.ipynb` contains the main implementation, experiment execution, analysis, and visualisation workflow.
- `data/ulysses22.tsp` contains the TSPLIB benchmark instance.
- `results/` contains the saved run-level data and statistical summaries.
- `figures/` contains the plots generated by the notebook.
- `report.pdf` contains the full research report.

## Getting started

### 1. Clone the repository

```bash
git clone https://github.com/fardinAhmed285/tsp-crossover-study.git
cd tsp-crossover-study
```

### 2. Create and activate a virtual environment

Python 3.12 was used for the experiments.

**Windows:**

```bash
py -3.12 -m venv .venv
.venv\Scripts\activate
```

**macOS / Linux:**

```bash
python3.12 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

The experiment used these package versions:

```bash
pip install numpy==1.26.4 scipy==1.13.1 pandas==2.2.2 matplotlib==3.9.2 deap==1.4.1 tsplib95==0.7.1 jupyter
```

### 4. Launch the notebook

```bash
jupyter notebook tsp_study.ipynb
```

Run the notebook cells in order, from top to bottom.

Make sure `data/ulysses22.tsp` is present in the `data/` directory.

### 5. Reuse saved results or run the experiments again

The notebook checks for existing result CSV files in `results/`.

- If the saved CSV files are present, the notebook can reuse them to generate the analysis and figures without running all simulations again.
- To execute the full set of 120 simulations from scratch, move or remove the relevant cached CSV files from `results/` before running the notebook.

**Note:** Removing cached result files means the full stochastic experiment must run again. This can take substantially longer than regenerating plots and statistical summaries from saved results.

## Reproducibility

The repository includes the benchmark input, experiment notebook, saved run-level outputs, summary tables, statistical-test outputs, and generated figures.

The full experiment uses matched seeds 2000–2029 for each configuration. The pilot uses seeds 1000–1009. The notebook and saved CSV files provide the basis for reproducing or checking the reported analysis.

For the exact implementation details, assumptions, and interpretation of the findings, consult `report.pdf` and `tsp_study.ipynb`.

## Technologies

- **Python 3.12** — experiment implementation
- **DEAP** — evolutionary computation framework
- **NumPy** — numerical operations
- **SciPy** — statistical analysis
- **Pandas** — experiment data handling
- **Matplotlib** — plots and visual analysis
- **TSPLIB / `tsplib95`** — benchmark instance handling
- **Jupyter Notebook** — experiment workflow and analysis

## What this project demonstrates

- Applying Genetic Algorithms to a combinatorial optimisation problem.
- Working with permutation-based representations and crossover operators.
- Designing controlled experiments for stochastic algorithms.
- Using matched random seeds to support fair comparisons.
- Measuring convergence and population diversity.
- Applying non-parametric statistical tests and multiple-comparison correction.
- Building a reproducible Python research workflow with saved experimental outputs.

## Authors

**Fardin Ahmed** · **Sparsh Dubey**

---

If you use this repository as a reference, please cite the project report and inspect the notebook for implementation details.
