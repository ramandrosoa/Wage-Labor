Wage Labour and Capital
================

### 1. Introduction

Reading Marx’s Wage Labour and Capital, I was intrigued by the
worker-capitalist relationship — specifically the tension between wages
determined by the law of supply and demand yet bounded below by the cost
of subsistence. Marx claimed that wages tend toward subsistence over
time, that labor surplus grows as capital accumulates, and that
structural crisis becomes inevitable past a certain threshold. Yet
despite the theoretical richness of these mechanisms, Marx reasoned
entirely in words and logical argument, never formalizing his claims as
quantitative, testable hypotheses — leaving open a fundamental question:
do these mechanisms, when expressed mathematically, actually produce the
dynamics he described?

In this project, the causal mechanisms of Wage Labour and Capital are
translated into a mathematical data generating process grounded in the
cliodynamics literature. To ensure that variables behave according to
theoretical mechanisms rather than being confounded by the complexity of
real historical processes, we rely on synthetic rather than empirical
historical data — allowing precise control over the data generating
process. The synthetic data is designed to replicate one fundamental
challenge of historical time series: the temporal dependence between
observations. A second challenge — the fragmentary and incomplete nature
of real historical archives — is acknowledged as a limitation and avenue
for future work.

The simulation is structured around two independent variables — wage gap
and labor surplus — whose dynamics are governed by theoretically
motivated structural equations developed in the following sections.
These variables feed into cumulative pressure terms ($CP_L$, $CP_W$),
which capture the accumulated effect of structural pressures over time
and predict the probability of structural crisis through a logistic
regression model with an interaction term formalizing Marx’s conjunction
hypothesis.

We observed N societies over T decades. To determine the sample size, we
used the **10 events per variable (EPV)** rule of thumb for logistic
regression. But two methodological challenges must be addressed. First,
the determination of N and T depends on the crisis probability and the
autocorrelation structure of the cumulative pressure variables. Second,
the cumulative pressure terms exhibit an **ARMA (1,q)** autocorrelation
structure, making analytical derivation of the **effective sample size
(ESS)** inappropriate. For these reasons, we proceed in two stages: a
pilot simulation with provisional parameters to empirically estimate the
autocorrelation structure, followed by the full simulation with final N
and T derived from both the empirical autocorrelation estimate and the
sensitivity analysis on crisis probability. Following the final
simulation, a sensitivity analysis on the structural parameters is
conducted to assess whether the core findings remain stable across
alternative parameter specifications.

Finally, we compare **frequentist and Bayesian approaches to parameter
recovery**, testing whether Marx’s structural parameters — including the
interaction term between wage depression and labor surplus — are
statistically detectable from synthetic historical data.

### 2. Theoretical Framework

#### The Directed Acyclic Graph (DAG)

``` r
knitr::include_graphics("wage_lab_files/figure-gfm/dag.png")
```

<img src="wage_lab_files/figure-gfm/dag.png" width="100%" />

**Capital accumulation** is the extra value created by workers and
reinvested to grow power and profit. Thus, it expands the **labor
surplus**, as workers must work more to generate higher profits for the
capitalist. At the same time, the **wage gap** widens: while the real
wage remains stagnant, the relative wage decreases because the cost of
subsistence tends to increase over time.

Since all past periods contribute to the current crisis, yet recent
events exert a disproportionate influence, we implemented a **cumulative
pressure structure** for both labor surplus and the wage gap. By
applying an exponential decay function, the model retains the influence
of all past periods while assigning greater weight to more recent
dynamics.

#### The Variables :

1.  The dependent variable : structural crisis

$P(crisis[t]) = logistic(z)$, with :

$$
z = \beta_0+\beta_1x_1+\beta_2x_2+\beta_3x_1x_2
$$

- $x_1$: cumulative wage gap

- $x_2$: cumulative labor surplus

- $x_1x_2$ : mutual reinforcement between wage gap and labor surplus

The structural crisis outcome $Y_t$ \~ $Bernoulli(pt)$ represents the
binary realization of accumulated structural pressure — does a
structural crisis occur in this decade or not? The probability
$P(crisis[t])$ is governed by a logistic function of the cumulative
pressure terms $CP_L$ and $CP_W$. This formulation captures the
threshold nature of structural crisis — pressure accumulates gradually
through wage depression and labor surplus until it crosses a critical
level, at which point crisis probability rises sharply. Marx explicitly
describes this intensification in Wage Labour and Capital: ‘in the same
measure in which the capitalists are compelled, by the movement
described above, to exploit the already existing gigantic means of
production on an ever-increasing scale, and for this purpose to set in
motion all the mainsprings of credit, in the same measure do they
increase the industrial earthquakes, in the midst of which the
commercial world can preserve itself only by sacrificing a portion of
its wealth, its products, and even its forces of production, to the gods
of the lower world — in short, the crises increase. They become more
frequent and more violent’ (Marx, 1849).

**The interaction term** : ($\beta_3x_1x_2$)

The interaction term $\beta_3x_1x_2$ formalizes the mutual reinforcement
between wage depression and labor surplus that Marx explicitly describes
in Wage Labour and Capital. In his summary of capital’s contradictions,
Marx traces how productive growth simultaneously extends the division of
labor, intensifies competition among workers, and depresses wages —
while also swelling the reserve army through the proletarianization of
small manufacturers: ‘the forest of outstretched arms, begging for work,
grows ever thicker, while the arms themselves grow ever leaner’ (Marx,
1849). This simultaneous thickening of labor surplus and shrinking of
wages is precisely what the interaction term captures — neither force
operates independently, but each amplifies the other. We explicitly
assume a multiplicative effect between the cumulative pressure terms,
such that the joint presence of wage depression and labor surplus
generates a disproportionate increase in crisis probability beyond their
additive contributions. While Marx does not explicitly distinguish
additive from multiplicative effects, the multiplicative assumption
reflects the theoretical logic of mutual reinforcement described above —
a hypothesis tested directly through the significance and magnitude of
$\beta_3$ in the parameter recovery analysis.

2.  The independent variables :

    - Labour surplus

$$
LaborSurplus(t) = \frac{K_l}{1+e^{-r_l(t-t_{0,l})}} + A e^{{-\delta_l}t}\cdot sin\frac{2t\pi}{P} + \epsilon
$$

- $\frac{K_l}{1+e^{-r_l(t-t_{0,l})}}$ : This term represents the
  logistic trend of labor surplus — the secular accumulation of the
  reserve army of labor across multiple boom-bust cycles. Its upward
  trajectory is driven by two compounding forces. First, each recovery
  absorbs fewer workers than the previous crisis displaced, leaving a
  residual surplus that accumulates over time. Second, technology
  progressively replaces labor — each new sector requires fewer workers
  than the one it displaced, meaning the trend never returns to its
  previous low. As Marx argues in Wage Labour and Capital, competition
  among capitalists drives the continuous introduction of machinery and
  greater division of labor, permanently swelling the reserve army: “the
  larger the army of workers among whom the labour is subdivided, the
  more gigantic the scale upon which machinery is introduced” (Marx,
  1849). The logistic shape reflects the self-reinforcing nature of this
  accumulation — slow growth in early capitalism accelerates as
  mechanization intensifies, before stabilizing as the system approaches
  its structural limits. The ceiling $K_l$ represents the maximum
  sustainable labor surplus — the point beyond which the system faces
  structural breakdown

- $A e^{{-\delta_l}t}\cdot sin\frac{2t\pi}{P} + \epsilon$ : This term
  captures the cyclical dynamics of labor surplus around the logistic
  trend. The sinusoidal component reflects the anarchic movement of
  capital described by Marx in Wage Labour and Capital: capital
  perpetually emigrates from sectors where prices fall below cost of
  production and immigrates into more profitable ones — “the high price
  produces an excessive immigration, and the low price an excessive
  emigration” (Marx, 1849). When $sin\frac{2t\pi}{P}$ \> 0, capital
  flight displaces workers, pushing labor surplus above the trend
  (Overproduction Phase). When $sin\frac{2t\pi}{P}$ \< 0, capital
  immigration into new sectors temporarily absorbs workers, pulling
  labor surplus below the trend (Recovery Phase). The exponential term
  $A e^{{-\delta_l}t}$ introduces amplitude modulation over time. No two
  cycles are identical, reflecting the unpredictable timing and
  magnitude of capital reallocation across sectors — consistent with
  Marx’s characterization of capital movement as anarchic rather than
  regular. The noise term $\epsilon$ \~ $N(0, \sigma^2)$ captures
  additional stochastic variation around the deterministic cycle
  structure.

  - Wage gap

$$
WageGap(t) = \frac{K_w}{1+e^{-r_w(t-t_{0,w})}}
$$

