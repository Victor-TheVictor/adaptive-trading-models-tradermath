# Project 1 design comparison

This is a **mathematical design comparison in Project 1**, not an observed ranking of which model earns more. All models use the same game rules, website validator, decision-time observations, posterior $P(V\mid I)$, fitted transition law, and Maker response model. They differ in *policy objective*. Any empirical comparison using a different information set for one model is invalid. A matched-seed, same-environment performance study is proposed for **Project 2, which has not started**.

| Property | Profit Maximization | Risk-Adjusted | Capital Preservation |
| --- | --- | --- | --- |
| Primary mathematical target | Maximize $E[B_4-500]$ | Maximize $E[B_4-500-\sum_r\lambda(s_r)L_r^2]$ | Lexicographically minimize $P(\min B_r<400)$, $P(B_4<500)$, $P(MDD>d_*)$; then maximize expected profit |
| Downside treatment | Through legal funding and future opportunity cost | Convex penalty on new drawdown; current drawdown changes calibrated λ | Direct priority on crossing floors and peak-drawdown threshold |
| Dynamic mechanism | Bellman continuation value changes with state | Precommitted bounded λ function updates at each observed state | Capital, peak and path flags change reachability of safety events; priority order stays fixed |
| Position size | Legal action with highest terminal mean | Legal action with highest mean less future downside cost; interior sizes possible | Legal action that preserves safety priorities, then highest mean within ties/tolerance |
| Maker quote | Maximize expected future profit under joint fills | Maximize profit less downside under joint fills | Minimize safety-event probabilities under joint fills |
| Primary evaluation | Mean final profit | Mean profit and new drawdown cost, with MDD/CVaR diagnostics | Frequency of floor breach, principal loss, and drawdown breach |
| Failure mode | Large tail loss | Penalty calibration misses opportunity or overreacts | Excessive skipping; forced Maker makes safety target infeasible |

## State-dependent behavior derived from objectives

| Observed situation | Profit Maximization | Risk-Adjusted | Capital Preservation |
| --- | --- | --- | --- |
| Balance rises above 500 | Legal size can grow; choose it only if terminal mean improves | More distance from 500 lowers the capital-deficit term in λ; downside still penalized | Principal-loss probability may fall, but a higher peak can raise future MDD breach risk |
| Balance falls below 500 | Reduced legal size and continuation value can change action | Capital-deficit and drawdown terms can raise λ after a settled loss | Already-below-500 does not force reckless recovery; compare probability of ending below 500 |
| Drawdown increases | Affect future value only through its correlation with capital/history | Larger $D$ increases λ if fitted $\theta_D>0$ | Peak-relative path event may already have occurred; compare remaining safety priorities |
| Balance approaches 400 | Legal size and future value shrink | Capital-deficit term raises λ if calibrated so | Probability of crossing 400 is first priority, including current-path history |
| High expected edge | Can justify larger legal size after accounting for future opportunities | Can outweigh convex downside penalty | Can be taken when it does not worsen higher-priority safety components beyond declared tolerance |
| High uncertainty | Alters action outcome distribution and expected continuation | Raises expected squared new drawdown for adverse outcomes | Raises chance of threshold breaches; effect depends on full distribution |
| Only one round remains | Terminal mean determines ranking | Terminal mean minus current drawdown penalty determines ranking | Exact terminal and path event probabilities determine ranking |

The table states conditional mathematical tendencies, not promises about every state. For example a large balance does not always increase Taker size when the quote has negative edge. A model can also make a market in *any* round equal to its seat, including round 1; a fixed “round 4 Maker” comparison would misclassify actual decisions.

## Shared metrics and units

Per game: final $B_4$, profit $B_4-500$ in points, initial-capital return $(B_4-500)/500$, peak $H_r$ in points, current $D_r$ and MDD as fractions, path breach $\min B_r<400$, terminal events $B_4<500$ and $B_4<400$, execution errors and penalties. Across games: mean and median profit, profit SD and downside deviation, mean MDD, empirical breach frequencies with confidence intervals, lower-tail quantiles and CVaR when supported by sample size. Compare recommendations with executed actions separately; realized P&L is not the model's expected P&L.

A historical alternative required $P(B_4<500)\le0.25$ and $P(B_4<400)\le0.05$. In this revision those limits are proposed research **reporting benchmarks**, not constraints silently shared by the three new models. Apply them uniformly as reported evaluation criteria unless a later design decision explicitly changes a model definition. Imposing them only on Profit Maximization would change the comparison.
