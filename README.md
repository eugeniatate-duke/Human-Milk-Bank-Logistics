# Routing for Social Good: Milk Bank Collection Planning

This project builds a one-week milk collection plan for the Oklahoma Mothers' Milk Bank (OMMB), a nonprofit that gathers frozen donor milk from 36 depots across Oklahoma, Kansas, Arkansas, and Missouri for medically fragile infants. With one driver and one truck limited to 12 hours of driving per day, each depot must be served exactly once during a Monday–Friday week, either by a truck pickup or by a direct shipment, at the lowest cost in driver labor, fuel, and shipping. We solve it two ways using the same rules and cost inputs: a simple, solver-free cluster-first, route-second algorithm in Python, and a mixed-integer optimization model in Pyomo that chooses which depots ship and the stop order of each route. We compare the two plans on cost, driving time, and operational feasibility, and note the limits of a one-week analysis.

Built for AIPI 530 (Optimization), Fall 2026, by Eugenia Tate.

## Results

| | Solution 1: algorithm | Solution 2: Pyomo model |
|---|---|---|
| Weekly cost | $846.50 | $667.28 |
| Routes used | 5 | 4 |
| Driving time | 54.53 h | 43.41 h |
| Depots shipped | 0 | 0 |

Solution 2 is feasible but not proven optimal: HiGHS stopped at its 30-minute time limit with a best bound of $522.86 (gap 21.6%).

## Repository contents

- `PartA/` : customer document, algorithm document, and the Part A notebook (simple algorithm)
- `PartB/` : customer document, mathematical formulation, and the Part B notebook (Pyomo model)
- `Slides_Case_Study_1.pdf` : slide deck covering both solutions
- `requirements.txt` : Python packages used
- `data/` : input Excel files (not included in the public repo)

## How to run

1. Install Python 3.10 or newer.
2. Install the packages: `pip install -r requirements.txt`
3. Put the two input files in a `data/` folder next to the notebooks, keeping their original names:
   - `Depot Information.xlsx`
   - `Optional - Driving Distance and Time.xlsx`
4. Open the notebooks in Jupyter or VS Code and choose Run All.
   - **Part A** (simple algorithm): runs in a few seconds and prints the K comparison, the route for each day, the total cost, and a final check that all operating rules are satisfied.
   - **Part B** (Pyomo model): uses the HiGHS solver (installed with the `highspy` package). The solve is limited by `TIME_LIMIT` in the solve cell. It is set to 1800 seconds (30 minutes). Set it to 300 for a quick test run. The next cell prints the solver status, best cost, lower bound, gap, and the rule check.

## Notes and limitations

- Both solutions use the same rules and costs: at most 5 routes, each within 720 minutes, labor $7.25/hour, fuel $2.70/gallon at 21 mpg, shipping $1.00/mile.
- The model covers one week. It ignores donation volumes, freezer capacity, and milk aging, so it cannot show that depots will not overflow. A multi-week model with donation forecasts would be needed for that.