The wage gap measures the distance between the nominal wage and the cost
of subsistence — the minimum expenditure required to reproduce the
worker’s labor power. As Marx illustrates in Wage Labour and Capital,
wages can fall in real terms even when nominal wages remain unchanged,
simply because the cost of subsistence rises: “the same money they
received in exchange less bread, meat, etc. Their wages fell, not
because the value of silver was less, but because the value of the means
of subsistence had increased” (Marx, 1849). This distinction between
nominal and real wages motivates modeling the wage gap directly — rather
than wages alone — as the relevant variable for structural crisis
prediction. The wage gap follows a logistic growth trajectory,
reflecting the gradual but self-reinforcing nature of wage depression
under capitalism. In the early phase — before the inflection point
$t_{0,w}$ — the gap remains small, as wages still cover subsistence
costs. Past the inflection point, the gap widens rapidly as labor
surplus accumulates and competitive pressure intensifies. The ceiling
$K_w$ represents the maximum sustainable wage gap — the point at which
wages can no longer cover subsistence costs and structural crisis
becomes inevitable.

$$
r_w = r_{base,w} + \alpha(LaborSurplus_{t-1})
$$

The growth rate $r_w$ is modeled as a function of lagged labor surplus,
reflecting Turchin and Nefedov’s (2009) empirical observation that
oversupply of labor leads to depressed wages — a mechanism Marx
identifies as the primary driver of wage depression in Wage Labour and
Capital.

### 3. Data Generating Process

#### 3.1 Pilot simulation

In the absence of precise empirical estimates for the structural
parameters, provisional values are assigned based on theoretical
plausibility, ensuring that the simulated dynamics are qualitatively
consistent with the mechanisms described in Marx’s Wage Labour and
Capital. Specifically, parameters are chosen so that labor surplus
follows a gradual logistic accumulation with dampened cyclical
fluctuations, wage gap exhibits a feedback driven S-shaped trajectory,
and cumulative pressures build meaningfully over time before the crisis
threshold is reached. While these provisional values cannot be formally
validated against historical data at this stage, a sensitivity analysis
is conducted after the final simulation to assess whether the core
findings, namely the reliable recovery of the logistic regression
parameters and the theoretically expected ordering $\beta_3$ \>
$\beta_1$ \> $\beta_2$, remain stable across alternative parameter
specifications. Robustness of the results across this range would
suggest that the conclusions are driven by the theoretical structure of
the model rather than by any specific parameter choice.

``` r
set.seed(123)
```

``` r
N_pilot <- 10
T_pilot <- 200
```

``` r
# labor_surplus parameters
K_l <- 1
r_l <- .05
t0_l  <- 50
A <- .2
P <- 5
delta_l <- 0.43

# wage_gap parameters
K_w <- 1
r_base <- .03
alpha <- .5
t0_w <- 60

# decay parameter (cumulative pressure)
delta_c <- 0.43
```

**Generate wage gap and labor surplus**

``` r
func_labor_surplus <- function(N, T, r_l, t0_l, A, P) {
  
  labor_surplus <- matrix(NA, nrow = T , ncol = N)
  for (n in 1:N) {
    for (t in 1:T) {
    
      logistic_trend <- K_l/(1+exp(-r_l*(t-t0_l)))
      oscillation <- A * exp(-delta_l*t) * sin((2*t*pi)/P)
      epsilon <- rnorm(1, mean = 0, sd = .02)
    
      labor_surplus[t, n] <- logistic_trend + oscillation + epsilon
    }
  }
  return(labor_surplus)
}
```

``` r
labor_surplus <- func_labor_surplus(N_pilot, T_pilot, r_l, t0_l, A, P )
```

``` r
func_wage_gap <- function(N, T, lab, r_base, alpha, t0_w,
                          sd_t0 = 5, sd_r = 1) {
  wage_gap <- matrix(NA, nrow = T, ncol = N)
  for (n in 1:N) {
    # one draw per society -> persistent, smooth differences
    t0_n <- t0_w + rnorm(1, 0, sd_t0)
    r_n  <- r_base * exp(rnorm(1, 0, sd_r))   # log-normal keeps r > 0
    for (t in 1:T) {
      r_w <- if (t == 1) r_n else r_n + alpha * lab[t - 1, n]
      wage_gap[t, n] <- K_w / (1 + exp(-r_w * (t - t0_n)))
    }
  }
  wage_gap
}
```

``` r
wage_gap <- func_wage_gap(N_pilot, T_pilot, labor_surplus, r_base, alpha, t0_w,
                          sd_t0 = 5, sd_r = 1)
```

**Generate cumulative wage gap and cumulative labor surplus**

Weights are assigned inversely to chronological distance, prioritizing
recent events over earlier ones via an exponential decay factor.

      -   Cumulative labour surplus

$$
CP\_L(t) = \sum^{t-1}_{k} L_{t-k}\cdot e^{-\delta_c\cdot k}
$$

      -   Cumulative wage gap

$$
CP\_W(t) = \sum^{t-1}_{k} W_{t-k}\cdot e^{-\delta_c\cdot k}
$$

``` r
cp <- function(N, T, lab, wag) {
  
  CP_l <- matrix(NA, nrow = T , ncol = N)
  CP_w <- matrix(NA, nrow = T , ncol = N)

  for (n in 1:N){
    for (t in 1:T){
    
    # cumulative pressure is only meaningful at t=2
      if (t==1){
        CP_l[t, n] <- 0
        CP_w[t, n] <- 0
      }
      else {
      
        cpsum_l <- 0
        cpsum_w <- 0
      
        for (k in 1:(t-1)){
          cpsum_l <- cpsum_l + lab[t-k, n] * exp(-delta_c*k)
          cpsum_w <- cpsum_w + wag[t-k, n] * exp(-delta_c*k)
      }
      
        CP_l[t, n] <- cpsum_l
        CP_w[t, n] <- cpsum_w
      }
    }
  }
  return( list(CP_l = CP_l, CP_w = CP_w) )
}
```

``` r
result <- cp(N_pilot, T_pilot, labor_surplus, wage_gap)
CP_w <- result$CP_w
CP_l <- result$CP_l
```

##### Estimation of the Autocorrelation Structure

The cumulative pressure series $CP_l$ and $CP_w$ are stochastic. Since
each society draws its own sequence of random shocks $\epsilon$ and
oscillation realizations, the autocorrelation function estimated from a
single society reflects idiosyncratic noise rather than the underlying
process structure shared by all societies. To obtain a more stable and
representative estimate of the autocorrelation structure, we average
across N societies. The estimation is further restricted to the
post-inflection phase, starting from $t_{0,l}$ and $t_{0,w}$. In the
pre-inflection phase, the logistic trend remains close to zero and the
cumulative pressures have not yet fully developed, which would bias the
autocorrelation estimate downward. The post-inflection phase, by
contrast, captures the mature dynamics of the system where structural
pressures have fully accumulated and the ARMA autocorrelation structure
is most clearly expressed empirically.

``` r
lag_max <- 20 
acf_matrix_l <- matrix(NA, nrow = lag_max , ncol = N_pilot)
acf_matrix_w <- matrix(NA, nrow = lag_max , ncol = N_pilot)
```

``` r
acf_func <- function(lag_max, acf_matrix, N, T, t0, CP) {
  for (s in 1:N){
    cps <- CP[t0:T, s]
    acf_matrix[,s] <- acf(cps, 
                          lag.max = lag_max,
                          plot = FALSE)$acf[-1]
  }
  acf_avg <- rowMeans(acf_matrix)
  return(acf_avg)
}
```

``` r
acf_avg_l <- acf_func(lag_max, acf_matrix_l, N_pilot, T_pilot, t0_l, CP_l)
acf_avg_w <- acf_func(lag_max, acf_matrix_w, N_pilot, T_pilot, t0_w, CP_w)
```

``` r
acf_plot <- function(acf, T, title = "Average ACF"){
  
   plot(1:lag_max, acf,
         type = "h",
         xlab = "Lag",
         ylab = "Average ACF",
         main = title,
         ylim = c(-0.2, 1))
    abline(h = 0)
          # Significance bounds
    abline(h =  1.96 / sqrt(T), lty = 2, col = "#E24B4A")
    abline(h = -1.96 / sqrt(T), lty = 2, col = "#E24B4A")
        
    legend("topright",
            legend = c("Average ACF", "95% significance bounds"),
            col    = c("black", "#E24B4A"),
            lty    = c(1, 2))
  
}
```

``` r
par(mfrow = c(1,2))
acf_plot(acf_avg_l, T_pilot, title = "ACF labor surplus")
acf_plot(acf_avg_w, T_pilot, title = "ACF wage gap" )
```

