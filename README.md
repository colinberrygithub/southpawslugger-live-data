# southpawslugger-live-data

Public JSON output for [southpawslugger.co.uk](https://southpawslugger.co.uk)'s
live Today tab.

This repository contains **only the JSON output of the model** — two files
that the site's browser-side JavaScript fetches. The model code, training
pipeline, methodology, and prompts live in a separate private repository.

## Files

- `baseline.json` — written once per day at 07:47 UTC by the morning
  training job. Per-game pre-game win probability and expected run totals
  for today's MLB games.
- `today_predictions.json` — written every 11 minutes during the
  20:00-06:00 UTC game window. Live linescore state plus per-game
  in-game win probability from a Monte Carlo simulation seeded by
  the morning baseline.

## What this is and isn't

Everything in this repo is the **output** of a forecasting model, not the
model itself. You can read the numbers but you cannot reconstruct the
underlying Bayesian model, training data, or feature engineering from
what's here. For the methodology see the
[Methodology page](https://southpawslugger.co.uk/methodology/).
