Walrasian Labor Market - Notes
================

Walrasian Equilibrium : $L^s(w)=L^d(w)=L$

The market clears when the quantity of labor demanded by the firm
exactly equals to the quantity of labor supplied by the workers at a
specific market-clearing wage $w$ .

- $L^s(w)$ : Labor supply function (The workers)

- $L^d(w)$ : Labor demand function (The Firms)

Where :

$$
\frac{\partial L^s}{\partial w} > 0 , \frac{\partial L^d}{\partial w} < 0
$$

- When wage decreases, companies want to hire more (downward-sloping)
  -\> $\frac{\partial L^d}{\partial w} < 0$

- When wage increases, workers want to work more (upward-sloping) -\>
  $\frac{\partial L^s}{\partial w} > 0$

The firm sets the labor demand so that $w = MPL$ , where $MPL$ is the
**Marginal Product of Labor (MPL)** — the additional output produced by
one more unit of labor. When wage equals $MPL$, the firm has no
incentive to hire more or less.

Wage can be expressed as follow :

$$w=\frac{U'_{leisure}}{U'_{consumption}}$$

- $U'_{leisure}$ : Marginal Utility of Leisure

- $U'_{consumption}$ : Marginal Utility of Consumption

This ratio is called **Marginal Rate of Substitution (MRS)**.

The market requires both conditions simultaneously :

$$
w= MPL = \frac{U'_{leisure}}{U'_{consumption}}
$$

The workers and the firm optimize the wage by setting $w$ as $w = MRS$
and $w = MPL$ respectively. The market clears when $MRS = MPL$

- Workers optimize -\> wage = MRS

- Firms optimize -\> wage = MPL

- Market clears -\> MRS = MPL

In a pure Walrasian World, the reserve army and the elite overproduction
cannot mathematically exist, but in the actual economy, the markets do
not clear instantly because of frictions — skill mismatch, position
scarcity…

**Excess demand function** : The Walrasian tatonnement works through
excess demand

$$
ED(w) = L^d(w) - L^s(w)  
$$

- $ED>0$ : more demand than supply -\> wage rises

- $ED<0$ : more supply than demand -\> wage falls

- $ED = 0$ : market clears -\> equilibrium

The frictions are the mechanisms that keep $ED \neq 0$ persistently.

#### Tier 1 : standard reserve army

**Friction 1 : Automation**

Automation causes labor displacement that forces job-to-job transitions,
and since the skills of the displaced workers are considered obsolete,
it will result to a relatively prolonged unemployment to acquire new
skills for the existing positions . Thus, automation prevents the
equilibrium of the Walrasian model.

Automation is a capital-substitute for labor : firms choose to demand
less labor because the machines can do the job. So automation affects
the labor demand.

By introducing this friction to the $MPL$ , the wage will be expressed
with a time varying technology parameter $A(t)$ .

$MPL$ is a derivative of the Production function (Cobb-Douglas) :

$$
Y = A(t) \cdot K^{\alpha} \cdot L^{(1 - \alpha)}
$$

- $A$ : Time varying technology parameter

- $K$ : Capital

- $L$ : Labor

- $\alpha$ and $1 - \alpha$ : Distribution of the income between the
  capital and the Labor. Typically $\alpha = 0.3$ and $1-\alpha = 0.7$

For this project, the labor is splitted to automatable labor $L_a$ and
non-automatable labor $L_n$. $L_a$ is composed by the workers who
perform routine, codifiable tasks that machines can replace, while $L_n$
represents the workers who perform tasks that complement rather than
compete automation — creativity, judgment, social interaction, complex
problem solving. So the production function become :

$$
Y = A(t) \cdot K^{\alpha} \cdot \tilde{L}^{(1 - \alpha)} 
$$

with $\tilde{L}$ = $L_n + \theta(t) \cdot L_a$ . $\theta(t)$ is an
automation coefficient — how productive automatable workers are relative
to machines. When $\theta(t)$ falls, automation intensifies.

To determine $MPL$ , we take the derivative of $Y$ with respect to $L_a$
. Using the chain rule :

$$MPL = \frac{\partial Y}{\partial L_a} = \frac{\partial Y}{\partial \tilde{L} } \cdot \frac{\partial \tilde{L}}{\partial L_a }$$

$$\frac{\partial Y}{\partial \tilde{L} } = (1- \alpha) A(t) \cdot K^{\alpha} \cdot \tilde{L}^{- \alpha}$$

$$\frac{\partial {\tilde{L}}}{\partial L_a } = \theta (t)$$

$$MPL = w = (1- \alpha)\cdot \theta(t)\cdot \frac {A(t) \cdot K^{\alpha}}{(L_n + \theta(t) \cdot L_a)^{\alpha}}$$