![](wage_lab_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
par(mfrow = c(1,1))
```

Both variables exhibit **high autocorrelation as a consequence of their
ARMA(1,q) structure**. Yet, unlike the wage gap, which follows a
smoother trajectory, the labor surplus features dampened oscillations
that cause $CP_L$ to retain more autocorrelation than $CP\_W$.

##### Derive T from the effective sample size (ESS)

In time series data, observations are not independent — each period
carries information from previous ones. The effective sample size
$T_{eff}$ allows us to measure the **true amount of independent
information in the raw sample T when observations are correlated.**

$$
T_{eff} = \frac{T}{1+2\sum^{\infty}_{k=1}\rho(k)}
$$

- $T$ : T_pilot, the raw sample size, the total number of observed
  periods.
- $T_{eff}$ : the ESS — the equivalent number of independent
  observations after accounting for autocorrelation
- $\rho$ : the autocorrelation structure.

``` r
# Labor surplus
Tl_obs <- T_pilot - round(t0_l)
Tl_eff <- Tl_obs/(1+2*sum(acf_avg_l))

# Wage gap
Tw_obs <- T_pilot - round(t0_w)
Tw_eff <- Tw_obs/(1+2*sum(acf_avg_w))

ratio_w <- Tw_eff/Tw_obs
ratio_l <- Tl_eff/Tl_obs

cat("Effective sample size — labor surplus:",
    round(Tl_eff, 3), "\n")
```

    ## Effective sample size — labor surplus: 5.27

``` r
cat("Effective sample size — wage gap:",
    round(Tw_eff, 3), "\n")
```

    ## Effective sample size — wage gap: 16.198

``` r
cat("Efficiency ratio — wage gap:", 
    round(ratio_w, 3), "\n")
```

    ## Efficiency ratio — wage gap: 0.116

``` r
cat("Efficiency ratio — labor surplus:", 
    round(ratio_l, 3), "\n")
```

    ## Efficiency ratio — labor surplus: 0.035

``` r
cat("Usable decades — labor surplus:", Tl_obs, "\n")
```

    ## Usable decades — labor surplus: 150

``` r
cat("Usable decades — wage gap:", Tw_obs)
```

    ## Usable decades — wage gap: 140

The effective sample size represents the number of independent,
non-redundant and unique observations contained in a correlated time
series. $Tl_{eff}$ \< $Tw_{eff}$ is theorically expected, as the
oscillatory component introduces cyclical dependence that reduces the
effective information content of $CP\_L$ relative to $CP\_W$.

The usable decades represent the number of observed periods required to
accumulate a given number of truly independent observations. Due to the
high autocorrelation structure of each variable, the raw sample is
substantially larger than the effective one. For instance, each society
must be observed for roughly **150 decades to yield only 5 independent
observations from the labor surplus**. Similarly, roughly **140 decades
of observation per society are needed to accumulate 15 independent
observations from the wage gap.**

##### Derive N from the events per variable (EPV)

Following the **Events Per Variable rule**, which recommends a minimum
of 10 observed events per predictor to ensure reliable parameter
estimation in logistic regression, and given that our model contains 3
variables ($x_1$, $x_2$, $x_1x_2$), the minimum number of **expected
crisis events is 3×10=30**

$$
EPV = N\cdot T_{eff}\cdot p
$$

- $EPV$: expected crisis events
- $N$: number of societies
- $T_{eff}$: effective sample size
- $p$: crisis probability

Every predictor requires a sufficient number of independent observations
to ensure reliable parameter estimates. By setting $T_{eff}$ based on
$CP\_W$, we guarantee at least 5 independent observations for $CP\_L$.

Recall $P(crisis[t]) = logistic(z)$, with

$$
z = \beta_0+\beta_1x_1+\beta_2x_2+\beta_3x_1x_2
$$

To estimate N, we vary the crisis probability p across a theoretically
motivated range. p represents the baseline crisis probability when
$CP\_W = CP\_L = 0$, interpreted as the **minimum crisis probability**
at the onset of capitalism before structural contradictions accumulate.
Setting $CP_W = CP_L = 0$ isolates $\beta_0$ as the sole determinant of
the baseline probability, ensuring that the sensitivity analysis varies
only the intercept while holding the pressure dynamics fixed.

$$
p (crisis|CP\_W = 0, CP\_L = 0) = \frac{1}{1+e^{-\beta_0}}
$$

A sensitivity analysis is performed on p, deriving the corresponding
$\beta_0$ for each value through. It is performed across the range p
$\in$ {0.001, 0.005, 0.01, 0.02, 0.05}, reflecting theoretically
plausible baseline rates from near-impossible to rare.

$$
\beta_0 = log(\frac{p}{1-p})
$$

``` r
p <- c(0.001, 0.005, 0.01, 0.02, 0.05)
beta0_values <- qlogis(p)
epv <- 30
T_eff_binding <- Tw_eff

# N calculation for each p
sensitivity_analysis <- data.frame(
  crisis_prob = p ,
  beta0 = round(beta0_values, 4), 
  N_needed = ceiling(epv/(T_eff_binding*p))
)

print(sensitivity_analysis)
```

    ##   crisis_prob   beta0 N_needed
    ## 1       0.001 -6.9068     1853
    ## 2       0.005 -5.2933      371
    ## 3       0.010 -4.5951      186
    ## 4       0.020 -3.8918       93
    ## 5       0.050 -2.9444       38

According to the EPV rule, a minimum of 30 crisis events is required
across the entire dataset to reliably estimate the three parameters of
the logistic model. Since the effective sample size is fixed at
$Tw_{eff}$, the number of societies N required to accumulate 30
effective crisis observations is inversely proportional to the baseline
crisis probability p. Consequently, lower values of p demand
substantially larger N to compensate the rarity of crisis events.

##### Generate the structural crisis (y_pilot)

The baseline crisis probability p is calibrated from Turchin and
Nefedov’s (2009) observation that recurrent waves of state breakdown
occured approximately three times over five centuries of European
history — the calamitous fourteenth century, the iron century of
1550-1660, and the age of revolutions of 1789-1849. This implies a
per-decade crisis probability of approximately p = 0.006. This motivates
the lower end of our sensitivity analysis, consistent with Marx’s
argument in Wage Labour and Capital that structural crisis require
prolonged accumulation of contradictions before becoming probable. The
combination that mostly fits with this argument is ($p = 0.005$,
$\beta_0 = -5.2933$, $N = 389$). Then, we will determine the parameters
$\beta_1$ , $\beta_2$ , $\beta_3$.

``` r
beta0 <- -5.2933    
beta1 <- 1.0   # wage gap effect — moderate
beta2 <- 0.5   # labor surplus effect — smaller alone
beta3 <- 2.0   # interaction — largest, captures Marx's threshold
```

The ordering $\beta_3$ \> $\beta_1$ \> $\beta_2$ reflects three
theoretical priorities. The dominance of $\beta_3$ formalizes Marx’s
conjunctural argument that **structural crisis emerges primarly from the
simultaneous occurrence of wage depression and labor surplus.** The
ordering $\beta_1$ \> $\beta_2$ reflects the relatively stronger direct
effect of wage gap compared to labor surplus in isolation, consistent
with Marx’s emphasis on wage depression as the most visible
manifestation of capitalist contradiction in Wage Labour and Capital.???

$$
z = \beta_0+\beta_1x_1+\beta_2x_2+\beta_3x_1x_2
$$

``` r
y_func <- function(beta0, beta1, beta2, beta3, CP_l, CP_w, T, N) {
  
  z <- beta0 + (beta1*CP_w) + (beta2*CP_l) + (beta3*CP_l*CP_w)
  p_crisis <- 1/(1+exp(-z))
  
  y <- matrix(rbinom(T*N, size = 1, prob = p_crisis), 
              ncol = N, nrow = T)
  
  return(y)
}
```

``` r
y_pilot <- y_func(beta0, beta1, beta2, beta3, CP_l, CP_w, T_pilot, N_pilot)
```

#### 3.2 Final simulation

For the final simulation, we set **N_final = 389 societies and T_final =
200**. This timeline spans the necessary pre- and post-inflection
decades. The choice to include exactly 150 post-inflection decades is
driven by the differing autocorrelation structures of our variables:
while these 150 decades translate to merely 5 effective independent
informations for labor surplus, they yield 15 independent observations
for the wage gap. The pre-inflection phase is retained to preserve the
theoretical warmup consistent with Marx’s early capitalism argument.
However, the labor surplus exhibits high autocorrelation, substantially
reducing the effective independent information per society. **To
compensate for this loss of independence and to satisfy the EPV rule of
a minimum of 30 effective crisis, N = 389 societies are required.**

``` r
N_final <- 389
T_final <- 200
```

``` r
# Generate labor surplus , wage gap, CP_l, CP_w, y with N_final, T_final 
labor_surplus_fin <- func_labor_surplus(N_final, T_final, r_l, t0_l, A, P )
wage_gap_fin <- func_wage_gap(N_final, T_final, labor_surplus_fin, r_base, alpha, t0_w)

cp_fin <- cp(N_final, T_final, labor_surplus_fin, wage_gap_fin)
CPfin_l <- cp_fin$CP_l
CPfin_w <- cp_fin$CP_w

y_final <- y_func(beta0, beta1, beta2, beta3, CPfin_l, CPfin_w, T_final, N_final)
```

``` r
# crisis event count comparison
data.frame (
  Simulation = c("Pilot","Final"),
  N = c(N_pilot, N_final),
  T = c(T_pilot, T_final),
  Total_crisis = c(sum(y_pilot), sum(y_final))
    )
```

    ##   Simulation   N   T Total_crisis
    ## 1      Pilot  10 200         1334
    ## 2      Final 389 200        51384

The substantial jump from 1334 to 51384 crises is an expected outcome
rather than a anomaly; it directly reflects the roughly 39-fold scaling
of the number of societies from $N_{pilot} = 10$ to $N_{final} = 389$

#### 3.3 Findings

**Structural dynamics — The variables trajectories** : a visualization
of the logistic growth of the wage gap and the dampened oscillations of
the labor surplus for a sample of 5 societies. The trajectory of each
society shows some slight differences.

``` r
trajectories_func <- function(variable, ylab = "variable", main = "variable trajectories — Sample of 5 Societies", cex.main = 0.9){
  matplot(1:T_final, variable[, 1:5],
        type = "l", lty = 1,
        col = adjustcolor(c("#378ADD","#E24B4A","#1D9E75",
                            "#D97706","#7C3AED"), alpha = 0.7),
        xlab = "Decades", ylab = ylab,
        main = main, cex.main = cex.main)
abline(v = t0_w, lty = 2, col = "gray50")
legend("bottomright", legend = paste("Society", 1:5),
       col = c("#378ADD","#E24B4A","#1D9E75","#D97706","#7C3AED"),
       lty = 1, cex = 0.7)
}
```

``` r
par(mfrow = c(2, 1), mar = c(3, 4, 2, 1))
trajectories_func(wage_gap_fin, ylab = "Wage Gap", main = "Wage gap trajectories - Sample of 5 societies")
trajectories_func(labor_surplus_fin, ylab = "Labour Surplus", main = "Labour surplus trajectories - Sample of 5 societies")
```

![](wage_lab_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

``` r
par(mfrow = c(1, 1))
```

**Structural dynamics — The cumulative pressure with crisis marker** : A
visualization of the temporal relationship between pressure and crisis.
It shows how the crises are clustered in period of high cumulative
pressure.

``` r
society_idx <- 1
crisis_decades <- which(y_final[, society_idx] == 1)

plot(1:T_final, CPfin_w[, society_idx],
     type = "l", col = "#378ADD", lwd = 1.5,
     xlab = "", ylab = "CP_W",
     main = "Cumulative Pressure and Crisis Events — Society 1")
abline(v = crisis_decades, col = "#E24B4A", lty = 2)
lines(1:T_final, CPfin_l[, 1], col = "#E24B4A", lwd = 2)
legend("topleft",
       legend = c("Cumulative Wage Gap",
                  "Cumulative Labor Surplus", 
                  "Crisis"),
       col   = c("#378ADD", "#E24B4A", "#E24B4A"),
       lty   = c(1, 1, 2), lwd = 2, cex = 0.7)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

**Crisis pattern — Crisis frequency over time** : A visualization of the
proportion of societies experiencing crisis per decade across the full
simulation. It demonstrates the increase of crisis frequency over time
as cumulative pressure builds.

``` r
crisis_rate_by_decade <- rowMeans(y_final)

plot(1:T_final, crisis_rate_by_decade,
     type = "l", col = "#E24B4A", lwd = 2, 
     xlab = "Decades", ylab = "Crisis rate", 
     main = "Crisis frequency across societies over time")
abline(h = mean(crisis_rate_by_decade), 
       lty = 2, col = "gray50")
abline(v = t0_l, lty = 3, col = "#1D9E75")
abline(v = t0_w, lty = 3, col = "#E24B4A")
legend("topleft", 
       legend = c("crisis rate", "mean rate", 
                  "labor surplus inflection point", 
                  "wage gap inflection point"), 
       col = c("#E24B4A", "gray50", "#1D9E75", "#E24B4A"), 
       lty = c(1, 2, 3, 3), cex = 0.8)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

**Crisis pattern — Joint distribution** : A scatter plot of the
cumulative wage gap and labor surplus colored by crisis outcome.

``` r
df_plot <- data.frame(
  CP_w   = as.vector(CPfin_w),
  CP_l   = as.vector(CPfin_l),
  crisis = factor(as.vector(y_final),
                  labels = c("No crisis", "Crisis"))
)

ggplot(df_plot, aes(x = CP_w, y = CP_l, color = crisis)) +
  geom_point(alpha = 0.1, size = 0.8) +
  scale_color_manual(values = c("No crisis" = "#378ADD",
                                "Crisis"    = "#E24B4A")) +
  labs(title    = "Joint Distribution of Cumulative Pressures",
       subtitle = "Crisis events cluster where both CP_W and CP_L are high",
       x = "Cumulative Wage Pressure (CP_W)",
       y = "Cumulative Labor Pressure (CP_L)",
       color = NULL) +
  theme_minimal() +
  theme(legend.position = "bottom")
```

![](wage_lab_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->

**Crisis pattern — Marginal effect** : a visualization of the crisis
probability as a function of the cumulative wage gap (holding at
low/medium/high cumulative labor surplus). This directly demonstrates
the interaction — the slope changes depending on the other variable’s
level.

``` r
marginal_effect <- function(CPx, CPy, CPpaste = "CPy", title = "Effect of CP_W at different CP_L levels", x = "CP_W" ) {
  
  CPx_seq <- seq(0, max(CPx), length.out = 100)
  cl_levels <- quantile(CPy, c(.25, .5, .75))
  
  df_marginal <- do.call(rbind, lapply(seq_along(cl_levels), function(i) {
    data.frame(
      CPx = CPx_seq, 
      p = plogis(beta0 + beta1*CPx_seq + beta2*cl_levels[i] + beta3*CPx_seq*cl_levels[i]),
      CPy_level = factor(paste0(CPpaste, round(cl_levels[i], 2), " (Q", c(25,50,75)[i], ")"))
    )
  }))
  
  p1 <- ggplot(df_marginal,
               aes(x = CPx, y = p, color = CPy_level)) +
    geom_line(linewidth = 1) +
    scale_color_manual(values = c("#378ADD", "#D97706", "#E24B4A")) +
    labs(title = title,
         x = x, y = "P(crisis)", color = NULL) +
    theme_minimal(base_size = 11) +
    theme(
      plot.title      = element_text(size = 10, face = "bold", hjust = 0),
      legend.position = "bottom",
      legend.title    = element_blank(),
      legend.text     = element_text(size = 7),
      legend.key.size = unit(0.35, "cm"),
      legend.margin   = margin(0, 0, 0, 0),
      legend.box.margin = margin(0, 0, 0, 0),
      legend.spacing.x = unit(0.2, "cm"))
  
  return(p1)
}
```

``` r
plot_w<-marginal_effect(CPfin_w, CPfin_l, CPpaste = "CP_L", title = "Effect of CP_W at different CP_L levels", x = "CP_W")
plot_l<-marginal_effect(CPfin_l, CPfin_w, CPpaste = "CP_W", title = "Effect of CP_L at different CP_W levels", x = "CP_L")
```

``` r
plot_w + plot_l +
  plot_annotation(
    title    = "Marginal Effects — Marx's Conjunction Hypothesis",
    subtitle = "Steeper slopes at higher levels of the other variable confirm the interaction",
    theme = theme(plot.title    = element_text(size = 14, face = "bold"),
                  plot.subtitle = element_text(size = 10, color = "grey30"))
  ) &
  theme(plot.tag = element_text(size = 10))
```

![](wage_lab_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

#### 3.3 Sensitivity Analysis on the structural parameters

As the parameters are provisional values, a sensitivity analysis is
implemented to assess whether the findings remain stable across
different values.

``` r
# sensitivity grid 
s_grid <- list (
  r_l = c(.02, .05, .03), 
  t0_l = c(30, 50, 70), 
  A = c(.1, .2, .3), 
  P = c(5, 10, 15), 
  r_base = c(.01, .04, .07), 
  alpha = c(0.1, 0.5, 0.9), 
  t0_w = c(10, 60, 110)
)

params_baseline <- list(
  r_l = 0.05, r_base = 0.03, A = 0.2,
  P = 5, alpha = 0.5, t0_l = 50, t0_w = 60
)

# function to run one simulation and return estimated betas
run_simulation <- function(r_l, r_base, A, P, alpha, t0_l, t0_w) {
  
  # generate labor surplus and wage gap
  labor_surplus_sim <- func_labor_surplus(N_final, T_final, r_l = r_l, t0_l = t0_l, A = A, P = P)
  wage_gap_sim <- func_wage_gap(N_final, T_final, labor_surplus_sim, r_base = r_base, t0_w = t0_w, alpha = alpha)
  
  # generate cumulative pressure
  results <- cp(N_final, T_final, labor_surplus_sim, wage_gap_sim)
  CPsim_w <- results$CP_w
  CPsim_l <- results$CP_l
  
  # generate crisis
  ysim <- y_func(beta0, beta1, beta2, beta3, CPsim_l, CPsim_w, T_final, N_final)
  
  # df long
  df <- data.frame(
    society = rep(1:N_final, each = T_final), 
    decade = rep(1:T_final, times = N_final), 
    CPsim_w = as.vector(CPsim_w), 
    CPsim_l = as.vector(CPsim_l), 
    ysim = as.vector(ysim)
  )
  
  # fit a model
  model <- glm(ysim~CPsim_w+CPsim_l+CPsim_w:CPsim_l, data = df, family = binomial)
  
  # summary
  coefs <- coef(model)
  crisis_freq <- mean(df$ysim)
  ordering_preserved <- coefs[4] > coefs[2] & coefs[2] > coefs[3]

  return(data.frame(
    beta1_est = round(coefs[2], 3),
    beta2_est = round(coefs[3], 3),
    beta3_est = round(coefs[4], 3),
    bias_beta1 = round(coefs[2] - beta1, 3),
    bias_beta2 = round(coefs[3] - beta2, 3),
    bias_beta3 = round(coefs[4] - beta3, 3),
    crisis_freq = round(crisis_freq, 4),
    ordering_preserved = ordering_preserved
  ))
  
}
```

``` r
# run sensitivity analysis
results_list <- list()

for (param_name in names(s_grid)){
  for (val in s_grid[[param_name]]){
    
    params <- params_baseline
    params[param_name] <- val
    
    # r_l, r_base, A, P, alpha, t0_l, t0_w
    res <- run_simulation(
      r_l = params$r_l, 
      r_base = params$r_base, 
      A = params$A, 
      alpha = params$alpha, 
      t0_l = params$t0_l, 
      t0_w = params$t0_w, 
      P = params$P
    )
    
    rownames(res) <- NULL
    
    res$parameter <- param_name
    res$value <- val 
    res$baseline <- val == params_baseline[[param_name]]
    
    results_list[[length(results_list) + 1]] <- res
  }
}

sensitivity_results <- do.call(rbind, results_list)
print(sensitivity_results)
```

    ##    beta1_est beta2_est beta3_est bias_beta1 bias_beta2 bias_beta3 crisis_freq
    ## 1      1.303     1.037     1.648      0.303      0.537     -0.352      0.6233
    ## 2      0.927     0.298     2.086     -0.073     -0.202      0.086      0.6597
    ## 3      1.039     0.834     1.914      0.039      0.334     -0.086      0.6420
    ## 4      0.778     0.291     2.173     -0.222     -0.209      0.173      0.6750
    ## 5      1.098     0.392     2.018      0.098     -0.108      0.018      0.6606
    ## 6      1.040     0.430     2.000      0.040     -0.070      0.000      0.6211
    ## 7      1.024     0.687     1.931      0.024      0.187     -0.069      0.6612
    ## 8      0.804     0.441     2.150     -0.196     -0.059      0.150      0.6610
    ## 9      0.929     0.489     2.017     -0.071     -0.011      0.017      0.6598
    ## 10     0.935     0.478     2.053     -0.065     -0.022      0.053      0.6593
    ## 11     0.824     0.629     2.060     -0.176      0.129      0.060      0.6585
    ## 12     0.866     0.504     2.045     -0.134      0.004      0.045      0.6617
    ## 13     1.006     0.748     1.921      0.006      0.248     -0.079      0.6606
    ## 14     1.015     1.034     1.828      0.015      0.534     -0.172      0.6587
    ## 15     1.051     0.671     1.875      0.051      0.171     -0.125      0.6610
    ## 16     1.056     0.674     1.924      0.056      0.174     -0.076      0.6439
    ## 17     1.115     0.897     1.790      0.115      0.397     -0.210      0.6607
    ## 18     1.074     0.521     1.955      0.074      0.021     -0.045      0.6625
    ## 19     1.061     1.530     1.456      0.061      1.030     -0.544      0.7514
    ## 20     0.922     0.249     2.128     -0.078     -0.251      0.128      0.6605
    ## 21     2.706     0.435     1.063      1.706     -0.065     -0.937      0.4351
    ##    ordering_preserved parameter  value baseline
    ## 1                TRUE       r_l   0.02    FALSE
    ## 2                TRUE       r_l   0.05     TRUE
    ## 3                TRUE       r_l   0.03    FALSE
    ## 4                TRUE      t0_l  30.00    FALSE
    ## 5                TRUE      t0_l  50.00     TRUE
    ## 6                TRUE      t0_l  70.00    FALSE
    ## 7                TRUE         A   0.10    FALSE
    ## 8                TRUE         A   0.20     TRUE
    ## 9                TRUE         A   0.30    FALSE
    ## 10               TRUE         P   5.00     TRUE
    ## 11               TRUE         P  10.00    FALSE
    ## 12               TRUE         P  15.00    FALSE
    ## 13               TRUE    r_base   0.01    FALSE
    ## 14              FALSE    r_base   0.04    FALSE
    ## 15               TRUE    r_base   0.07    FALSE
    ## 16               TRUE     alpha   0.10    FALSE
    ## 17               TRUE     alpha   0.50     TRUE
    ## 18               TRUE     alpha   0.90    FALSE
    ## 19              FALSE      t0_w  10.00    FALSE
    ## 20               TRUE      t0_w  60.00     TRUE
    ## 21              FALSE      t0_w 110.00    FALSE

``` r
# ordering preserved summary
data.frame(
  total_specs = nrow(sensitivity_results), 
  ordering_preserved = sum(sensitivity_results$ordering_preserved), 
  ordering_violated = sum(!sensitivity_results$ordering_preserved), 
  perc_ordering_preserved = round(mean(sensitivity_results$ordering_preserved)*100, 1)
)
```

    ##   total_specs ordering_preserved ordering_violated perc_ordering_preserved
    ## 1          21                 18                 3                    85.7

The theoretical ordering $\beta_3 > \beta_2 > \beta_1$ is preserved in
18 out of 21 specifications, corresponding to 85.7% of the sensitivity
grid, suggesting that the model’s ability to recover the relative
importance of the predictors is largely robust to structural parameter
changes. In particular, Marx’s conjunctural argument, that **the
interaction term dominates individual effects, remains supported across
the vast majority of specifications**.

``` r
# crisis probability summary
data.frame(
  mean = round(mean(sensitivity_results$crisis_freq), 4), 
  median = round(median(sensitivity_results$crisis_freq), 4), 
  max = round(max(sensitivity_results$crisis_freq), 4), 
  min = round(min(sensitivity_results$crisis_freq), 4)
)
```

    ##     mean median    max    min
    ## 1 0.6494 0.6605 0.7514 0.4351

**The sensitivity analysis reveals that crisis frequency is highly
sensitive to changes in the structural parameters.** When structural
parameters are varied, the scale of $CP_W$ and $CP_L$ changes
accordingly, while $\beta_0$ remains fixed at its baseline value. Since
$\beta_0$ no longer corresponds to the target crisis probability p =
0.005 under the new CP scales, crisis frequency fluctuates substantially
across specifications, ranging from 0.4351 to 0.7514 with a mean of
0.6494. This limitation arises from the sensitivity analysis design, in
which $\beta_0$ is not recalibrated for each structural parameter
combination. Future work could address this by deriving specific
$\beta_0$ to ensure constant crisis frequency across the sensitivity
grid.

### 4. Parameter recovery analysis

#### 4.1 Frequentist approach

The true parameters are known by construction. By fitting the logistic
regression, we can validate whether the sample size N derived from the
EPV rule is sufficient to recover the true parameters, assess if a
standard logistic regression remains reliable under the ARMA(1,q)
autocorrelation structure of $CP_L$ and $CP_W$ and test whether the
parameter $\beta_3$ is detectable. Here, $\beta_3$ represents the
mathematical formalization of Marx’s argument that the interaction of
wage depression and labor surplus amplifies crisis emergence. This
approach allows us to determine whether the statistical model can
recover known parameters from synthetic data generated under controlled
conditions. **If the estimated parameters diverge substantially from the
true parameters, the model certainly cannot be trusted to estimate
unknown parameters from real historical data**

To fit the logistic regression, we will utilize two estimation
approaches and evaluate the differences in the resulting coefficients :

**Long format + glm()**

This is the cleanest approach for logistic regression in R: the NxT
matrices to a long format dataframe and fit the data straightforwardly.

``` r
df_long <- data.frame(
  society = rep(1:N_final, each = T_final), 
  decade = rep(1:T_final, times = N_final), 
  CP_w = as.vector(CPfin_w), 
  CP_l = as.vector(CPfin_l), 
  y = as.vector(y_final)
)
```

``` r
model <- glm(y~CP_w+CP_l+CP_w:CP_l, 
             data = df_long, 
             family = binomial(link = "logit"))
summary(model)
```

    ## 
    ## Call:
    ## glm(formula = y ~ CP_w + CP_l + CP_w:CP_l, family = binomial(link = "logit"), 
    ##     data = df_long)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -3.0576  -0.1110   0.1536   0.1764   3.2605  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept) -5.31055    0.14681 -36.173  < 2e-16 ***
    ## CP_w         1.03531    0.11638   8.896  < 2e-16 ***
    ## CP_l         0.57678    0.17944   3.214  0.00131 ** 
    ## CP_w:CP_l    1.95633    0.09633  20.310  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 99697  on 77799  degrees of freedom
    ## Residual deviance: 16522  on 77796  degrees of freedom
    ## AIC: 16530
    ## 
    ## Number of Fisher Scoring iterations: 7

**Custom Maximum Likelihood Estimation function**

This method shows what glm() is doing internally.

``` r
X <- cbind(
  intercept = 1, 
  CP_w = as.vector(CPfin_w), 
  CP_l = as.vector(CPfin_l), 
  interaction = as.vector(CPfin_w)*as.vector(CPfin_l)
)

y_vec <- as.vector(y_final)
```

``` r
log_likelihood <- function(beta, X, y) {
  
  z <- X %*% beta
  p <- 1/(1+exp(-z))
  ll <- sum(y*log(p + 1e-10) + (1-y)*log(1-p  + 1e-10))
  
  return(-ll)
}
```

``` r
results <- optim(
    par = c(0,0,0,0),
    fn = log_likelihood,
    X = X, 
    y = y_vec, 
    method = "BFGS", 
    hessian = TRUE
  )
```

``` r
data.frame(
  true = c(beta0, beta1, beta2, beta3), 
  estimated_1 = c(coef(model)), 
  estimated_2 = c(results$par)
)
```

    ##                true estimated_1 estimated_2
    ## (Intercept) -5.2933  -5.3105509  -5.3105189
    ## CP_w         1.0000   1.0353090   1.0352842
    ## CP_l         0.5000   0.5767759   0.5767352
    ## CP_w:CP_l    2.0000   1.9563257   1.9563533

The close alignment between the estimated and true parameters
demonstrates that both models successfully recover the parameters
derived from Marxian theory. This also validates our use of the EPV
rule, confirming that the sample size was sufficient for reliable
estimation. Furthermore, the estimated parameters maintain the expected
ordering ($\beta_3$ \> $\beta_2$ \> $\beta_1$), providing empirical
support for the theoretical argument regarding the dominance of the
interaction term. Finally, despite the autocorrelated structure of the
CP variables, standard logistic regression via glm() performs comparably
to the custom MLE. This suggests that, under this specific simulation
design, the autocorrelation does not introduce severe bias into the
estimates.

#### 4.2 Bayesian approach

The previous simulations used a frequentist framework, which treats
parameters as fixed, unknown values. However, because this project aims
to model the challenges of real historical data—where observations are
not truly independent—a Bayesian approach provides a more honest and
accurate quantification of uncertainty. Bayesian inference takes a
different view: instead of assuming fixed parameters, it treats them as
random variables with probability distributions. It combines our initial
beliefs (the prior) with the evidence from the data (the likelihood) to
produce updated beliefs (the posterior): **posterior ∝ likelihood ×
prior** .Rather than a single point estimate, the posterior captures the
full range of plausible parameter values, making uncertainty
quantification explicit. Since the posterior is typically analytically
intractable, we approximate it through sampling. To this end, we
implement two methods from the Markov Chain Monte Carlo (MCMC) family: a
manual Metropolis–Hastings sampler and Hamiltonian Monte Carlo.

      -   Manual Metropolis-Hastings

For our manual Metropolis-Hastings implementation, we assign Normal
priors to the model parameters. Specifically, we center the priors for
the parameters $\beta_0$, $\beta_1$, $\beta_2$ and $\beta_3$ at zero to
reflect a neutral initial assumption.

``` r
# log prior function 
log_prior <- function(beta){
  # beta0~Normal(0, 10)
  # beta1, beta2, beta3~Normal(0, 5)
  dnorm(beta[1], 0, 10, log = TRUE)+ 
  dnorm(beta[2], 0, 5, log = TRUE)+ 
  dnorm(beta[3], 0, 5, log = TRUE)+ 
  dnorm(beta[4], 0, 5, log = TRUE)
}

# Same log_likehood as we use in the manual implementation of the logistic regression.
log_posterior <- function(beta, X, y){
  log_prior(beta) + log_likelihood(beta, X, y)
}
```

``` r
manual_metropolis_hasting <- function(X, y, n_iter = 5000, proposal_sd = .05, model){
  
  # initialize the storage
  samples <- matrix(NA, nrow = n_iter, ncol = 4)
  colnames(samples) <- c("beta0","beta1","beta2", "beta3")
  
  # starting point 
  beta_current <- coef(model)
  
  # initialize the acceptance
  acceptance <- 0
  
  for (i in 1:n_iter){
    # add random noise to beta_current to get the beta_proposed
    beta_proposed <- beta_current + rnorm(4, mean = 0, sd = proposal_sd)
    # compute the log posterior ratio
    log_ratio <- log_posterior(beta_proposed, X, y) - log_posterior(beta_current, X, y)
    # accept or reject 
    if (log(runif(1)) < log_ratio) {
      beta_current <- beta_proposed # update beta_current
      acceptance <- acceptance + 1
    }
    samples[i,] <- beta_current
  }
  cat("Acceptance rate: ", round(acceptance/n_iter, 3), "\n")
  
  return(samples)
}
```

``` r
set.seed(42)
mcmc_samples <- manual_metropolis_hasting(X, y_vec, n_iter = 5000, proposal_sd = .05, model)
```

    ## Acceptance rate:  0.813

``` r
trace_plot <- function(sample){

    for (j in 1:4) {
      plot(sample[, j],
           type = "l",
           main = paste("Trace —", colnames(sample)[j]),
           xlab = "Iteration",
           ylab = "Value",
           col  = "#378ADD")
      abline(h = c(beta0, beta1, beta2, beta3)[j],
             col = "#E24B4A", lwd = 2, lty = 2)
    }
}

post_plot <- function(samples){
    # discard warm up 
  warm_up <- 1000 
  post_samples <- samples[(warm_up+1):(nrow(samples)), ]
  

  for(j in 1:4){
    param_name <- colnames(post_samples)[j]
    true_value <- c(beta0, beta1, beta2, beta3)
    
    hist(post_samples[,j], 
         breaks = 50, 
         main   = paste("Posterior of", param_name),
         xlab   = param_name,
         col    = "#378ADD",
         border = "white",
         freq   = FALSE)
    
      # true value
    abline(v = true_value, col = "#E24B4A", lwd = 2, lty = 2)
    
    # posterior mean
    abline(v = mean(post_samples[, j]), 
           col = "#1D9E75", lwd = 2)
    
    legend("topright",
           legend = c("True value", "Posterior mean"),
           col    = c("#E24B4A", "#1D9E75"),
           lty    = c(2, 1), lwd = 2, cex = 0.7)
  }
}
```

``` r
par(mfcol = c(2, 4), mar = c(4, 4, 2, 1))
trace_plot(mcmc_samples)
post_plot(mcmc_samples)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-45-1.png)<!-- -->

