# PeriodicGARCH.jl

Periodic GARCH: volatility with a calendar dummy.

Bollerslev and Ghysels's periodic GARCH(1,1), with a calendar dummy in the mean and the variance equation, fitted by quasi-maximum likelihood with robust standard errors, and the statistics of their Table 2.

Article: Bollerslev, T. and Ghysels, E. (1996). Periodic autoregressive conditional heteroscedasticity. Journal of Business & Economic Statistics 14(2), 139-151. [doi:10.1080/07350015.1996.10524640](https://doi.org/10.1080/07350015.1996.10524640)

## Usage

```julia
using PeriodicGARCH, CSV, Tables
bg96 = Tables.columntable(CSV.File(download("https://raw.githubusercontent.com/Trumpingtons/PollisDatasets.jl/main/data/bg96.csv")))
y, s = Float64.(bg96.return), Float64.(bg96.nontrading)  # daily returns and the non-trading-day dummy
m = pgarch(y, s; free=(:mu, :omega, :omega1, :alpha, :alpha1, :beta), start=[0.0, 0.3, 0.05, 0.15, -0.1, 0.8])
m.θ, m.se  # estimates and robust standard errors, in the order of PARAMETERS
sqrt.(m.h)  # the conditional volatility
```

## Vignette

[Periodic GARCH: volatility with a calendar dummy](vignette/PeriodicGARCH.pdf)

## Tests

The tests compute the published tables below from their data and check every printed value, rounded to the digits printed:

- Persistence of a shock, p. 146 (`test/data/targets/persistence.csv`)
- Table 2, columns 5 and 6, p. 146 (`test/data/targets/table2.csv`)

The data, downloaded from its links when the tests run or kept in `test/data/inputs`:

- `bg96`: <https://raw.githubusercontent.com/Trumpingtons/PollisDatasets.jl/main/data/bg96.csv>, PollisDatasets: the 1,974 daily Deutsche mark / British pound returns of 1984 to 1991, with the paper's non-trading-day dummy

Run them with `using Pkg; Pkg.test("PeriodicGARCH")`.

Made with [Pollis](https://pollis.org).
