# TraderMath research collection

**Project 1:** Adaptive Trading Models in the TraderMath Trading Game  
**Author:** Victor Chen  
**Scope:** Trader Math → Market Games → Trading Game → Easy

This directory collects the original research, the historical two-mode algorithm, the revised three-model algorithm, and the supporting audit and experiment design. The [revised main algorithm](adaptive-trading-models/TraderMath_Adaptive_Algorithm.md) is the **self-contained Project 1** model-design document. It declares assumptions and information boundaries, derives the three policies, and provides a workflow diagram for each. **Project 2 has not started**; its proposed role is a controlled same-environment comparison of the three frozen models.

## Current algorithm and model designs

| Document | What it contains | Status |
| --- | --- | --- |
| [Adaptive trading algorithm](adaptive-trading-models/TraderMath_Adaptive_Algorithm.md) | Self-contained declaration and assumptions, introduction, definitions, derivations, algorithm logic, three flowcharts, complexity, limitations, future Project 2, and notation | Current Project 1 research specification |
| [Profit Maximization](adaptive-trading-models/models/01-profit-maximization.md) | Four-round expected terminal-profit objective and workflow | Four-round finite-grid simulation implemented; exact policy and calibration pending |
| [Risk-Adjusted](adaptive-trading-models/models/02-risk-adjusted.md) | Dynamic squared-new-drawdown objective, coefficient design, and workflow | Four-round risk-adjusted simulation implemented; calibration pending |
| [Capital Preservation](adaptive-trading-models/models/03-capital-preservation.md) | Lexicographic 400/500/path-risk priorities and workflow | Frozen finite-rule simulation implemented; calibration pending |

## Supporting research

| Document | What it contains |
| --- | --- |
| [Research landing page](adaptive-trading-models/README.md) | Reading path and current status |
| [Existing-research audit](adaptive-trading-models/01-existing-research-audit.md) | Old formulas, research claims, and live-code coverage |
| [Shared revised specification](adaptive-trading-models/02-revised-research-specification.md) | Common game state, information boundary, probability/payoff kernel, and objectives |
| [Model comparison](adaptive-trading-models/04-comparative-framework.md) | Side-by-side objectives, state-dependent behavior, and metrics |
| [Experimental validation](adaptive-trading-models/05-experimental-validation.md) | Decision snapshots, train/holdout split, outcomes, and offline Oracle definitions |
| [Data integrity and information leakage prevention](adaptive-trading-models/07-data-integrity-and-information-leakage-prevention.md) | Evidence, visible-card capture, Oracle Lab separation, and proposed Project 2 information comparisons |
| [Design decisions and remaining questions](adaptive-trading-models/06-unresolved-decisions.md) | Confirmed choices, unknown game mechanics, and calibration work |

## Historical research

| Document | What it contains |
| --- | --- |
| [Original TraderMath research PDF](<legacy/A Quantitative Decision Framework for Trader Math Market Games - TraderMath Research.pdf>) | Earlier research manuscript supplied for Project 1 |
| [Historical TraderMath algorithm](legacy/TraderMath_Algorithm.mdx) | Earlier Profit/Extreme design and its original flowchart; preserved as a baseline |

The Python desk now provides finite-grid, four-round simulation approximations for all three objectives. Its assumptions are uncalibrated and its finite continuation rules are not the exact research policies. It does not estimate a validated joint bot-response model or calculate the proposed offline Oracles.
