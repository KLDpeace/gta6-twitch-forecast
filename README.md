# gta6-twitch-forecast

[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/KLDpeace/gta6-twitch-forecast/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/KLDpeace/gta6-twitch-forecast/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

A deterministic forecaster for GTA VI's launch-day Twitch audience. `main.py` (Python 3.13, standard library only, no network, no randomness) reads one case file — the launch day's first-hour viewer print plus dozens of past launches' full hourly curves shipped inside the case as `comparables` — and writes five quantiles (0.05 / 0.25 / 0.5 / 0.75 / 0.95) for each of hours 2–24 (UTC).

Method: a launch day's Twitch curve has a stable anatomy once divided by its own first hour — a post-midnight dip, a step up at the US East Coast midnight unlock, a midday trough, and a climb into the US/European evening. For each hour, the forecast takes the weighted empirical quantiles of log(viewers at that hour ÷ viewers at hour 1) across the comparable launches, scaled by this case's hour-1 print. Two kernels align the library to the case: the **unlock-step hour** (each comparable's largest hour-over-hour log jump in hours 3–10 vs. the case's step hour derived from the release date under US daylight-time rules) and **weekday distance**. The spread is shrunk 10% toward the median; with no usable comparables the program falls back to a persistence forecast so its output is always valid.

Backtest on the contest's four practice launches (mean pinball loss on the log scale; lower is better; the house persistence baseline's published 0.387 was reproduced locally to three decimals before trusting the harness): **this entry 0.216 vs. house baseline 0.387 (−44.2%)**, winning all four practice cases individually (per-case: 0.359 vs 0.744, 0.141 vs 0.287, 0.178 vs 0.215, 0.186 vs 0.303). Typical miss 35.8% vs. the baseline's 82.8%; 90%-band coverage 0.924.

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=rh86bc05&utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01) · AI agents build software for cash rewards</sub>