``` r
par(mfrow = c(1, 1))
```

- $\beta_0$ , $\beta_2$ : The chain drifts immediately from the true
  value and stabilized in an incorrect plateau. This happens when the
  posterior has multiple modes : a result of the multicollinearity
  between $CP_W$ and $CP_L$ and the interaction term.
- $\beta_1$, $\beta_3$ : The chain is exploring but very inefficiently.
  The traces drift slowly and are highly autocorrelated. While
  convergence is eventually achieved, the inefficiency reflects the
  difficulty of isotropic proposals in navigating the correlated
  posterior geometry.

**Remediation — Standardized observations:** To optimize the
Metropolis-Hastings algorithm, the predictor matrix was standardized.
Putting all variables on the same scale prevents numerical instability
and symmetrizes the likelihood surface, making it much easier for the
algorithm’s random walk to navigate the parameters efficiently.
Centering the predictors before forming the interaction term removes the
artificial correlation that created the spurious modes, so chains no
longer get trapped ($\beta_0$ , $\beta_2$), placing all variables on a
common scale makes the posterior roughly round, so a single step size
works in every direction and the chain crosses it in normal strides
rather than baby steps ($\beta_1$, $\beta_3$).

``` r
# standardize X
X_std <- cbind(
  intercept = 1, 
  CP_w = scale(as.vector(CPfin_w))[,1], 
  CP_l = scale(as.vector(CPfin_l))[,1], 
  interaction = scale(as.vector(CPfin_w)*as.vector(CPfin_l))[,1]
)
```

