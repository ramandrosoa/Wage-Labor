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
$t_0$ is the normalization reference. Population trajectory follows the
secular-cycle phases — expansion, stagflation, crisis, depression.

To model the population growth, we use a logistic growth with carrying
capacity :

$$
\frac{dN}{dt} = r_N \cdot N (1-\frac{ N}{K}) - d_N(t) \cdot N
$$

- $r_N$ : natural population growth rate
- $K$ : carrying capacity (maximum sustainable population given
  agricultural/economic productivity)
- $d_N(t)$ : excess mortality rate for the general population

Separating excess mortality from natural population growth exposes the
secular-cycle phases. As a general rule, mortality rises alongside
population; but once carrying capacity is breached, the two
decouple—population falls while mortality keeps climbing :

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
living, and they multiply to the point of overproduction, fueling
intense intra-elite competition. Historically, conflicts among the
elites serve as the trigger for the next phase — crisis — during which
the mortality rate continues its upward trajectory until reaching its
highest point.

**Excess mortality rate and the phase indicator**

We introduce a phase indicator $\phi(t)$ to distinguish between the
excess mortality rate during stagflation and that during crisis. Let
$\phi(t) \in \{0,1,2,3\}$ denote the active phase at time $t$ , with the
index taken modulo 4.

$$
d_N(t) = d_{N,base} + d_{N, \phi(t)} 
$$

or

$$
d_N(t) = d_{N,base} + (d_{N,exp} \cdot \mathbb{1}[\phi(t) = 0]) + (d_{N,stag} \cdot \mathbb{1}[\phi(t) = 1]) + (d_{N,crisis} \cdot \mathbb{1}[\phi(t) = 2]) + (d_{N,depr} \cdot \mathbb{1}[\phi(t) = 3]) 
$$

$\phi(t)$ is a phase signal that modulates the equation according to the
current regime: the corresponding indicator $1[\phi(t) = \cdot]$ equals
1 when $\phi(t)$ matches that phase and 0 otherwise.

For instance, when the system is in stagflation, the indicator function
$1[\phi(t) = 1]$ activates, contributing to $d_{N,stag}$. During crisis,
$1[\phi(t) = 2]$ takes over, raising the excess mortality by
$d_{N, crisis}$ .

So the phase indicator modulates mortality, which modulates population,
which modulates labor supply, which modulates the reserve army $R_1$ —
the chain is:

$$
\phi(t) \implies d_N(t) \implies N(t) \implies L^s_a(t) \implies R_1(t)
$$

The excess mortality is tied to the capital $C(t-\omega)$ . $\omega$ is
the lag between the capital destruction and the death it causes.
$d_{N, \phi(t)}$ is given by :

$$
d_{N, \phi(t)} = \kappa_{\phi(t)} \big[1 - \frac{C(t-\omega)}{C_{ref}(t)} \big]_+
$$

- $\kappa_{\phi(t)}$ is a phase-dependent parameter, with
  $\kappa_{exp} < \kappa_{depr} < \kappa_{stag} < \kappa_{crisis}$ . The
  phase changes the sensitivity of mortality.
- $C_{ref}(t)$ is the peak over all history. it is given by :

$$
C_{ref}(t) = max_{c(t) - window} C(s)
$$

$C_{ref}(t)$ never decreases. It stores the running peak : a peak can
change multiple times during the same phase, and a peak recorded from a
previous phase can still be a reference in the current one. Thus, the
excess mortality is captured by the shortfall of the capital relative to
its running maximum.

When capital shrinks in practical terms, there is a loss of livelihood
that subsequently drives excess mortality. During stagflation, the
elites stop investing in productive capital. They tend to protect their
existing wealth by acquiring inflation hedges or by moving their liquid
capital out of the country to safer jurisdictions; they cease to
allocate their money to new factories, businesses, and maintenance — **a
passive capital destruction** via neglect and flight. Consequently, the
existing infrastructure deteriorates and no new economic capacity is
built for the growing population, while prices keep rising and real
wages remain constant. Meanwhile, the commoners are forced into deadly
compromises. Nutrient-dense foods give way to cheap, calorie-heavy
substitutes; doctor visits are postponed even when symptoms of serious
disease appear; and as rents rise with inflation, evictions follow. Each
compromise quietly shortens lives. At the macro level, capital
stagnation and misallocation depress labor demand, pushing wages down
further, deepening poverty, and ultimately driving up excess mortality.
During crisis, capital declines due to existing conflicts. The resulting
inequality from stagflation ignites widespread despair and anger among
the working class, while the frustration of aspirant elites fuels the
rise of instability. Then, counter-elites rally the masses for the
purpose of capturing and redistributing the wealth and power of the
elites; **active and physical capital destruction** can be an
alternative to weaken them. Moreover, capital declines through
operational paralysis and institutional collapse; with the loss of
customer traffic, small businesses close, with market unpredictability,
large enterprises halt their operations, as taxes dry up, municipal
services break down, and hospitals may be forced to close due to supply
scarcity. That leads to a sudden loss of daily income for workers and
the failure of essential distribution. On top of that, these conflicts
can be followed by violence. However, it is rarely the aspirants’ first
choice, because they turn to violence only when peaceful and
institutional paths to power are completely blocked. Thus, violence is
the final escalation after years of failed reform, once the aspirants
realize that they cannot win inside the system. That crucially
accelerates mortality from a slow climb into a sudden demographic
collapse.

