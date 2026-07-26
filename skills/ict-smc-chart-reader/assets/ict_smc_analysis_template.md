# ICT / SMC Chart Analysis Report

**Ticker/Symbol**: [Symbol Name]
**Chart Timeframe (entry)**: [e.g., 15m / 1H]
**HTF Timeframe (structure)**: [e.g., 4H / Daily]
**Analysis Date**: [YYYY-MM-DD]
**Analyst**: Claude ICT/SMC Chart Reader

---

## 1. Chart Overview

[Brief description of the instrument, current price context, and which
timeframe(s) were provided.]

---

## 2. Step 1 — HTF Trend Structure

- **Bias**: [BULLISH / BEARISH / NEUTRAL]
- **Structural swings supporting this bias**:
  1. [Swing description, e.g., "structural swing low, held above the prior
     swing low"]
  2. [Swing description]
  3. [Swing description]
- **Sequence type**: [HH/HL sequence / LH/LL sequence / overlapping-range]

---

## 3. Step 2 — BOS / CHoCH

- **Type**: [Bullish BOS / Bearish BOS / CHoCH detected / None visible]
- **Swing point broken**: [Description of the specific structural swing]
- **Confirmation quality**: [Clean closing displacement / marginal close]
- **Fair Value Gap left behind**: [Yes/No — description if relevant]

---

## 4. Step 3 — Inducement (Liquidity Sweep)

- **Status**: [SWEPT / NOT_YET_SWEPT / NO_INDUCEMENT_VISIBLE]
- **Liquidity pool description**: [Minor swing high/low, EQH/EQL — where it
  sits relative to the BOS point and the Breaker Block]
- **Sweep confirmation**: [Wick beyond the level, closed back inside —
  description]
- **Confidence impact**: [How the presence/absence of a clean sweep affects
  overall confidence]

---

## 5. Step 4 — Breaker Block Return

- **Breaker type**: [Bullish Breaker / Bearish Breaker]
- **Origin candle/block**: [Description — last opposing-direction candle(s)
  before the BOS impulse]
- **Origin swing broken?**: [Yes — cite the BOS from Step 2 / Not yet, so this
  is still an Order Block, not a Breaker]
- **Zone boundaries**: [High: ... / Low: ... — described relative to the
  origin candle(s), not rounded]
- **Reaction observed at zone**: [Rejection wick / lower-TF BOS inside the
  zone / no reaction yet]

---

## 6. Sequence Status

**Overall status**: [NOT_YET_FORMED / DEVELOPING / SEQUENCE_COMPLETE / INVALIDATED]

[One-paragraph synthesis of why the sequence has reached this status, citing
Steps 1-4 directly. If any stage is missing or out of order, state that
explicitly rather than forcing a complete-sequence read.]

---

## 7. Trade Idea (only if SEQUENCE_COMPLETE)

- **Direction**: [Long / Short]
- **Entry context**: [Reaction location inside the Breaker Block]
- **Stop Loss**: [Below the Breaker Block low (long) / Above the Breaker
  Block high (short) — described relative to the zone boundary from Step 5,
  with a volatility-sized buffer, not a fixed value]
- **Target / Draw on Liquidity**: [Next structural swing high/low or external
  liquidity pool, described relative to visible chart structure]
- **Invalidation**: [Closing break beyond the Breaker Block's far boundary —
  same level as the stop]

---

## 8. Confidence and Caveats

- **Confidence**: [High / Medium / Low]
- **Reasoning**: [What strengthens or weakens confidence — clean vs marginal
  BOS, presence/absence of inducement sweep, single vs multiple touches of
  the Breaker Block, HTF/LTF alignment]
- **What would change this read**: [Specific structural developments that
  would invalidate or upgrade the current assessment]

---

## 9. Disclaimer

This analysis is based purely on ICT/Smart Money Concepts market-structure
reading of the supplied chart image(s): higher-timeframe trend structure,
Break of Structure, inducement/liquidity sweeps, and Breaker Block reaction.
It does not consider fundamentals, news, or sentiment, and does not use any
static or predetermined support/resistance levels — every level above is
re-derived from the current chart. This is a probabilistic structural read,
not a prediction or investment recommendation.

---

**End of Analysis**