``` r
model_std <- glm(y_vec~X_std-1, family = binomial)
```

``` r
# manual Metropolis Hasting with standardized observations X_std
set.seed(42)
mcmc_std <- manual_metropolis_hasting(X_std, y_vec, n_iter = 5000, proposal_sd = .05, model_std)
```

    ## Acceptance rate:  0.52

``` r
par(mfcol = c(2, 4), mar = c(4, 4, 2, 1))
trace_plot(mcmc_std)
post_plot(mcmc_std)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-49-1.png)<!-- -->

``` r
par(mfrow = c(1, 1))
```

- $\beta_0$ : Initialized at 0 — far above the true value (-5.2933) —
  and takes \~2000 iterations to cross the true value. This indicates
  insufficient warmup; early post-warmup samples fail to represent the
  stationary distribution and contaminate the inference.
- $\beta_1$, $\beta_2$, $\beta_3$ : These parameters drift
  systematically downward from the true value and never recover. Because
  $\beta_0$ starts heavily overestimated,the model immediately
  compensates by pushing the slopes too low. Since the cumulative
  pressure terms $CP_W$ and $CP_L$ are positively correlated, , the
  sampler becomes trapped on a likelihood “ridge”— unable to distinguish
  the true parameter combination from false, compensatory ones.
  Consequently, even after $\beta_0$ crosses the true value, the trapped
  slopes force it to continue drifting downward to maintain the
  likelihood.

**Remediation — multivariate Normal proposal:** The compensatory trap
and likelihood “ridge” described above stem from extreme
multicollinearity: the positively correlated predictors ($CP_W$, $CP_L$
and their interaction term) lack the independent variation needed to
isolate the intercept and slopes. **A standard Metropolis-Hastings
algorithm updates parameters independently, blinding it to this joint
covariance and causing it to slide along these false compensatory
paths.** By implementing a multivariate Normal proposal, the sampler can
learn the underlying covariance structure and propose joint parameter
updates. **This allows $\beta_0$ and the slopes to move together in the
correct directions, effectively escaping the ridge and converging to the
true values.**

``` r
multivariate_normal_proposal <- function(X, y, n_iter = 5000, proposal_sd = .05, model){
  
  # initialize the storage
  samples <- matrix(NA, nrow = n_iter, ncol = 4)
  colnames(samples) <- c("beta0","beta1","beta2", "beta3")
  
  # starting point 
  beta_current <- coef(model)
  
  # initialize the acceptance
  acceptance <- 0
  
  step_cov <- vcov(model)*proposal_sd^2
  
  for (i in 1:n_iter){
    # add random noise to beta_current to get the beta_proposed
    beta_proposed <- mvrnorm(n = 1, mu = beta_current, Sigma = step_cov)
    # compute the log posterior ratio
    log_ratio <- log_posterior(beta_proposed, X, y) - log_posterior(beta_current, X, y)
    # accept or reject 
    if (log(runif(1)) < log_ratio) {
      beta_current <- beta_proposed # update beta_current
      acceptance <- acceptance + 1
    }
    samples[i,] <- beta_current
  }
  cat("Acceptance rate: ", round(acceptance/n_iter, 3), "\n")
  
  return(samples)
}
```

``` r
set.seed(42)
mnp_samples <- multivariate_normal_proposal(X, y_vec, n_iter = 5000, proposal_sd = .05, model)
```

    ## Acceptance rate:  0.705

``` r
par(mfcol = c(2, 4), mar = c(4, 4, 2, 1))
trace_plot(mnp_samples)
post_plot(mnp_samples)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-52-1.png)<!-- -->

