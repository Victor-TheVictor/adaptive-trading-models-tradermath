# Existing research audit

## Sources and status key

**Old research:** `TraderMath_Algorithm.mdx`, sections 1–7, and its `paper/research-paper.qmd` counterpart. **Prototype:** `src/tradermath_bot/{cards,taker,maker}.py`, `app.py`, `browser_extension/{content,popup}.js`, and `static/recorder.js` in `tradermath-easy-assistant`. **Status:** implemented, partial, research only, or unverified. The old paper and MDX describe the same two-mode research design; they are not evidence that code implements it.

## Mechanics and formulas

| Formula or rule | Meaning, assumptions, purpose, source | Status and limitation |
| --- | --- | --- |
| $B_0=500$, $B_r=B_{r-1}+\Delta_r$, $r=1,\ldots,4$ | Score accounting in old §2; realized P&L is added after each round. | Implemented in the tool's current-round display and completed-game reconciliation. Penalty and timing effects still require direct observation. |
| Maker when $r=p$; otherwise Taker | Old §1–2; $p$ is the player's seat. This matches screenshots with Maker in rounds 1, 2, and 4. | Implemented in Python and popup. The revision prompt's fixed-round-4 Maker assumption conflicts with this evidence. |
| $V=S+\sum_{j=1}^{3-k}X_j$, $L=S+2(3-k)$, $U^{vis}=S+14(3-k)$ | $k$ visible ranks, $S$ their sum; old §2. Hidden ranks range 2–14. | Implemented for the selected card model. Card-dealing mechanism unverified. |
| $P(V=v\mid H)=13^{-h}\sum_{x_1,\dots,x_h=2}^{14}\mathbf1\{S+\sum x_j=v\}$; $\mu=S+8h$, $\sigma^2=14h$ | Old §3.1; independent uniform ranks is a provisional prior. Full distribution matters for tail outcomes. | Implemented in `cards.py`. A second rank-deck-without-replacement option exists, but suits and cross-round depletion are not tracked. Neither model has been validated against held-out dealt cards. |
| $P(V\mid H,Q,\mathcal H)\propto P(Q\mid V,H,\mathcal H)P(V\mid H,\mathcal H)$ | Old §3.1; quote carries information only if a trained likelihood improves held-out prediction. | Research only. Prototype ignores quote as information about $V$. |
| $\Delta^{buy}=q(V-a)$, $\Delta^{sell}=q(b-V)$, $\Delta^{skip}=0$ | Old §3.2; $b,a$ are another Maker's quote. $E[\Delta]$ equals $q(\mu-a)$ or $q(b-\mu)$; payoff variance is $q^2\sigma^2$. | Payoff and outcome enumeration implemented. The current `taker.py` ranks by *current-round* expected balance in Profit mode, not expected $B_4$. |
| $q^{buy}_{max}=\lfloor B/a\rfloor$; $q^{sell}_{max}=\lfloor B/\max(1,U-b)\rfloor$ | Old §3.2; proposed legal caps, with $U=42$ or visible-support upper bound. | Partial. Code uses the visible-state support bound. The site's exact validator and collateral accounting are unverified; legal sizes must follow the live validator. |
| $G=N^+(a-V)+N^-(V-b)$ | Old §3.3; all three bots share one $V$. $N^+$ buy from us; $N^-$ sell to us. | Realized payoff and a model *interface* implemented. No fitted joint response distribution or optimal quote selection. Maker page shows an exploratory spread-2/4/6 grid. |
| $E[G]=E[N^+](a-\mu)+E[N^-](\mu-b)+\mathrm{Cov}(N^--N^+,V)$ | Old §3.3; covariance captures possible adverse selection conditional on public information and candidate quote. | Research only. Adverse selection is a testable hypothesis, not proof that bots know hidden cards. |
| Old Profit: maximize $E[B_4-500]$ subject to $P(B_4<500)\le .25$, $P(B_4<400)\le .05$ | Old §3.4; proposed *ex ante* whole-session caps, with lexicographic fallback if infeasible. | Research only. Prototype displays conditional one-round probabilities; it does not enforce either session cap. The thresholds are proposals, not game rules. |
| Old Extreme: maximize $P(B_4\ge T)$ for a target fixed before play | Old §3.4; target-hit dynamic program on a fitted four-round transition law. | Partial: code ranks current-round target probabilities only. No full-session dynamic program. The revision replaces this mode as a research objective. |

## Present decision process and data boundary

The extension attempts to read round, seat, balance, displayed quote, and visible ranks. The server uses the chosen card distribution to enumerate legal Taker sizes, calculates a payoff distribution for each, and ranks by conditional expected balance. The maker assistant displays settlement support, mean, SD, an exploratory quote grid, and realized P&L once actual fills are entered. It does not predict the joint bot fills. A browser-local completed-game recorder stores revealed ranks separately from ranks available at decision time and checks totals. The extension and recorder are fallible; a claimed successful read is not evidence of correctness without comparing the page and captured values. Earlier failures misclassified face-up cards as hidden.

The live decision must use only public, decision-time information. Hidden rank values in page source, a later settlement page, and an offline Oracle are outside that information set. An Oracle result stored after import must never become a training feature for the live policy.

## Supported limitations

1. The operational ranker uses one-round expected balance. It has no calibrated four-round transition law, so it cannot honor the old full-session chance constraints or account for future positions.
2. The independence prior and alternative rank-deck prior are assumptions. Neither proves the site's dealing rule or whether cards are retained between rounds.
3. The live short collateral cap remains provisional. A mathematically attractive quantity can be rejected by the website.
4. Maker expected return depends on counterfactual bot responses to candidate quotes. Only observed realized fills and a protocol are present.
5. The recorder has outcomes and visible counts but does not yet provide a validated, session-level training set with model versions, recommendation-versus-execution, timing/penalties, or robust uncertainty intervals.
6. The old MDX describes training/held-out separation but supplies no fitted model or measured performance. Any superiority claim is currently a hypothesis.
