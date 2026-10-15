# NC Option Signals — Build Queue

Build only when the owner says "build". Current code: nc_option_signals.pine (v0.10). Older builds in archive/.
Indicator name on chart: "NC Option Signals" (renamed from "NC Mock" at owner's request, v0.10). Version shown in the diagnostics panel first row and in the file header. Keep it as a private/personal script — don't publish under the original author's name.
Owner wants to go step by step.

## Versions
- v0.1 — first mock. Verified compiles.
- v0.2 — exhaustion 2.5×; blocked-by counts; STATUS "Signal Given". Verified.
- v0.3 — Data Window "DW" plots. Verified.
- v0.4 — BSX → BSE:SENSEX; "Underlying not found"; header left-aligned. Verified.
- v0.5 — option B loosening. Superseded.
- v0.6 — I1 outcome tracking + OUTCOMES table. Superseded.
- v0.7 — SPEC REALIGNMENT S1–S15 (full rewrite). FULLY VERIFIED: 5m, 10m, 15m, SENSEX, Seller.
- v0.8 — trail label on opposite side; OUTCOMES win rate + median. VERIFIED.
- v0.9 — B (I3 + costs, measurement only): scored from actual entry (next bar open), gap fills at open, net of brokerage/slippage/charges; lot size auto (NIFTY 65, BANKNIFTY 30, FINNIFTY 60, MIDCPNIFTY 120, SENSEX 20). NOT yet compile-verified.
- v0.10 (2026-10-15) — rename only: indicator("NC Option Signals"), diagnostics row "NC Option Signals v0.10". Logic = v0.9. NOT yet compile-verified.
  Provisional numbers (unchanged): delta band 0.05–1.20, fit ≥ 0.30, drift ≤ 0.50, persistence buyer 2 / seller 3 / MCX +1, cooldown 3 bars, exhaustion RSI ≥80 → ≤60 (buyer), ≤20 → ≥40 (seller), min premium NIFTY 15 / BANKNIFTY 40 / SENSEX 40, trail at T1 → SL line to signal premium.

## Results (signal-premium reference, hold to SL/T4/15:15, NO costs)
| Contract / TF / mode | Signals | ≥T1 | ≥T2 | ≥T3 | T4 | SL before T1 | 15:15 exits | Replaced | Avg | Win rate | Median |
|---|---|---|---|---|---|---|---|---|---|---|---|
| SENSEX 15 OCT CE 72200, 5m, Buyer (delayed, expiry) | 8 | 63% | 50% | 25% | 13% | 0% | 63% | 2 | +34.7% | – | – |
| NIFTY 19 OCT CE 23000, 5m, Buyer (v0.8) | 15 | 27% | 20% | 13% | 13% | 7% | 53% | 4 | +10.8% | 60% | +0.8% |
| NIFTY 19 OCT CE 23000, 10m, Buyer | 15 | 27% | 20% | 13% | 13% | 13% | 47% | 4 | +10.4% | – | – |
| NIFTY 19 OCT CE 23000, 15m, Buyer | 15 | 27% | 27% | 13% | 13% | 13% | 47% | 4 | +11.8% | – | – |
| NIFTY 19 OCT PE 23000, 10m, Seller | 16 | 25% | 13% | 0% | 0% | 6% | 69% | 4 | +8.0% | – | – |
| NIFTY 19 OCT PE 23000, 5m, Seller (v0.8) | 16 | 25% | 6% | 0% | 0% | 6% | 69% | 4 | +7.5% | 63% | +4.4% |
- Buyer: median +0.8% vs avg +10.8% → typical buy flat; profit from 2 T4 runs. Seller: median +4.4%, win 63%, no T3/T4, unlimited risk.
- Small samples (15–16 trades, ~1 week, one expiry).

## Next
- Load v0.10 on CE 23000 (Buyer) and PE 23000 (Seller), 5m; record NET avg / win / median / cost.
- A. Accuracy: compare with the ORIGINAL on the same charts; calibrate [P] numbers from its diagnostics panel.

## Optional
- 15m Entry pill overlaps signal pill — original R3 shows the same overlap.

## Still unknown
Persistence counts, cooldown, delta band, fit min, drift ceiling & units, exhaustion extreme/retracement, seller min premium values, trail amount + where the line moves, VWMA offset size, seller slower TF, BANKEX option root and lot size.

## Improvements BEYOND the original (step by step; toggles)
- I1 ✅ outcome tracking (+ win rate/median v0.8). I3 ✅ scoring from actual entry + costs (v0.9).
- I2 Strategy-tester version. I4 Opening filter. I5 Expiry-day awareness. I6 Liquidity filter. I7 India VIX direction. I8 No re-entry after SL. I9 Volatility-scaled levels. Seller naked-risk warning.
