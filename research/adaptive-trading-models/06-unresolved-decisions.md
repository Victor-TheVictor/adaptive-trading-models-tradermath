# Design decisions and remaining research questions

The user selected the model definitions and project placement below. The remaining items are calibration choices or missing observations. A research threshold is not a game rule.

1. **Confirmed role rotation:** user selected Maker when round equals seat. Record the game version and screen evidence; do not use fixed-round-4 Maker language.
2. **Project placement:** Project 1 research is published in this research-only repository. The application, extensions, and experimental datasets remain in a separate private workspace. The old algorithm is archived in `research/legacy/`; the revision lives in `research/adaptive-trading-models/`.
3. **Card dealing:** independent ranks versus a 52-card deck, reset versus retention across rounds, and whether suits affect probabilities. Resolve from completed-game data and game rules.
4. **Legal validator:** precise long cash rule, short collateral upper bound, quote-price range, spread limits per setting, negative-balance handling, and penalty/timer rules. Until verified use the live validator as authoritative.
5. **Maker response:** enough quote variation and joint bot fill observations to fit and validate $P(V,Z\mid s,b,a)$? Until then no model-specific “optimal” Maker quote claim.
6. **Risk-Adjusted objective selected:** expected four-round profit minus a dynamic coefficient times squared new drawdown is the primary research model. Still choose λ function terms, coefficient bounds, fitting criterion, and test whether this risk cost predicts desired MDD/CVaR improvements. Compare simpler fixed and component variants first.
7. **Capital Preservation priority:** approve strict lexicographic ordering, calibrated $d_*$, and numerical tie tolerance; compare a tolerance-based safety set if strict ordering causes near-universal skipping.
8. **Threshold interpretation selected:** 500 is initial principal; 400 is a research risk threshold, not an asserted game-specific cutoff. The former 25%/5% loss-probability caps are candidate evaluation benchmarks, not automatic new-model rules.
9. **Policy class and replanning:** deterministic versus randomized policies; finite-state approximation; how ex ante risk commitments remain intact if a planned policy is recomputed mid-game.
10. **Project 2 controlled environment:** a matched-seed simulator and three-model comparison are proposed but have not started. The live website's seed mechanism and replay capability are unverified. Future work must choose a structural bot-response model, validate simulator-to-live behavior, keep exogenous inputs paired while recomputing action-dependent states, and freeze separate training/evaluation seeds.
11. **Evaluation design:** practical effect sizes, confidence-interval targets, model assignment schedule, treatment of non-adherence, and the minimum simulator validity needed before reporting a Project 2 winner.
12. **Oracle scope:** whether an Oracle knows current $V$ only, realized future exogenous outcomes, or counterfactual bot responses; choose and report each benchmark separately.

## Approval gate before software changes

These documents specify research. Changing the working Python decision engine, extension, importer, or Oracle implementation is a separate implementation step after the revised model definitions and unresolved choices are reviewed. The existing tool should not be labeled as executing these three policies while it still ranks one-round expected balance.