Automation reduces the demand of automatable labor $L^d_a$ . To get
$L^d_a$ , we solve the equation above for $L_a$ as a function of the
wage $w$ :

$$
L^d_a = \frac{1}{\theta(t)} [(\frac{(1-\alpha) \cdot \theta(t)\cdot A(t) \cdot K^{\alpha}}{w})^{1/\alpha} - L_n]
$$

- **Verifying the slope condition**
  $\frac{\partial L^d_a}{\partial w} < 0$

$$
Y = (1-\alpha) ) \cdot \theta(t) \cdot A(t) \cdot K^{\alpha}
$$

$$
L^d_a = \frac{1}{\theta(t)} [\frac{Y}{w}]^{1/\alpha}
$$

$$
L^d_a = \frac{1}{\theta(t)} \cdot Y^{1/\alpha} \cdot w^{-1/\alpha}
$$

$$
\frac{\partial L^d_a}{\partial w} = \frac{-1}{\alpha} \cdot \frac{1}{\theta(t)} \cdot Y^{\frac{1}{\alpha}}  \cdot w^{\frac{-1-\alpha}{\alpha}} < 0
$$

-\> $\frac{\partial L^d_a}{\partial w}$ is indeed negative.

This Tier 1 friction is the excess labor supply, given as :

$$
R_1(t) = L^s_a(w) - L^d_a(w, \theta (t)) > 0
$$

- $R_1$ : reserve army of labor

- $L^s_a(w)$ : automatable labor supply

- $L^d_a(w, \theta(t))$ : automatable labor demand

Walrasian model requires $R_1(t)$ = 0, but when automation falls
$\theta(t)$ faster than wage $w$, the excess labor supply persists. —
The desired wage $w_1$ at which the workers are willing to work does not
match the wage $w_2$ at which the firms are willing to hire .

When automation arrives, a part of the works can be done by machines.
Thus, $w_2$ is adjusted by the firms as technology improves. Generally,
$w_2$ falls. But in real world, $w_1$ can not instantly falls to $w_2$,
because of the workers existing contracts, the minimum wage floor, the
workers may not refuse to accept lower wage. But most importantly, the
wage can not fall below what is needed to survive — **The subsistence
floor** $\bar{w}$

- **How** $\theta$ **affects the labor demand — Sign of**
  $\frac{\partial L^d_a}{\theta}$

$$
P = \frac{(1-\alpha) \cdot A(t) \cdot K^{\alpha}}{w}
$$

$$
L^d_a = \frac{1}{\theta(t)} \cdot P\cdot\theta(t)^{1/\alpha} - L_n
$$ $$
L^d_a  = \theta(t)^{\frac {1-\alpha}{\alpha}} \cdot P^{1/\alpha} - L_n
$$

$$
\frac{\partial L^d_a}{\partial \theta} = [(\frac {1-\alpha}{\alpha})\cdot P^{1/\alpha}\cdot \theta]^{1/\alpha} > 0
$$

-\> $\frac{\partial L^d_a}{\partial \theta}$ is positive. When
$\theta(t)$ falls, the labor demand $L^d_a$ falls. Intuitively, as
automatable workers become less productive relative to machine ($\theta$
falls), firms want fewer of them at any given wage.

With the wage stickiness and the swift technological progess,
$\theta(t)$ falls faster than the market-clearing wage $w$ . This yields
to a decreasing labor demand and a persistent excess of labor supply.

- **Equilibrium : MRS and MPL cross at wage** $w$ :

The Marginal Rate of Substitution represents the workers’optimization.
They choose how many hours to work by balancing the benefit of working —
earning wage to buy consumption goods, the cost of working — giving up
leisure time. Then, higher wage makes working more attractive than
leisure. Workers have reservation wage : below this wage, leisure time
is more valuable than consumption wage buys. Crucially, there is a
subsistence constraint — consumption can not fall below some minimum
$\bar{c}$ .

Friction occurs when minimum wage workers are willing to accept is lower
than what the firms accept to pay. The maximum wage the firms are
willing to pay is the Marginal Product of Labor. If the maximum demand
wage is below the minimum supply wage, no transaction is possible.

The minimum wage expressed in terms of utility function and minimum
consumption $\bar{c}$ :

$$
\bar{w} = \frac{U'_{leisure}}{U'_{consumption} |_{c = \bar{c}}}
$$

When the workers have more leisure time than working time, the
consumption is very low. At a subsistence level, each additional unit of
consumption is extremely valuable. Thus, $U'_{consumption}$ at $\bar{c}$
is a large denominator which makes $\bar{w}$ low.

#### Tier 2 : elite reserve army (elite overproduction)
