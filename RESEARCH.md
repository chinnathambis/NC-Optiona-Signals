# NC Option Signals — Research Log

Target: "NC - Option Signals" by niftyclubz (TradingView, invite-only, protected source)
URL: https://in.tradingview.com/script/tjL26eUn-NC-Option-Signals/
Phase: 4/5 — Recreation built (v0.1–v0.10); full description re-read 2026-10-15 → spec realignment done in v0.7 (see BUILD_QUEUE.md)

## Resource inventory

| # | Resource | Type | Answers | Reliability / limits |
|---|---|---|---|---|
| R1 | TradingView publish page — first read (summary) | Direct (author) | Building blocks | Summary only — superseded by R1b |
| R1b | TradingView publish page — FULL section-by-section read (2026-10-15 20:15 IST), incl. both release notes | Direct (author's own words) | Trigger logic, gates, cutoffs, statuses, levels lifespan, exhaustion, trail reminder, BSE rule | High for design intent; still no numeric thresholds (persistence count, cooldown, delta band, fit min, drift ceiling, min premium, exhaustion extreme) |
| R2 | Screenshot — NIFTY 01 SEP 2026 PUT 24200, 10m, Seller, 25 Aug (screenshots/R2_…) | Direct visual | Pills, levels, tags, dashboard | High; author-selected |
| R3 | Screenshot — NIFTY 01 SEP 2026 CALL 24200, 10m, Buyer, 25 Aug (screenshots/R3_…) | Direct visual | Same | Same |
| R4 | Screenshot — OLDER "NC - Option Sell", 28 JUL CE 23800, 10m | Direct visual, earlier version | Old dashboard numbers (Est. Delta 0.91, Adverse Drift 0.02 / 0.5) | Older logic |
| R5 | Photo — current Inputs dialog | Direct | Input list | High |
| R6 | Photo — ORIGINAL on 08 SEP CE 23900, 5m, Buyer, 1–4 Sep | Direct visual, owner's chart | Frequency, a loser, status text | High; photo |
| R7 | Mock screenshots (v0.1–v0.4) on CE 22900/22950/23000 and SENSEX | Our output | Mock behaviour | — |

## CONFIRMED by R1b (author's own description) — design facts
Engine & underlying
- All calcs on completed 10m bars; chart TF only controls visibility. 5m/10m: signal prints when engine bar completes. 15m: waits for chart bar close (≤10 min late). Other TFs: engine paused, status "Chart Timeframe Not Supported".
- Underlying from symbol: index options → cash index; stock options → stock; MCX options → front-month future. Specific index names tested before general ones (substring clash, e.g. NIFTY vs BANKNIFTY/FINNIFTY/MIDCPNIFTY).
- Rolling 30-bar window of completed 10m bars measures how premium RESPONDED to the underlying.
  - Est. delta = observed premium change per 1-pt underlying change (behavioural, not a model).
  - Adverse drift = premium change independent of the underlying, running against the trade direction, MEASURED AGAINST THE CONTRACT'S OWN VARIABILITY (normalised). Drift in your favour never blocks.
  - Underlying fit = how closely premium movement tracks the underlying.
  - EXCLUDED bar pairs: the pair spanning an overnight/holiday gap; post-3:15 PM bars on NSE/BSE (MCX not affected). → the regression is on bar-to-bar CHANGES, not levels.
- Warm-up: needs 30 completed 10m bars → status "Warming Up — No Signals Yet".
Gates
- Delta gate = a BAND (floor and ceiling; above ceiling = not credible, thin/erratic quotes).
- Adverse-drift gate = ceiling (direction flips with mode).
- Fit gate = minimum.
- Strike Quality: Good (all pass) / Weak (one fails) / Avoid (two+ fail).
Trigger (all on ONE completed 10m bar)
1. Momentum PERSISTENCE: MACD above signal (Buyer) / below (Seller) BEYOND THE FIRST BAR. Short side needs more persistence than long; MCX needs more again → sells arrive later by design.
2. RSI(14) vs its 14-period average agrees on the same bar.
3. All three gates pass AND 30-bar window full.
4. Underlying MACD supports the trade (description says 15m; release note: Buyer now on engine TF = 10m, Seller "slower").
5. BUYER ONLY: premium closes above its VWMA20 (release note: VWMA reference now OFFSET, not same-bar). SELLER HAS NO VWMA CONDITION.
6. A COOLDOWN since the last signal has elapsed AND this momentum run has not already fired.
- ONE signal per momentum run; latch releases when momentum turns over (MACD crosses back) and at the start of each new day.
- No distance test except the exhaustion filter.
Cutoffs & BSE
- NSE/BSE: no signals after 3:15 PM (closing auction 3:15–3:35). MCX exempt.
- SENSEX/BANKEX: the 15m directional check (condition 4) reads NIFTY, not SENSEX/BANKEX; regression + gates still use the true underlying.
Chart
- Signal pill = 10m bar close time; Entry pill next bar; repeat tag "2nd Signal - Be Cautious" (numbered); only the most recent tag shown, clears after two trading sessions.
- SL % 10–90 default 30; targets 25/40/60% + outer 100% (Buyer) / 90% (Seller). T1 ≈ risk → partial exit.
- Levels drawn once, run 150 MINUTES then end (not extended); cleared by a new signal and at start of each day.
- Shading light = momentum or RSI alone; stronger = both. Shading/STATUS show forming conditions only.
- Banners/watermarks for non-option charts, unsupported TF, unrecognised underlying, unsupported symbol, 15m lateness warning.
Dashboard
- INDIA VIX (value, change vs prev close; context only). SPOT (name, price, day change). TREND 30m (MACD+RSI agree) + 10m (MACD only), context only.
- STATUS strings: "Chart Timeframe Not Supported" (red) · "Warming Up — No Signals Yet" (blue) · "CAS - Index Frozen (3:15 PM)" (orange) · "Post - CAS — Avoid Entry" (grey) · "Signal Given — Wait for Next" (teal) · "Try Another Strike" (orange, a gate blocks) · "Trading in Buying / Selling Zone" (purple, both hold) · "Entering Buying / Selling Zone" (purple, one holds) · "Not in Buying / Selling Zone" (grey) · "Analysis Only — No Signals" (blue).
- Colour key: red fault, orange caution, blue info, teal already signalled, purple zone, grey rest/wind-down.
- Persistence count, 15m check and RSI have no status message.
Diagnostics panel: resolved underlying, each gate's state, Est. Delta / Adverse Drift / Underlying Fit readings.
Alerts: "Buy Signal" and "Sell Signal" on every chart regardless of Mode; message = direction, symbol, premium, stop, 4 targets, signal time; seven "Alert Data:" plots in Style tab.
Release notes
- (7 days before 15 Oct) underlying confirmation on engine TF; Buyer VWMA offset; EXHAUSTION FILTER: suspends signals when premium reaches a momentum extreme, resumes only after a meaningful retracement, both modes, resets each session; Seller min premium for NIFTY/BANKNIFTY/SENSEX (values not stated; Buyer, stock options, MCX unaffected); TRAIL REMINDER (on by default): once position moves in favour by a set amount, SL line shifts, changes colour, shows "review your stop" reminder — visual only, alerts keep original stop; repeat tags don't accumulate, clear after two sessions.
- (15 Oct) Seller uses slower underlying confirmation than Buyer; daily limit 3 buys / 2 sells per contract per session (beyond: not displayed, no alert); repeat-tag count fix.

## Still UNKNOWN (numbers not published)
Persistence bar counts (buyer / seller / MCX), cooldown length, delta band floor+ceiling, fit minimum, drift ceiling (old version showed 0.5), "momentum extreme" + "meaningful retracement" definitions, seller min premium values, trail "set amount" and where the line shifts to, VWMA offset size, seller "slower" TF.

## Observations from screenshots (kept)
- O1 pill = 10m close time; purple signal bar; Entry next bar. — High
- O2 Buy pills below, Sell above; caution tag opposite. — High
- O3 Levels = signal close × {0.70, 1.25, 1.40, 1.60, 2.00} (buyer), ×{1.30, 0.75, 0.60, 0.40, 0.10} (seller), rounded — 3 checks. — High
- O6 Buyer bought CE / Seller sold PE on a bullish day → direction-aware underlying check. — Medium
- O15 (R6) ~1 buy per session — consistent with one-signal-per-run latch + cooldown. — Medium
- O16 (R6) 3 Sep 09:45 buy after gap-up lost. — Medium
- O17 (R6) buy at ≈1.57× session low allowed. — Medium
- O19 (R7) Mock v0.1–v0.4 too few buys — partly because mock re-arms on any condition flip and uses level regression; superseded by R1b realignment (v0.7).

## Next action
Compare against the original on the same chart (CE 23000 Buyer / PE 23000 Seller); calibrate the provisional numbers from the original's diagnostics panel.
