Four-phase structure for the reserve armies - Notes
================

#### Tier 1 : standard reserve armies

$$
R_1(t) = \begin{cases}
0 & \text{if } \theta(t)> \theta^\ast (t) \\
L^s_a(\bar{w}_{min}, t) - L^d_a(\bar{w}_{min},\theta(t)) & \text{if } \theta(t)< \theta^\ast (t)
\end{cases}
$$

With $L^s_a(\bar{w}_{min}, t)$ =
$L^s_a(\bar{w}_{min}) \cdot \frac{N(t)}{N(t_0)}$

$\frac{N(t)}{N(t_0)}$ is a population scaling factor : as the population
grows over time, the pool of available workers expands proportionally.
$t_0$ is the normalization reference. Population growth follows the
secular-cycle phases — expansion, stagflation, crisis, depression.

To model the population growth, we use a logistic growth with carrying
capacity :

$$
\frac{dN}{dt} = r_N \cdot N \cdot (\frac{N}{K}) - d_N(t) \cdot N
$$

- $r_N$ : natural population growth rate
- $K$ : carrying capacity (maximum sustainable population given
  agricultural/economic productivity)
- $d_N(t)$ : excess mortality rate for the general population

Separating excess mortality from natural population growth exposes the
secular-cycle phases. As a general rule, mortality rises alongside
population; but once carrying capacity is breached, the two
decouple—population falls while mortality keeps climbing, driven by
warfare, famine, and institutional collapse:

- Expansion: $d_N(t)$ ≈ 0 — normal mortality only
- Stagflation: $d_N(t)$ small — slightly elevated (famine, disease
  begin)
- Crisis: $d_N(t)$ large — war, famine, epidemic mortality
- Depression: $d_N(t)$ → 0 — mortality returns to normal

Thus, the phase dynamics come from $d_N(t)$ . We assume that excess
mortality of the general population begins during stagflation and peaks
dramatically during the transition into the crisis phase. Throughout
stagflation, the population continues to grow until it reaches its
ceiling. At this point, available resources are no longer sufficient for
subsistence. Although population density remains high, foodstuff prices
rise steadily. Inflation affects both the general population and the
elites; however, the former suffer from malnutrition, famine, and
disease, while the latter still manage to preserve their standard of
living. This asymmetry stems from the pressure elites exert on the
proletariat by depressing wages or raising land rents. Driven by these
conditions, elites increasingly vie for higher positions, fueling
intense intra-elite competition. Historically, conflicts among the
elites serve as the trigger for the next phase — crisis — during which
the mortality rate continues its upward trajectory until reaching its
highest point.

**Excess mortality rate and the phase indicator**

We introduce a phase indicator $\phi(t)$ to distinguish between the
excess mortality rate during stagflation and that during crisis.

$$
d_N(t) = d_{N,base} + (d_{N,stag} \cdot 1[\phi(t) = stag]) + (d_{N,crisis} \cdot 1[\phi(t) = crisis]) 
$$

$\phi(t)$ is a phase signal that modulates the equation according to the
current regime: the corresponding indicator $1[\phi(t) = \cdot]$ equals
1 when $\phi(t)$ matches that phase and 0 otherwise.

When the system is in stagflation, the indicator function
$1[\phi(t) = stag]$ activates, contributing to $d_{N,stag}$. During
crisis, $1[\phi(t) = crisis]$ takes over, raising the excess mortality
by $d_{N, crisis}$ .

**The floor condition**

Because the secular accumulation mechanism prevents any cycle from fully
resetting, each successive cycle should start from a baseline $R_1$
higher than that of the previous one.

$$
R_{1,floor}^{n+1} = R_{1,floor}^{n} \cdot (1 + g_{floor}) 
$$

We add a floor condition to ensure that the reserve army at cycle $n+1$
is no smaller than that at cycle $n$ . $R_{1,floor}$ denotes the minimum
reserve army that persists even after the most severe purge. Each crisis
removes part of the reserve army, yet it does not erase the conditions
that produce it.
