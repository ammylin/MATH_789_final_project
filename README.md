# MATH 789 Final Project
## Measuring Credit Card Portfolio Risk: A Vasicek Model Approach with Monte Carlo Simulation

**Authors:** Jordan Andrew, Ammy Lin, Farnoosh Memari, Tonantzin Real Rojas  
**Institution:** Duke University
**Course:** MATH 789: Fundamentals of Finance Business Models  
**Instructor:** Professor David Ye

## Final Deliverables
For a complete detailed analysis and the summary presentation, please refer to the files included in this repository:
* [**Full Report (paper.pdf)**](paper.pdf) - Contains the complete methodology, literature review, and detailed results.
* [**Project Presentation (presentation.pdf)**](presentation.pdf) - A summary slide deck covering the overview, technical approach, and key insights.

## Abstract
This project implements a Vasicek-model Monte Carlo simulation to quantify credit card portfolio risk, measuring Value-at-Risk (VaR) and Conditional Value-at-Risk (CVaR) under varying default probabilities (3%-22.15%) and correlations (5%-15%). Unlike traditional approaches that assume independent defaults, our framework captures default dependency, which is critical for realistic stress testing.

The analysis demonstrates that correlation structures dramatically impact tail risk, with **VaR increasing up to 75%** and **CVaR increasing up to 93%** when correlation rises from 5% to 15% at high default probabilities. The study illustrates how systemic factors amplify losses in retail credit portfolios under stress conditions.

## Methodology

### The Vasicek One-Factor Model
We utilized the Vasicek framework to model correlated defaults. This model assumes that the asset value of a borrower is driven by a common **systematic factor** (economy-wide) and an **idiosyncratic factor** (borrower-specific).

The asset value $X_{i}$ for borrower $i$ in simulation $m$ is defined as:

$$X_{i,m} = \sqrt{\rho} \cdot Z_{m} + \sqrt{1-\rho} \cdot \epsilon_{i,m}$$

Where:
* $Z_{m} \sim N(0,1)$ is the systematic risk factor affecting all borrowers.
* $\epsilon_{i,m} \sim N(0,1)$ is the idiosyncratic risk factor unique to borrower $i$.
* $\rho$ is the asset correlation parameter.

### Default Condition
A default occurs when the borrower's creditworthiness falls below a specific threshold $\tau$, derived from the Probability of Default (PD):

$$D_{i,m} = 1\{X_{i,m} < \Phi^{-1}(PD)\}$$

### Portfolio Loss Calculation
The total portfolio loss for a given simulation is the sum of credit exposures ($C_i$) for all defaulting clients:

$$L_{m} = \sum_{i=1}^{N} D_{i,m} \cdot C_{i}$$

We performed **$M=10,000$ iterations** for a portfolio of **$N=30,000$ clients**.

## Data Source
The analysis utilizes the "Credit Card Clients" dataset from Taiwan, covering the period from April 2005 to September 2005. 
* **Sample Size:** 30,000 clients.
* **Key Insight:** Default probabilities in the dataset ranged from 3% to 22.15%.
* **Demographics:** The portfolio is predominantly female and composed of single, well-educated individuals (University/Graduate school level).

## Key Results

### 1. Correlation vs. PD
Our simulations revealed that while the Probability of Default (PD) drives the *Expected Loss*, the correlation parameter $\rho$ governs the *Tail Risk* (extreme losses).

### 2. Tail Risk Amplification
When default correlation increases, the "fatness" of the tail increases, leading to "explosive" growth in VaR and CVaR.



**Comparative VaR (99% Confidence Level):**

| Correlation | PD | VaR (99%) | CVaR (99%) |
| :--- | :--- | :--- | :--- |
| **5%** | 3.00% | NT$ 414,593,000 | NT$ 484,376,138 |
| **15%** | 3.00% | NT$ 746,879,600 | NT$ 948,810,032 |
| **15%** | 22.15% | **NT$ 2,802,354,880** | **NT$ 3,092,803,048** |

### 3. Expected vs. Maximum Loss
In scenarios with low PD but high correlation, the Maximum Loss observed in simulations exceeded the Expected Loss by over **1000%**. This highlights the danger of relying solely on expected loss metrics for capital reservation.



## Usage

To reproduce the Monte Carlo simulations:

1.  **Install dependencies:**
    ```bash
    pip install pandas numpy matplotlib scipy
    ```

2.  **Run the analysis:**
    * Launch Jupyter Notebook:
        ```bash
        jupyter notebook
        ```
    * Open `final_sims.ipynb` and execute all cells.

## References
1.  Basel Committee on Banking Supervision (2006). *International convergence of capital measurement and capital standards*.
2.  Vasicek, O. (2002). *Loan portfolio value*. Risk, 15(12).
3.  Glasserman, P. (2003). *Monte Carlo Methods in Financial Engineering*.

## Acknowledgments
This project was developed as part of the graduate curriculum for MATH 789 at Duke University. Special thanks to Professor David Ye for guidance on the application of the Vasicek model to retail credit portfolios.
