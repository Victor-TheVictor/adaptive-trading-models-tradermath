# Adaptive Trading Models in the TraderMath Trading Game

**Project 1 — Model and Algorithm Design.** Research revision for the four-round Easy Trading Game, by Victor Chen. This is a mathematical specification for three policies: Profit Maximization, Risk-Adjusted, and Capital Preservation. The Python tool provides finite-grid four-round simulation approximations, with uncalibrated future quotes and bot responses; it does not implement the exact research policies.

Start with the [complete revised algorithm](TraderMath_Adaptive_Algorithm.md). It is self-contained and follows the earlier research format while adding an assumption/declaration section, three model flowcharts, a notation table, and explicit limitations and future improvements. Continue with [the audit](01-existing-research-audit.md), [common framework](02-revised-research-specification.md), [individual model specifications](models/), [design comparison](04-comparative-framework.md), [validation plan](05-experimental-validation.md), and [design decisions](06-unresolved-decisions.md). The [research collection](../README.md) indexes the full Project 1 record. The historical algorithm may be archived or removed without changing the current model definitions.

**Selected model definitions:** Risk-Adjusted uses expected four-round profit minus a state-dependent coefficient times squared new drawdown. Capital Preservation treats 400 points as a research risk threshold, below the 500-point initial principal. The desk approximates these user-selected objectives under its stated simulation assumptions; the threshold is not a verified game rule.

**Information integrity:** [Follow the discovery, screenshot evidence, normal-reader safeguards, Oracle Lab and future information comparison](07-data-integrity-and-information-leakage-prevention.md).

**Status:** research specification. Remaining game mechanics, transition probabilities, bot response, and parameter values marked *unverified* require observation or held-out calibration before operational claims.

**Future Project 2:** a separate, not-yet-started empirical comparison would configure one validated seeded environment and run all three frozen policies on matched scenarios. Project 1 does not claim that the live website offers seed control or that any model has already won a controlled comparison.
