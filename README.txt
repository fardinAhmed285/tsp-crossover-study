================================================================================
EVOLUTIONARY COMPUTATION PRACTICAL WORK - PART 5
The Travelling Salesman Problem: What Should Crossover Preserve?
================================================================================

Authors:     Fardin Ahmed, Sparsh Dubey
Course:      Computational Intelligence (Prof. Lluis A. Belanche)
Program:     Master in Artificial Intelligence
Institution: Universitat Politecnica de Catalunya (UPC)
Benchmark:   TSPLIB ulysses22 (22 cities, GEO distance, Best known tour = 7013)

--------------------------------------------------------------------------------
1. WHAT THIS PROJECT IS ABOUT
--------------------------------------------------------------------------------
In this project, we test different Genetic Algorithm (GA) crossover operators on
the Travelling Salesman Problem (TSP) to answer a simple question:

"Do crossover operators that preserve parent edges (connections between cities)
work better than those that preserve city positions, and is edge inheritance
the actual reason?"

To test this fairly, we compare four setups:
1. PMX (Partially Mapped Crossover): Keeps cities in their original slot
   positions and fixes duplicates using a mapping table.
2. OX (Order Crossover): Copies a slice of cities from one parent and fills
   the rest in the exact relative order they appear in the other parent.
3. ER (Edge Recombination Crossover): Builds a map of city neighbors from both
   parents and picks cities with the fewest remaining connections so parent
   roads (edges) stay intact.
4. NO-CX (Baseline Control): Turns crossover completely off (rate = 0.0).
   Tours evolve using only selection, mutation, and elitism. This shows us
   whether crossover actually helps compared to mutation alone.

--------------------------------------------------------------------------------
2. FOLDER & FILE LAYOUT
--------------------------------------------------------------------------------
.
|-- tsp_study.ipynb                     # Main Jupyter Notebook with all code
|-- report.pdf                          # Final compiled PDF report
|-- README.txt                          # This guide
|-- .gitignore                          # Standard Python/Jupyter ignore rules
|-- data/
|   `-- ulysses22.tsp                   # Official 22-city TSPLIB dataset
|-- results/                            # Saved CSV tables from our runs
|   |-- ulysses22_pilot10_summary.csv   # Pilot test results (10 runs/operator)
|   |-- ulysses22_runs.csv              # Final tour lengths for all 120 runs
|   |-- ulysses22_history.csv           # Progress logged every 10 generations
|   |-- ulysses22_summary.csv           # Overall summary stats for all 4 setups
|   |-- ulysses22_friedman_test.csv     # Overall statistical test output
|   `-- ulysses22_pairwise_tests.csv    # Head-to-head comparison test outputs
`-- figures/                            # Generated PDF charts for the report
    |-- fig2_ulysses22_performance.pdf  # Boxplots & convergence curves
    |-- fig3_mechanism_dynamics.pdf     # Edge inheritance & diversity over time
    `-- fig4_ulysses22_tours.pdf        # Map plots of the best tours found

--------------------------------------------------------------------------------
3. SETUP & REQUIRED LIBRARIES
--------------------------------------------------------------------------------
We ran everything using Python 3.12. Below are the exact package versions used:

- Python 3.12
- numpy 1.26.4
- scipy 1.13.1
- pandas 2.2.2
- matplotlib 3.9.2
- deap 1.4.1
- tsplib95 0.7.1
- jupyter / ipykernel

You can install everything in one command:
    pip install numpy==1.26.4 scipy==1.13.1 pandas==2.2.2 matplotlib==3.9.2 deap==1.4.1 tsplib95==0.7.1 jupyter

Make sure the `data/ulysses22.tsp` file is inside the `data/` folder before
running the notebook.

--------------------------------------------------------------------------------
4. HOW THE EXPERIMENT WORKS (STEP-BY-STEP)
--------------------------------------------------------------------------------
Step 1 - The Map and Distances:
We use `ulysses22.tsp`, which has 22 cities based on Homer's Odyssey. Distances
are calculated using the official TSPLIB spherical Earth formula (GEO) and
stored in a 22x22 lookup table. The shortest possible tour length is 7013.