``` r
par(mfrow = c(1, 1))
```

- $\beta_0$ , $\beta_3$ : The chains cross the true value during the
  first iterations but subsequently drift upward and never recover,
  converging to a posterior mean that consistently overestimates the
  true value.
- $\beta_1$ , $\beta_2$ : The opposite pattern is observed, with chains
  drifting downward and converging to a posterior mean that
  underestimates the true value.

**Diagnosing convergence with the R-hat statistic:** The trace plots and
posterior distributions of the parameters suggest that the manual
Metropolis–Hastings sampler failed to achieve simultaneous convergence
across all parameters. To confirm this with a quantitative measure, we
compute the R-hat. **R-hat is a convergence diagnostic that compares the
variance between chains to the variance within each chain.** It
indicates whether the MCMC chains have converged to the same posterior
distribution: values close to 1 suggest convergence, while larger values
reveal that the chains are exploring different regions of the parameter
space.

**Between-chain variance** : This measures how different the two chain
means are from each other.

$$
B = \frac{N}{M-1} \sum^M_{m=1} (\hat{\psi}_m - \hat{\psi})^2
$$

- $M$ : number of chains.
- $N$ : number of post-warmup samples per chain.
- $\hat{\psi}_m$ : The mean of chain m.
- $\hat{\psi}$: The overall mean across all chain m.

