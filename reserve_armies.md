Four-phase structure for the reserve armies - Notes
================

#### Tier 1 : the standard reserve armies

$$
R_1(t) = \begin{cases}
0 & \text{if } \theta(t)> \theta^\ast (t) \\
L^s_a(\bar{w}_{min}, t) - L^d_a(\bar{w}_{min},\theta(t)) & \text{if } \theta(t)< \theta^\ast (t)
\end{cases}
$$

With labor supply :

$$
L^s_a(\bar{w}_{min}, t) = L^s_a(\bar{w}_{min}) \cdot \frac{N(t)}{N(t_0)}
$$

$\frac{N(t)}{N(t_0)}$ is a population scaling factor : as the population
grows over time, the pool of available workers expands proportionally.
$t_0$ is the normalization reference. Population growth follows the
secular-cycle phases — expansion, stagflation, crisis, depression.

To model the population growth, we use a logistic growth with carrying
capacity :

$$
\frac{dN}{dt} = r_N \cdot N \cdot \frac{N}{K} - d_N(t) \cdot N
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
living. They multiplied to the point of overproduction, fueling intense
intra-elite competition. Historically, conflicts among the elites serve
as the trigger for the next phase — crisis — during which the mortality
rate continues its upward trajectory until reaching its highest point.

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

So the phase indicator modulates mortality, which modulates population,
which modulates labor supply, which modulates the reserve army $R_1$ —
the chain is:

$$
\phi(t) \implies d_N(t) \implies N(t) \implies L^s_a(t) \implies R_1(t)
$$

**What triggers each phase transition**

The four phases follow a fixed cyclic order. Each phase is constructed
sequentially, conditional on the realization of its predecessor, and
accounts for the evolution of the population $N(t)$ elative to the
carrying capacity $K$ over the course of the cycle. Formally, for a
phase $\phi_j$ to be active at time $t$ , the preceding phase
$\phi_{j-1}$ must have been active at $t-n_j$ where $n_j > 0$ is the
phase-specific transition lag :

$$
\phi(t) = \phi_j \implies \phi(t-n_j) = \phi_{j-1}\ ,j  \ mod \ 4
$$

**The floor condition**

Because the secular accumulation mechanism prevents any cycle from fully
resetting, each successive cycle should start from a baseline $R_1$
higher than that of the previous one. To ensure $R_1$ never resets to 0
:

$$
R_1^{n}(t) \geq R_{1,floor}^{n}
$$

$R_{1,floor}$ denotes the minimum reserve army that persists even after
the most severe purge. And we add a floor condition to ensure that the
reserve army at cycle $n+1$ is no smaller than that at cycle $n$ .

$$
R_{1,floor}^{n+1} = R_{1,floor}^{n} \cdot (1 + g_{1,floor}) 
$$

**Each crisis removes part of the reserve army, yet it does not erase
the conditions that produce it**. After depression, the economy enters a
new expansion but it starts from a slightly more automated baseline than
the previous expansion. The more automation penetrates during a cycle,
the more workers remain permanently displaced after recovery (their
skills have grown obsolete, they are too old to retrain, and the sectors
that shed them never fully recover). Thus, automation intensity governs
the severity of each cycle, captured by the parameter $g_{1,floor}$ .

$g_{1,floor}$ is secular growth rate that reflects the accumulation of
technological displacement. It measures how much structural change each
cycle leaves behind. For $t = 1, ... , n$ :

$$
g_{1,floor} = \frac{\theta(1) - \theta(n)}{\theta(1)}
$$

This formula captures the proportional decline in the automation
coefficient $\theta$ over the cycle. The more automation intensifies (
$\theta$ falls ) , the higher the floor for the next cycle.

#### Tier 2 : the elite reserve armies

$$
R_2 = max(A(t)- W(t), 0)
$$

The aspirants and established elites growth are respectively :

$$
\frac{dA}{dt} = [\mu \cdot (\frac{\rho}{w}) \cdot N] + [\lambda  \cdot R_1(t-\tau)] - \gamma \cdot W - d_A(t) \cdot A
$$

$$
\frac{dE}{dt} = r_e \cdot E \cdot \frac{\rho}{w} + \gamma \cdot W - d_E(t) \cdot E
$$

- $d_A(t)$ : the aspirants purge term.
- $d_E(t)$ : the position-holders purge term.
- $\gamma \cdot W$ : the placement into positions. It represents the
  aspirants successfully placed into available positions.

**Differential purge rates and the phase indicator**