Step 2 - Keeping Everything Fair (Same Settings):
Except for the crossover operator, every single setting stays identical across
all four setups:
- Tour format:       List of city numbers from 0 to 21
- Population size:   200 tours per generation
- Generations:       1,000 generations (200,000 tour evaluations per run)
- Parent selection:  Tournament selection (pick best out of 3 random tours)
- Crossover rate:    0.90 for PMX, OX, and ER (0.00 for NO-CX)
- Mutation:          Simple Inversion Mutation (reverses a random section of a
                     tour) applied at a 0.20 rate
- Elitism:           The single best tour is always copied to the next round
- Logging:           Metrics are recorded every 10 generations

Step 3 - Tracking What Happens Inside the Population:
- Edge Heritability: Right after crossover (and before mutation), we check what
  fraction of a child's 22 edges came directly from either parent.
- Edge Diversity: Every 10 generations, we count how many unique city-to-city
  edges exist across the entire population of 200 tours, divided by 22. Higher
  numbers mean more variety; 1.0 means every tour in the population is identical.

Step 4 - Matching Random Seeds:
- Pilot study: Seeds 1000 to 1009 (10 runs per operator).
- Main study: Seeds 2000 to 2029 (30 runs per setup = 120 runs total).
Because we reuse the exact same 30 seeds for PMX, OX, ER, and NO-CX, every
setup starts with the exact same 200 random tours on each seed.

Step 5 - Comparing Results Statistically:
Since the runs share the same starting seeds, we treat them as paired data.
We first run a Friedman test to confirm there is a real difference across the
four setups, then run paired Wilcoxon signed-rank tests (with Holm p-value
adjustment) to compare them head-to-head.

--------------------------------------------------------------------------------
5. HOW TO RUN THE CODE (STEP-BY-STEP)
--------------------------------------------------------------------------------
1. Open your terminal in the main project folder and launch Jupyter:
       jupyter notebook tsp_study.ipynb

2. Note on Saved CSV Files (Caching):
   The notebook automatically checks if the result CSV files already exist in
   the `results/` folder:
   - If the CSV files are present, it skips re-running the GA simulations and
     immediately generates the statistical tables and PDF figures in a few
     seconds.
   - If you want to run all 150 evolutionary simulations from scratch, simply
     delete or move the CSV files inside `results/` before running the cells.

3. Run all cells in order from top to bottom ("Run All"):
   - Cell 1: Loads libraries and creates `results/` and `figures/` folders.
   - Cell 2: Reads `data/ulysses22.tsp` and builds the distance table.
   - Cell 3: Sets up PMX, OX, ER, and Inversion Mutation in DEAP.
   - Cell 4: Defines the edge heritability and edge diversity functions.
   - Cell 5: Sets up the main Genetic Algorithm loop.
   - Cell 6: Runs (or loads) the 10-seed pilot study.
   - Cells 7-8: Runs (or loads) the 30-seed main study across all 4 setups and
     saves the run logs and history CSVs.
   - Cell 9: Runs the statistical tests and saves the summary tables.
   - Remaining cells: Creates and saves the PDF plots in `figures/`.

--------------------------------------------------------------------------------
6. MAIN TAKEAWAYS FROM OUR RESULTS
--------------------------------------------------------------------------------
- Overall difference: The Friedman test confirmed a clear statistical
  difference between the four setups (p = 0.000381).
- OX and ER beat PMX: Both OX (found optimal 7013 in 60.0% of runs, median
  7013.0) and ER (found optimal 7013 in 56.7% of runs, median 7013.0) performed
  much better than PMX (23.3% optimal rate, median 7107.0).
- Crossover beats pure mutation: ER significantly beat the NO-CX baseline
  (40.0% optimal rate, median 7089.5), proving that smart recombination helps.
- OX vs. ER trade-off: Both ended up with virtually the same final tour
  quality (no statistically significant difference between them), but they got
  there differently. ER preserved nearly 100% of parent edges and found good
  tours very quickly (median 52.5 generations), but lost population diversity
  fast. OX preserved slightly fewer edges, which kept more variety in the
  population and avoided getting stuck early.
================================================================================