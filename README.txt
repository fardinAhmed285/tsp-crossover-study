================================================================================
The Travelling Salesman Problem
================================================================================

Authors:     Fardin Ahmed, Sparsh Dubey

--------------------------------------------------------------------------------
1. FOLDER & FILE LAYOUT
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
`-- figures/                            
    |-- fig2_ulysses22_performance      # Boxplots & convergence curves
    |-- fig3_mechanism_dynamics 	# Edge inheritance & diversity over time
    |-- fig3_edge_diversity         
    `-- fig4_ulysses22_tours            # Map plots of the best tours found
	
--------------------------------------------------------------------------------
2. SETUP & REQUIRED LIBRARIES
--------------------------------------------------------------------------------
Below are the exact package versions used:

- Python 3.12
- numpy 1.26.4
- scipy 1.13.1
- pandas 2.2.2
- matplotlib 3.9.2
- deap 1.4.1
- tsplib95 0.7.1
- jupyter 

You can install everything in one command:
    pip install numpy==1.26.4 scipy==1.13.1 pandas==2.2.2 matplotlib==3.9.2 deap==1.4.1 tsplib95==0.7.1 jupyter

Make sure the `data/ulysses22.tsp` file is inside the `data/` folder before
running the notebook.

--------------------------------------------------------------------------------
3. HOW TO RUN THE CODE (STEP-BY-STEP)
--------------------------------------------------------------------------------
 a) Open the terminal in the main project folder and launch Jupyter:
       jupyter notebook tsp_study.ipynb

 b) Note on Saved CSV Files (Caching):
    The notebook automatically checks if the result CSV files already exist in
    the `results/` folder:
   - If the CSV files are present, it skips re-running the GA simulations and
     immediately generates the statistical tables and PDF figures in a few
     seconds.
   - If you want to run all 120 evolutionary simulations from scratch, simply
     delete or move the CSV files inside `results/` before running the cells.

 c) Run all cells in order from top to bottom ("Run All")
 
