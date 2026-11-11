# Volatility Smile e calibrazione SVI

Estrazione della volatilità implicita da prezzi di opzioni europee e calibrazione del modello **SVI (Stochastic Volatility Inspired)** di Jim Gatheral, con controlli di assenza di arbitraggio.

## Contenuto — [`svi_smile.ipynb`](svi_smile.ipynb)

* Prezzo **Black-Scholes** delle call europee e inversione numerica con il metodo di **Brent** (`scipy.optimize.brentq`) per ottenere la **volatilità implicita**, scartando i prezzi fuori dai limiti di non arbitraggio.
* Calibrazione della parametrizzazione **raw SVI** della varianza totale
  $w(k) = a + b\left(\rho\,(k-m) + \sqrt{(k-m)^2 + \sigma^2}\right)$
  con minimi quadrati vincolati (`SLSQP`):
  * varianza totale minima non negativa $a + b\,\sigma\sqrt{1-\rho^2} \ge 0$;
  * pendenza delle ali $b(1+|\rho|) \le 2$ (formula dei momenti di Roger Lee).
* Verifica a posteriori dell'assenza di **arbitraggio a farfalla** con la condizione di Durrleman $g(k) \ge 0$.

## Risultati

Su 25 strike sintetici con rumore sui prezzi, il fit ha un errore quadratico medio di 0.0097 rispetto alle volatilità di mercato e di 0.0073 rispetto alla volatilità vera usata per generarle; il minimo di $g(k)$ su $k \in [-1.5, 1.5]$ è 0.26, quindi lo smile calibrato è privo di arbitraggio a farfalla.

## ▶️ Esecuzione

Serve Python 3.9 o successivo.

```bash
git clone https://github.com/lancellottimatteo19/volatility-smile-svi.git
cd volatility-smile-svi
pip install -r requirements.txt
jupyter lab
```

I notebook sono salvati con gli output, quindi grafici e risultati si vedono anche direttamente su GitHub.
