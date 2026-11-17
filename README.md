# Adaptive Trading Models in the TraderMath Trading Game

**Project 1 — Model and algorithm design**  
Victor Chen · TraderMath Market Games / Trading Game / Easy

This research project develops three quantitative decision models for a four-round trading game: **Profit Maximization**, **Risk-Adjusted**, and **Capital Preservation**. Each model uses the same decision-time information but has a different objective and approach to risk. The [research collection](research/README.md) contains assumptions, derivations, model flowcharts, limitations, and the original study. The [complete algorithm design](research/adaptive-trading-models/TraderMath_Adaptive_Algorithm.md) explains the common framework and all three models.

The separate [illustrated Trading Tool guide](docs/trading-tool-guide.md) explains how to use the desk, interpret its metrics, and read three real-round examples.

## Information integrity and discovery

While building a trading assistant, unusually accurate apparent predictions prompted an investigation of the game's page. One recorded round showed that a card displayed face down had its rank and suit in a front-image element in the browser DOM before a quote was submitted. The post-game summary confirmed the same card. The [investigation](research/adaptive-trading-models/07-data-integrity-and-information-leakage-prevention.md) contains the decision-time and settlement screenshots, a short HTML evidence excerpt, the normal tool's information boundary, and the separation of the experimental Oracle Lab from normal model evaluation. The screenshots document an observation; they do not establish the current state of the third-party website.

The normal tool is designed to capture face-up ranks automatically while excluding hidden ranks from live decisions. It depends on the site's rendering and requires continued real-game validation. The experimental Oracle Lab intentionally uses exposed current-card metadata and is not a fair-information model. The project does not claim to have repaired the third-party site.

## From exploration to Project 2

In a few exploratory Oracle Lab games, profitability appeared sensitive to particular card draws and opponent behavior, including maker bid and ask quotes. These informal observations do not quantify causal effects or establish a long-run winner. Because unmatched games can differ in cards, quotes, fills, and bankroll trajectories, they do not provide a controlled model comparison.

**Project 2 is proposed and has not started.** It will build and validate a controlled environment to compare all three frozen models on matched scenarios and to study partial information versus known current-round hidden cards. Action-dependent fills and bankrolls must be recomputed when strategies differ. Any future conclusions will depend on simulation validity and uncertainty.

## Implementation status and scope

The Python decision tool provides finite-grid, four-round rollout approximations under editable, uncalibrated assumptions about future quotes and bot behavior. It is not an exact solution of the research policies; its outputs do not guarantee performance. This public repository presents the research and selected evidence. The application source, extensions and experimental dataset are maintained privately.

This is an independent personal research project, not affiliated with or endorsed by TraderMath.