During stagflation, a **wealth pump** begins to funnel resources away
from commoners: a growing population creates labor oversupply while
resources grow scarce. The upper class profits from this squeeze by
depressing wages and raising prices. While the general population
suffers immiseration, the elites siphon off the surplus, multiply, and
thereby generate elite overproduction. The elite population, however, is
also bound by a carrying capacity defined by the number of available
positions, which grows only very slowly. The crisis does not erupt so
long as elite cohesion holds, but the competition for these positions
sparked by overproduction becomes the breaking point of that cohesion —
the elite splits into ruling factions and counter-elites, and the
conflicts begin. Violence, lawlessness, and economic collapse during the
crisis phase violently prune the upper class, which contracts in both
numbers and wealth. Systemic exhaustion from the crisis then purges the
remaining elites through passive economic and demographic elimination
during the depression phase. Without the wealth pump, they can no longer
sustain their former standard of living. The failed elites undergo rapid
downward mobility and may perish from starvation and disease. After the
purge, the elite population shrinks to a manageable level, resetting the
secular cycle for a new expansion.

$$
d_A(t) = d_{A,base} + (d_{A,crisis} \cdot 1[\phi = crisis]) + (d_{A,depr} \cdot 1[\phi = depr])
$$

$$
d_E(t) = d_{E,base} + (d_{E,crisis} \cdot 1[\phi = crisis]) + (d_{E,depr} \cdot 1[\phi = depr])
$$

**The floor condition**

For $t = 1, ..., n$

$$
g_{2,floor} = \frac{max_tA(t) - A(1)}{max_tA(t)}
$$

$g_{2,floor}$ captures the peak aspirant accumulation. The peak of $A$
during the cycle is what matters — it tells how large the aspirants pool
actually grew before being purged. After the depression, $A(n)$ is
artificially low and it does not reflect the structural change the cycle
produced. A large floor indicates that a high number of aspirants sought
elite positions during the cycle. The more aspirants a cycle produces,
the more credential infrastructure, educational pathways, and social
expectations of elite status persist after the purge. This leaves behind
a structural residue that is transferred to the next cycle. Thus, the
floor condition ensures that no two cycles are alike — each cycle starts
from a slightly more elite-reproducing baseline than its predecessor.

#### Do the purges clear the labor market?

In a Walrasian world, the reserve armies cannot mathematically exist.
Yet our natural evolution toward progress is precisely what feeds the
exclusion machine — the very apparatus that continuously expands the
reserve armies. Progress, for instance, drives automation that displaces
workers, while the accessibility of symbolic pathways (university
degrees, political influence) to the upper class yields elite
overproduction. Within a secular cycle, the trajectory of the reserve
army does not remain constant: when purges begin, it tends to decline.
Thus, purges remove a part of the reserve army but do not fully reset
the cycle after the depression: the system, always governed by the
elites, remains. This persistence stems not only from technological
progress and the credential pathways built up during the previous cycle,
but also by the consolidation of the winning elite faction. Indeed,
Boyer (2015) underscores that the very stability of such a coalition
endogenously triggers the transition from growth to crisis, affecting
both the economic and political orders. Crucially, because the crisis is
a direct consequence of a stable ruling class rather than an external
failure, the structural integrity of the “same” system remains
uncontested when the crisis peaks. Exhausted by a prolonged period of
crisis and depression, the population is easily pacified by the
subsequent expansion and its attendant material improvements; this
transition leads to the false conclusion that the system does not fail.
In reality, the illusion of an attainable equilibrium merely masks how
the purge during the crisis and depression phases operates as a systemic
reset, recalibrating the machinery of elite domination for a new cycle.
Thus, expansion leads once more to overpopulation, and the next phases
follow in sequence — stagflation, crisis, depression — as the reserve
armies re-emerge.

The purges do not clear the market; they function instead as a
biological regulatory mechanism that perpetuates the secular cycle. Marx
conceived of crises as the terminal contradiction of capitalism — the
inevitable outcome of structural pressures. Yet the challenge they pose
to the status quo yields no reformed system, and capitalism does not
resolve its contradictions. Instead, it purges whenever the working
class and the upper class expand to the point of labor oversupply and
elite overproduction. The resulting “great reset” of both populations
temporarily improves living conditions during the early phase of
expansion. In this way, the system continually renews its own
legitimacy: a few elite aspirants secure positions, and the system
appears meritocratic; some workers find employment, and it appears
responsive; wages rise, and it appears self-correcting. As Lordon
(1997;1999) suggested, this illustrates the symbolic representation
required by the hegemonic bloc (a coalition of different social strata
unified under the leadership of the dominant class), to secure the
consent of the subaltern classes without coercion.
