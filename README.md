# Option Pricing: Three Methods Compared

Pricing a European call option via closed-form Black-Scholes, Monte Carlo 
simulation, and finite-difference solution of the Black-Scholes PDE — 
comparing accuracy, convergence behavior, and trade-offs between the methods.

## Overview
Implements and validates three independent methods for pricing a 
European call option, using their agreement to confirm correctness and 
examining how each converges to the exact answer

## Methods
- **Black-Scholes**: closed-form analytical solution, used as ground truth
- **Monte Carlo**: simulates terminal stock prices via geometric Brownian 
  motion, averages discounted payoffs
- **Finite differences**: solves the Black-Scholes PDE numerically via 
  Crank-Nicolson, using a banded solver for efficiency

## Results
- All three methods agree closely for standard inputs as shown in this plot for varying strike price with - stock price: 100, Time to expiry: 10 years, risk free rate: 5%, volatility: 20% 
![alt text](plots/Method_Comparison.png)

- Monte Carlo convergence: error shrinks as N increases
- Finite-difference convergence: error shrinks as the grid is refined, 
  with one notable exception discussed below

## A notable finding: strike/grid alignment
Convergence was non-monotonic when the strike price didn't land exactly on a grid node — a known artifact of applying finite differences to a payoff with a kink at S=K, since finite-difference methods assume local smoothness. In this test, since S=K, grid misalignment affected both the representation of the kink and the precision of reading off the final price at S — a coarser grid made both effects worse, so decreasing grid spacing didn't always improve accuracy unless ds was chosen to keep K (and S) aligned to a grid node.

## What I'd improve with more time
- Extend to American options (early exercise) via the finite-difference method
- Price path-dependent options (Asian, barrier) via Monte Carlo, where no 
  closed-form solution exists
- Variance reduction techniques (antithetic variates) to speed up Monte 
  Carlo convergence
- Non-dimensionalize the PDE to reduce it to the heat equation