**Within-chain variance** : This measures how much each chain varies
internally around its own mean.

$$
W = \frac{1}{M} \sum^M_{m=1} s^2_m
$$

- $s^2_m$ : The variance of the chain m.

**Estimated marginal posterior variance** : This combines B and W into
an estimate of the true posterior variance.

$$
\hat{var} = \frac{N-1}{N}W + \frac{1}{N}B
$$

**R-hat** :

$$
\hat{R} = \sqrt \frac{\hat{var}}{W}
$$

- $\hat{R}$ ≤ 1 : **Ideal convergence.** The chains have mixed well and
  accurately represent the target posterior distribution.
- $\hat{R}$ = 1.01 to 1.05 : **Minor concern.** The chains have largely
  converged, but there may be slight imperfections in mixing. Results
  are generally reliable, but inspecting trace plots or increasing
  iterations is recommended.
- $\hat{R}$ = 1.05 to 1.10 : **Serious concern.** The chains have not
  fully explored the parameter space; posterior summaries may be
  unstable or biased.
- $\hat{R}$ \> 1.10 : **Convergence failure.** The sampler failed to
  explore the posterior distribution. Results should not be trusted, and
  the model specification, priors, or sampler settings require revision.

``` r
compute_rhat <- function(samples) {
  
  n_params <- ncol(samples) 
  rhat <- numeric(n_params) # create an empty vector of Rhat
  
  for (j in 1:n_params) {
    chain <- samples[,j]
    N_total <- length(chain)
    
    # split the chain
    half <- floor(N_total/2)
    chain1 <- chain[1:half]
    chain2 <- chain[(half+1):(half*2)] # to make sure chain1 and chain2 have the same length
    
    # set N and M
    N <- half # length of each split chain
    M <- 2 # number of chains
    
    # chains mean
    psi_bar_1 <- mean(chain1)
    psi_bar_2 <- mean(chain2)
    psi <- mean(psi_bar_1, psi_bar_2)
    
    # between-chain variance
    B <- (N/(M-1)) * sum( (psi_bar_1 - psi)^2, (psi_bar_2 - psi)^2)
    
    # within chain variance 
    W <- (var(chain1)+var(chain2))/M
    
    # estimated marginal posterior variance 
    var_hat <- ((N-1)/N)* W + (1/N)*B
    
    # R-hat 
    rhat[j] <- sqrt(var_hat/W)
  }
  return(rhat)
}
```

``` r
rhat_mcmc_samples <- compute_rhat(mcmc_samples)
rhat_mcmc_std <- compute_rhat(mcmc_std)
rhat_mnp_samples <- compute_rhat(mnp_samples)
```

