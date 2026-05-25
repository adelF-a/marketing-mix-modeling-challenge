# Marketing Mix Modeling (MMM) Case Study

## Project Overview
This repository contains a data-driven evaluation of a 104-week marketing dataset spanning from 2020 to 2022. The objective of this analysis is to evaluate the performance of seven distinct marketing spend channels and identify their impact on overall company revenue. 

The primary analytical focus is on identifying performance drivers, understanding time-lagged marketing effects (Adstock), and isolating specific high-growth periods to optimize budget allocation.

## Key Methodology & Findings
* **Data Chronology:** Rectified date string formatting issues using European date parsing standards and aligned the dataset chronologically to establish a reliable baseline.
* **Momentum Analysis:** Isolated the top four major positive revenue jumps alongside their preceding four-week operational windows to track lag effects and marketing build-ups.
* **The Holiday Effect:** Identified a notable consumer behavior trend during late November 2020, where a significant drop in revenue was immediately followed by a record-breaking spike, aligning with seasonal Black Friday purchase patterns.

## Technical Stack & Environment
* **Environment Name:** `pymc_env`
* **Language:** Python (configured via virtual environment)
* **Core Libraries:** Pandas, NumPy, Matplotlib, Seaborn
* **Bayesian Framework:** PyMC (v3.14.4)