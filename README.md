# southpawslugger-live-data

Public JSON output for [southpawslugger.co.uk](https://southpawslugger.co.uk)'s
live Today tab and Pitching tab.

This repository contains **only the JSON output of the model** — two files
that the site's browser-side JavaScript fetches. The model code, training
pipeline, methodology, and prompts live in a separate private repository.

## Files

### `baseline.json`

Written once per day at 07:47 UTC by the morning training job. Contains
per-game pre-game win probability and expected run totals for today's
MLB games (the active ET slate, which covers UTC yesterday + UTC today).

```json
{
  "generated_at": "2026-05-14T07:47:30Z",
  "date": "2026-05-14",
  "games": [
    {
      "game_pk": 123456,
      "home_team_id": 110,
      "away_team_id": 147,
      "pre_home_win_prob": 0.5476,
      "pre_pred_home_runs": 4.32,
      "pre_pred_away_runs": 4.05
    }
  ]
}
```

### `today_predictions.json`

Written by a live-poller every ~11 minutes during each day's actual game
window (start time and duration adjusted daily by Cloud Scheduler to fit
the day's slate). Contains:

- **Live linescore state** — current inning, outs, base state, score,
  current batter and pitcher names.
- **Live in-game win probability** per live game — computed by a Monte
  Carlo simulation that uses the morning baseline as its Poisson rate
  prior. See the
  [Methodology page §11](https://southpawslugger.co.uk/methodology/)
  for the math.
- **Posted lineups** when teams publish them (~3 hours pre-game).
- **Pitch-by-pitch data for each starting pitcher** — coordinates,
  type, velocity, outcome — for rendering the strike-zone chart on
  the [Pitching tab](https://southpawslugger.co.uk/pitching/).

```json
{
  "updated_at": "2026-05-14T22:34:00Z",
  "date": "2026-05-14",
  "live_game_count": 5,
  "games": [
    {
      "game_pk": 123456,
      "home_team_id": 110,
      "away_team_id": 147,
      "abstract_status": "Live",
      "detailed_status": "In Progress",
      "pre_home_win_prob": 0.5476,
      "live_home_win_prob": 0.612,
      "linescore": { "current_inning": 5, "outs": 1, "...": "..." },
      "lineups": { "home": [], "away": [] },
      "starting_pitchers": {
        "home": {
          "pitcher_id": 99999,
          "pitcher_name": "Sonny Gray",
          "pitch_count": 67,
          "pitches": [
            { "px": 0.31, "pz": 2.5, "type": "FF", "velocity": 96.2, "call": "S", "inning": 1 }
          ]
        },
        "away": { }
      }
    }
  ]
}
```

## What this is and isn't

Everything in this repo is the **output** of a forecasting model, not the
model itself. You can read the numbers but you cannot reconstruct the
underlying Bayesian model, training data, or feature engineering from
what's here. For the methodology see the
[Methodology page](https://southpawslugger.co.uk/methodology/).