``` r
data.frame(
  parameters = c("beta0", "beta1", "beta2", "beta3"), 
  rhat1 = c(rhat_mcmc_samples), 
  rhat2 = c(rhat_mcmc_std), 
  rhat3 = c(rhat_mnp_samples)
)
```

    ##   parameters    rhat1    rhat2    rhat3
    ## 1      beta0 2.168147 3.940527 3.460051
    ## 2      beta1 1.180604 3.541819 2.855445
    ## 3      beta2 1.032897 2.190855 3.597005
    ## 4      beta3 2.291407 3.556249 3.377270

**R-hat results for the Metropolis–Hastings variants:** All three
samplers—standard Metropolis–Hastings, Metropolis–Hastings with
standardized predictors, and Metropolis–Hastings with a multivariate
normal proposal—fail the convergence test. With $\hat{R}$ values ranging
from 1.28 to 3.85, every variant lies far from the $\hat{R}$ ≤ 1
benchmark. These values quantitatively confirm the diagnosis suggested
by the trace plots: no variant achieves simultaneous convergence across
all four parameters. Notably, even in the best case (standard
Metropolis–Hastings, where $\beta_2$ $\hat{R}$ reaches 1.030176),
convergence occurs for a single parameter only, while the remaining
three fail badly. **This illustrates a key limitation: individual
parameters may appear well-behaved in isolation, yet the joint posterior
remains unreachable.** Moreover, the fact that refining the proposal
(standardization, multivariate structure) does not resolve the issue
suggests the bottleneck is not the tuning of the proposal, but the
random-walk nature of Metropolis–Hastings itself—reinforcing the need
for the gradient-based approach of Hamiltonian Monte Carlo (HMC).

      -   Hamiltonian Monte Carlo

``` r
bayesian_model <- brm(
  formula = y~CP_w+CP_l+CP_w:CP_l, 
  data = df_long, 
  family = bernoulli(link = logit), 
  prior = c(
    prior(normal(0, 10), class = "Intercept"), 
    prior(normal(0, 5), class = "b")
  ), 
  chain = 4, 
  iter = 2000, 
  warmup = 1000, 
  seed = 42, 
  cores = 4, 
  control = list(
    adapt_delta = .95, 
    max_treedepth = 12
  )
)
```

    ## Compiling Stan program...

    ## Start sampling

``` r
plot(bayesian_model)
```

![](wage_lab_files/figure-gfm/unnamed-chunk-57-1.png)<!-- -->

**Hamiltonian Monte Carlo results :** In contrast to the manual
Metropolis–Hastings sampler, the HMC chains converge for all four
parameters simultaneously. For each of $\beta_0$ , $\beta_1$ , $\beta_2$
, $\beta_3$ , the chains repeatedly cross the true value and remain
centered around it, indicating that the posterior means accurately
recover the true parameters. This success is expected.
Metropolis–Hastings explores the parameter space through a random walk,
which leads to slow mixing and weak exploration—especially when
parameters are correlated. HMC, by contrast, uses gradient information
to propose informed, long-range moves, allowing the chains to mix
efficiently and reach the target posterior much faster. This illustrates
why gradient-based samplers like HMC are the method of choice in modern
Bayesian software.

``` r
summary_bayesian <- summary(bayesian_model)
rhat <- summary_bayesian$fixed[, "Rhat"]

data.frame(
  data.frame(
  parameters = c("beta0", "beta1", "beta2", "beta3"), 
  rhat_mh_1 = c(rhat_mcmc_samples), 
  rhat_mh_2 = c(rhat_mcmc_std), 
  rhat_mh_3 = c(rhat_mnp_samples),
  rhat_hmc =  (rhat)
)
)
```

    ##   parameters rhat_mh_1 rhat_mh_2 rhat_mh_3  rhat_hmc
    ## 1      beta0  2.168147  3.940527  3.460051 0.9996589
    ## 2      beta1  1.180604  3.541819  2.855445 1.0013379
    ## 3      beta2  1.032897  2.190855  3.597005 0.9998596
    ## 4      beta3  2.291407  3.556249  3.377270 1.0002140

All parameters achieved $\hat{R} \approx 1$ with the Hamiltonian Monte
Carlo algorithm, confirming convergence across the four chains. In
contrast, the manual Metropolis-Hastings sampler failed to achieve
simultaneous convergence for all parameters.

### 5. Time Series Diagnosis :

In logistic regression, the variables must be independent. The ARMA(1,q)
structure of $CP_w$ and $CP_l$ violates this assumption, resulting in
biased coefficients, an overconfident model, and inaccurate standard
errors. To address this, a time series diagnosis is performed: the ACF
of the pilot simulation is compared with that of the final simulation in
order to check the stability of the DGP and the validity of the ESS.
Moreover, residual autocorrelation from the logistic regression is
examined, since correlated residuals can lead to biased coefficients.

**1. Comparison of the ACF of the pilot and final simulation :**

``` r
acfin_matrix_l <- matrix(NA, nrow = lag_max , ncol = N_final)
acfin_matrix_w <- matrix(NA, nrow = lag_max , ncol = N_final)

acfin_avg_l <- acf_func(lag_max, acfin_matrix_l, N_final, T_final, t0_l, CPfin_l)
acfin_avg_w <- acf_func(lag_max, acfin_matrix_w, N_final, T_final, t0_w, CPfin_w)
```

``` r
# compare acf pilot and acf final for labor surplus
acf_comp_l<- data.frame(lag = 1:lag_max, 
                        acf_final = acfin_avg_l, 
                        acf_pilot = acf_avg_l)

# compare acf pilot and acf final for wage gap
acf_comp_w <- data.frame(lag = 1:lag_max, 
                         acf_final = acfin_avg_w, 
                         acf_pilot = acf_avg_w)
```

``` r
acf_comparison <- function(acf_comp, title = "ACF Comparison: Final vs Pilot"){
  
  # convert to long format 
  acf_long <- pivot_longer(acf_comp, 
                           cols = c(acf_final, acf_pilot), 
                           names_to = "Type", 
                           values_to = "ACF_Values")
  
  p <- ggplot(acf_long, aes(x = lag, y = ACF_Values, color = Type, group = Type)) +
    geom_line(position = position_dodge(width = 0.2), linewidth = 1) + 
    geom_point(position = position_dodge(width = 0.2), size = 3) + 
    geom_hline(yintercept = 0, linetype = "dashed", color = "gray") +
    theme_minimal() +
   
    labs(title = title,        
         x = "Lag", 
         y = "ACF Value") +
    scale_color_manual(values = c("acf_final" = "steelblue", "acf_pilot" = "firebrick"))
  
  # Return the plot
  return(p) 
}
```

``` r
plot_wage <- acf_comparison(acf_comp_w, title = "ACF Comparison (wage gap)" )
plot_labor <-acf_comparison(acf_comp_l, title = "ACF Comparison (labor surplus)" )
combined_plot <- plot_wage + plot_labor
combined_plot
```

![](wage_lab_files/figure-gfm/unnamed-chunk-62-1.png)<!-- -->

As confirmed by the plots, the final and pilot simulations exhibit the
**same autocorrelation structure.** This alignment indicate two key
points:

- **Stability of the Data Generating Process (DGP):** The DGP is stable,
  meaning the underlying parameters consistently produce the same
  autocorrelation structure regardless of the sample size.

- **Validity of the Effective Sample Size (ESS):** The ESS derived from
  the pilot simulation remains valid for the final simulation, as the
  underlying autocorrelation structure has remained unchanged.

**2. Residual autocorrelation from logistic regression**

The ARMA(1,q) structure of $CP_w$ and $CP_l$ violates the logistic
regression assumption of independent observations. However, this does
not necessarily bias the estimated $\beta$ coefficients; rather, the
primary consequence is **inaccurate standard errors**. Because $CP_w$
and $CP_l$ are expected to capture the temporal structure, the residuals
should be uncorrelated. **The presence of correlated residuals can lead
to biased standard errors.**

``` r
residuals_pearson <- residuals(model, type = "pearson")
lag_max <- min(Tl_obs/4, 20)
df_long_ord <- df_long %>% arrange(society, decade)
acf_resid_matrix <- matrix (NA, nrow = lag_max, ncol = N_final)


for (s in 1:N_final){
  # Extract the residuals for society s
  res_s <- residuals_pearson[df_long_ord$society == s]
  acf_resid_matrix[,s]<-acf(res_s, lag_max, plot = FALSE)$acf[-1]
}

avg_resid_matrix <- rowMeans(acf_resid_matrix)
```

``` r
acf_plot(avg_resid_matrix, T_final, title = "ACF residuals")
```

![](wage_lab_files/figure-gfm/unnamed-chunk-64-1.png)<!-- -->

Because the predictors $CP_W$ and $CP_L$ effectively explain the
autoregressive nature of the data, the model residuals exhibit
negligible autocorrelation, as demonstrated in the plots below.

### Discussion
