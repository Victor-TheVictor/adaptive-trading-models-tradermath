# Revised research specification

## Objective, game, and information

Compare three mathematically different policies on the same Easy game and legal action set. A game has four rounds, three ranks per settlement, and initial balance $B_0=500$. **Provisional observed rule:** player in seat $p$ is Maker in round $p$, Taker in the other three rounds. This rule takes precedence over the unverified claim that round 4 is always Maker. Exact quote-price bounds, position collateral, card dealing, penalties, and end-of-round timing remain to be documented from the active game configuration.

At decision $r$, $s_r=(r,p,B_{r-1},H_{r-1},D_{r-1},I_r,\mathcal H_{r-1},\theta)$, where $H_{r-1}=\max_{0\le j<r}B_j$ is historical peak, $D_{r-1}=(H_{r-1}-B_{r-1})/H_{r-1}$ is current fractional drawdown, $I_r$ contains visible cards, observed Taker quote, available displayed balances and clock, and $\mathcal H_{r-1}$ contains *completed* prior rounds. $\theta$ is the frozen probability-model version. On Maker turns the candidate quote is an action, not a state feature. $B_r=B_{r-1}+\Delta_r$, $H_r=\max(H_{r-1},B_r)$, $D_r=(H_r-B_r)/H_r$, and $MDD=\max_{0\le j\le4}D_j$. With positive initial capital, these fractions are defined even if a later game balance is negative.

Available cash is current balance less any capital committed to still-open positions; this deduction is zero **only if** each round settles before the next decision. The actual validator determines legal size. This specification uses legal-action oracle $\mathcal A(s_r)$ rather than presenting provisional caps as confirmed rules. Profit in points is $B_4-B_0$; return on initial capital is $(B_4-B_0)/B_0$. Intraround unrealized P&L and exposure are recorded only if the game actually exposes persistent positions.

## Shared probability and payoff kernel

For visible ranks $c_1,\dots,c_k$, let $S=\sum c_i$, $h=3-k$, $V=S+\sum_{j=1}^{h}X_j$. The baseline rank prior is independent discrete uniform on $\{2,\dots,14\}$, yielding $P(V=v\mid I_r)$ by convolution, $E[V]=S+8h$, and $\mathrm{Var}(V)=14h$. The without-replacement deck and quote-conditioned posterior are *alternative models requiring held-out validation*, not extra information granted to one strategy. All three policies share the same selected posterior and model version in a comparison.

For Taker quote $(b,a)$ and size $q$, $\Delta^{buy}=q(V-a)$, $\Delta^{sell}=q(b-V)$, and $\Delta^{skip}=0$. For Maker quote $(b,a)$ and three bots' joint fills $Z$, $\Delta^{maker}=\sum_i[N_i^+(a-V)+N_i^-(V-b)]$. Candidate Maker actions require $P(V,Z\mid s_r,b,a)$; observing fills at one quote cannot identify fills under another. If no credible response model exists, Maker quote *optimization* is unavailable and all three policies must use the same predeclared exploration or baseline quote rule. A modeled quote never becomes an observed fill.

The transition kernel $\widehat P(s_{r+1}\mid s_r,a_r)$ combines current settlement, maker fills where applicable, next public cards/quote, bankroll, peak, and history. It is fitted from completed training games and frozen before evaluation. A policy $\pi$ maps decision-time states to legal actions. Enumerating a candidate action and summing over unknown outcomes is valid; substituting the later revealed $V$ during live decisions is leakage.

## Three objectives

**Profit Maximization:** $\max_{\pi}\ E_\pi[B_4-B_0]$ over legal policies. No arbitrary risk penalty is inserted; bankroll limits and future opportunity cost can still make a smaller position optimal.

**Risk-Adjusted:** maximize $E_\pi[B_4-B_0-\sum_r\lambda(s_r)L_r^2]$, where $L_r=H_{r-1}(D_r-D_{r-1})_+$ is *new drawdown* in points. Squaring penalizes larger downside moves more strongly and permits interior position sizes in the final round. $\lambda(s)$ has units of inverse points and is a bounded, precommitted function of observed state that changes after each settlement. The user selected this as the primary new risk objective; it is not an original formula from the old algorithm. Its parameters require calibration; CVaR remains a separate evaluation diagnostic to avoid counting the same tail twice.

**Capital Preservation:** lexicographically minimize $(P_\pi(\min_{1\le r\le4}B_r<400),\ P_\pi(B_4<500),\ P_\pi(MDD>d_*),\ -E_\pi[B_4-B_0])$. The user confirmed that 400 is a research risk threshold, not a verified game rule. This prioritizes avoiding that line and losing initial capital, then peak drawdown, then profit among equally protective policies. $d_*$ is a predeclared, calibrated threshold. The strict lexicographic rule is a research proposal; it may trade infrequently and must be compared with a tolerance-based alternative on held-out games.

All probabilities are **ex ante session** quantities under a common transition model. A conditional probability displayed after reaching round 3 is not the same as a session-start risk probability. If planning is recomputed mid-session, the frozen policy or state-contingent budget from the initial plan must be preserved; resetting ex ante caps after a loss changes the objective.

## Decision procedure

The research target is to fix the model, probability version, calibration, seat, and evaluation/training assignment before play; verify each public observation; evaluate legal actions under a shared fitted continuation law; and record recommendation and executed action separately. After settlement, append revealed ranks and realized outcomes as labels. Fit new model versions only between training sessions.

The current desk approximates this procedure with a finite Taker action grid, uncalibrated simulated future states, and a shared baseline Maker quote; it does not enumerate all website-legal policies or use a fitted bot-response law. The normal Page Reader automatically captures face-up ranks when the read succeeds; the player remains responsible for executing trades and checking the game validator.