**Phase transition**

The four phases follow a fixed cyclic order. Each phase is constructed
sequentially, conditional on the realization of its predecessor, and
accounts for the evolution of the population $N(t)$ relative to the
carrying capacity $K$ over the course of the cycle. The phase indicator
is a state machine rather than a function recomputed at each step. It
carries two state variables : the current phase $\phi(t)$ and a counter
$c(t)$ giving the number of decades spent in the current phase since
entry. Each phase $j$ has a minimum dwell time $n_j > 0$ . While
$c(t)<n_j$ , the exit conditions are not evaluated and the system
remains in phase $j$ . Once $c(t) \geq n_j$ , the exit conditions of
phase $j$ are checked at every decade. The system then transitions as
follows :

$$
\phi(t+1) = \begin{cases}
\phi(t) + 1 \ mod \ 4 & \text{if} \ c(t)>n_{\phi(t)} \ \text{and} \ S_{\phi(t)}(N(t), K) \\
\phi(t) & \text{otherwise}
\end{cases}
$$

$$
c(t+1) = \begin{cases}
0 & \text{if transition occurs} \\
c(t) + 1 & \text{otherwise}
\end{cases}
$$

- $n_{\phi(t)}$ : minimum dwell time for phase $\phi(t)$ to transition
  to phase $\phi(t) + 1$
- $S_{\phi(t)}(N(t), K)$ : state of the population $N$ relative to its
  ceiling $K$ — the exit condition of the phase $j$ at decade $t$

A phase therefore lasts for at least $n_j$ and lasts longer whenever its
exit condition is not yet satisfied. The conditions are checked again at
each following decades. The phase indicator $\phi(t+1)$ is determined
from $N(t)$ from the previous decade, and the population $N(t+1)$ of the
corresponding decade is generated using the excess mortality rate
$d_{\phi(t+1)}$ , based on the active phase at decade $t+1$ .

$$
N(t) \implies \phi(t+1) \implies d_N(t+1) \implies N(t+1)
$$

**The exit conditions**

- expansion → stagflation $(C_0)$

Let $v(t) \in \{0,1\}$ a state variable that activates whether
“convexity has been observed” :

$$
v(t) =  \max \Big(v(t-1) \ , \mathbb{1}\big[\Delta^2 N(t-1) > \epsilon_2 \cdot K \ \text{and} \ \Delta^2 N(t) > \epsilon_2 \cdot K\big]\Big)
$$

$N$ is stochastic , to avoid setting the latch from noise, we define a
small tolerance term $\epsilon$ scaled by $K$ to keep it comparable
across the simulation. $\epsilon_1$ serves as band for $\Delta N$ and
$\epsilon_2$ as a convexity threshold. For robustness against noise, we
transition to the next phase when two consecutive decades exhibit
concavity.

The following exit conditions $S_0$ are checked when $c(t) \geq n_0:$

$$
v(t) = 1 \ , \Delta^2 N(t-1) <  -\epsilon_2 \cdot K \ \text{and} \ \Delta^2 N(t) < -\epsilon_2 \cdot K  \ , \Delta N(t) > \epsilon_1 \cdot K , N(t) < (1-\delta)K
$$

The population has accelerated in the early expansion (
$\Delta^2 N(t) > \epsilon_2 \cdot K$ ) . Later, it starts decelerating
but still growing below the ceiling (
$\Delta^2 N(t) < -\epsilon_2 \cdot K$ ) .

- stagflation → crisis $(C_1)$

The state variable $v(t)$ activates when “plateau has been observed” :

$$
v(t) = \max \Big(v(t-1) , \mathbb{1}\big[|\Delta N(t-1)| < \epsilon_1 \cdot K \ \text{and} \ |\Delta N(t)| < \epsilon_1 \cdot K \ \ \text{and} \ N(t) \geq (1-\delta)K\big] \Big)
$$

The following exit conditions $S_1$ are checked when $c(t) \geq n_1:$

$$
v(t) = 1 \ , \ \Delta N(t-1) < -\epsilon_1 \cdot K \ , \ \Delta N(t) < -\epsilon_1 \cdot K \ , \ N(t) \geq (1-\delta)K
$$

During stagflation, population growth slows and approaches the carrying
capacity. Resource scarcity and rising inflation deteriorate overall
living conditions, driving the population to plateau and subsequently
decline as mortality increases. This decline marks the entry into
crisis, as rising mortality begins to outpace population growth.

- crisis → depression

