# Volatility Smile and SVI Calibration

Implied volatility extracted from European option prices, and a calibration of Jim Gatheral's **SVI (Stochastic
Volatility Inspired)** model with no-arbitrage constraints.

## Contents — [`svi_smile.ipynb`](svi_smile.ipynb)

* **Black-Scholes** pricing of European calls and numerical inversion with **Brent's method**
  (`scipy.optimize.brentq`) to obtain the **implied volatility**, discarding prices outside the no-arbitrage
  bounds.
* Calibration of the **raw SVI** parameterisation of total implied variance
  $w(k) = a + b\left(\rho (k-m) + \sqrt{(k-m)^2 + \sigma^2}\right)$
  by constrained least squares (`SLSQP`):
  * non-negative minimum total variance, $a + b \sigma\sqrt{1-\rho^2} \ge 0$;
  * wing slopes $b(1+|\rho|) \le 2$ (Roger Lee's moment formula).
* An ex-post check for **butterfly arbitrage** with Durrleman's condition $g(k) \ge 0$.

## Results

On 25 synthetic strikes with noisy prices, the fit has a root-mean-square error of 0.0097 against the market
implied volatilities and 0.0073 against the true volatility used to generate them. The minimum of $g(k)$ over
$k \in [-1.5, 1.5]$ is 0.26, so the calibrated smile is free of butterfly arbitrage.

## How to run

Requires Python 3.9 or later.

```bash
git clone https://github.com/lancellottimatteo19/volatility-smile-svi.git
cd volatility-smile-svi
pip install -r requirements.txt
jupyter lab
```

The notebook is saved with its outputs, so charts and results are visible directly on GitHub.