When $N$ exit stagflation, it is already near $K$, and
$(N(t-1) < (1-\delta)K \ \text{and} \ N(t) < (1-\delta)K)$ is quickly
true even if the mortality is still weak. To ensure crisis ends after
real population loss, $v(t)$ activates when the population is lower than
its level at crisis entry. We store $N$ at crisis entry, and set when
$\phi = 2$ happens.

$$
v(t) = \max \Big( v(t-1) , \mathbb{1} \big[ N(t-1) < (1-\zeta)N_{entry} \ \text{and} \ N(t) < (1-\zeta)N_{entry} \big] \Big) 
$$

The following exit conditions $S_2$ are checked when $c(t) \geq n_2:$

$$
v(t) = 1 \ , \ \Delta^2 N(t-1) > \epsilon_2 \cdot K \ , \ \Delta^2 N(t) > \epsilon_2 \cdot K \ , \ \Delta N(t) <  \epsilon_1 \cdot K \ , \ N(t) < (1-\delta)K
$$

The population decline is decelerating as we near the depression phase.
Formally,
$(\Delta^2 N(t) > \epsilon_2 \cdot K \ , \ \Delta N(t) < \epsilon_1 \cdot K)$
signals that the population is approaching a trough — the point at which
it will reach its lowest level before stabilizing or rebounding.

- depression → expansion $S_3$

$N(t)$ alone is not enough to separate depression from expansion by
level : both phases have $N$ well below $K$ . We therefore use the
position of $C(t)$ relative to its baseline at the entry of a cycle as
exit conditions from depression.

$v(t)$ activates when $C(t)$ has passed its trough :

$$
v(t) = \max \Big(v(t-1), \mathbb{1}\big[C(t-1) \leq (1- \eta)C_0 \ \text{and} \ C(t) \leq (1- \eta)C_0 \big] \Big)
$$

The following exit conditions $C_3$ are checked when $c(t) \geq n_3:$

$$
v(t) = 1 \ ,\ \Delta C(t-1) > \epsilon_1 \cdot C_0 \ \text{and} \ \Delta C(t) > \epsilon_1 \cdot C_0 
$$

Before reaching its trough, capital undergoes a rapid decline during the
crisis, followed by a decelerating slide and a plateau; then a recovery
follows, marking the exit from depression into a new cycle. This
mechanism formalizes the Schumpeterian creative destruction of capital.

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
R_2 = \max \big(A(t)- W(t), 0\big)
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
secular cycle for a new expansion. Thus, the mortality sensitivity
$\kappa$ changes according to the population as well :
$\kappa_{exp} < \kappa_{stag} < \kappa_{depr} < \kappa_{crisis}$ .

$$
d_A(t) = d_{A,base} + d_{A, \phi(t)} 
$$

$$
d_E(t) = d_{E,base} + d_{E, \phi(t)}
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
workers, while the accessibility of symbolic pathways to the upper class
yields elite overproduction. Within a secular cycle, the reserve army
does not remain constant: when living conditions worsen, it declines
alongside the population, but it does not fully reset after the
depression, since the capital destruction triggered by the crisis also
depresses labor demand. The system thus persists with a sustainable
level of the reserve army. This persistence stems not only from
technological progress and the credential pathways built up during the
previous cycle, but also by the consolidation of the winning elite
faction. Indeed, Boyer (2015) underscores that the very stability of
such a coalition endogenously triggers the transition from growth to
crisis, affecting both the economic and political orders. Crucially,
because the crisis is a direct consequence of a stable ruling class
rather than an external failure, the structural integrity of the “same”
system remains uncontested when the crisis peaks. Exhausted by a
prolonged period of crisis and depression, the population is easily
pacified by the subsequent expansion and its attendant material
improvements; this transition leads to the false conclusion that the
system has finally achieved stability. In reality, the illusion of an
attainable equilibrium merely masks how the purge during the crisis and
depression phases operates as a systemic reset, recalibrating the
machinery of elite domination for a new cycle. Thus, expansion leads
once more to overpopulation, and the next phases follow in sequence —
stagflation, crisis, depression — as the reserve armies re-emerge. The
purges do not clear the market; they function instead as a biological
regulatory mechanism that perpetuates the secular cycle. Marx conceived
of crises as the terminal contradiction of capitalism — the inevitable
outcome of structural pressures. Yet the challenge they pose to the
status quo yields no reformed system, and capitalism does not resolve
its contradictions. Instead, it purges whenever the working class and
the upper class expand to the point of labor oversupply and elite
overproduction. The resulting “great reset” of both populations
temporarily improves living conditions during the early phase of
expansion. In this way, the system continually renews its own
legitimacy: a few elite aspirants secure positions, and the system
appears meritocratic; some workers find employment, and it appears
responsive; wages rise, and it appears self-correcting. As Lordon
(1997;1999) suggested, this illustrates the symbolic representation
required by the hegemonic bloc (a coalition of different social strata
unified under the leadership of the dominant class), to secure the
consent of the subaltern classes without coercion.
