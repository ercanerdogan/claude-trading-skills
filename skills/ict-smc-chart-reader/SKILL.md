---
name: ict-smc-chart-reader
description: This skill should be used when analyzing a chart screenshot using ICT (Inner Circle Trader) / Smart Money Concepts methodology. Use this skill when the user provides a chart image and asks for market structure, Break of Structure (BOS), liquidity/inducement, or Breaker Block analysis, or requests an SMC-style entry sequence with a dynamically-derived stop loss. Always evaluates levels dynamically from the current chart; never uses static or predetermined support/resistance levels.
---

# ICT/SMC Chart Reader

## Overview

This skill enables systematic analysis of chart screenshots using ICT
(Inner Circle Trader) / Smart Money Concepts (SMC) methodology. It reads
higher-timeframe (HTF) trend structure, confirms a Break of Structure (BOS),
identifies the inducement (liquidity sweep trap) that follows it, and checks
whether price has returned to react at the resulting Breaker Block — the
exact four-stage sequence smart-money-concept traders use to validate an
entry. Every level used in the analysis (swing points, BOS trigger,
liquidity pools, Breaker Block boundaries, stop loss, target) is re-derived
dynamically from the supplied chart; this skill never relies on static or
predetermined support/resistance lines.

## When to Use

Use this skill when:
- User provides a chart screenshot and asks for ICT or Smart Money Concepts
  analysis
- User asks whether a Break of Structure (BOS) or Change of Character (CHoCH)
  has occurred
- User asks about liquidity sweeps, inducement, equal highs/lows (EQH/EQL),
  or stop hunts on a chart
- User asks about Order Blocks or Breaker Blocks and whether price has
  returned to react at one
- User wants an SMC-style entry idea with a structurally-derived stop loss
  and target (not a static support/resistance level)

Do NOT use this skill when:
- User wants classic technical analysis (trendlines, moving averages, chart
  patterns, volume) without an ICT/SMC framing — use `technical-analyst`
  instead
- User wants market breadth or sector rotation analysis — use
  `breadth-chart-analyst` or `sector-analyst` instead
- User asks for a static support/resistance level call — this skill
  deliberately refuses fixed S/R and will redirect to structural, dynamic
  levels instead

## Prerequisites

- **Chart Image Required**: User must provide a chart screenshot. This skill
  cannot run on a ticker symbol alone — it reads price structure directly
  from the image.
- **No API Keys Required**: Pure image-based visual analysis; no external
  data fetches.
- **HTF Context Helpful**: If the user can identify which timeframe is HTF
  relative to their intended entry timeframe (e.g., "4H is my HTF, I enter on
  15m"), state it up front. If not stated, infer a reasonable HTF/entry-TF
  relationship from what's visible in the chart and state that assumption
  explicitly in the report.

## Output

This skill generates a markdown analysis report saved to the `reports/`
directory:
- **File format**: `ict_smc_analysis_[SYMBOL]_[YYYY-MM-DD].md`
- **Content**: HTF trend structure, BOS/CHoCH read, inducement/liquidity
  sweep status, Breaker Block zone and reaction, overall sequence status, and
  (when the sequence is complete) a trade idea with a dynamically-derived
  stop loss and target.

## Core Principles

1. **Dynamic-Only Levels**: Never use a static, predetermined, or round-number
   support/resistance level. Every level is re-derived from the current
   chart's visible swing structure at analysis time — see
   `references/ict_smc_methodology.md` Section 0.
2. **Strict Sequence Order**: HTF trend → BOS → inducement → Breaker Block
   return, checked in that order. Do not skip a stage or infer a later stage
   without evidence for the earlier ones.
3. **Closes, Not Wicks, Confirm Structure**: BOS and CHoCH require a closing
   break. A wick-only poke through a level is treated as a candidate
   liquidity sweep, not a structure break.
4. **Breaker ≠ Order Block**: A Breaker Block requires that its origin swing
   has already been broken. An untested origin swing means the zone is still
   an Order Block, not yet a Breaker.
5. **State Uncertainty Explicitly**: If a stage is ambiguous, missing, or not
   yet confirmed, say so and lower confidence rather than forcing a complete
   read.

## Analysis Workflow

### Step 0: Load the ICT/SMC Methodology

Before analyzing any chart, read the full methodology and definitions:

```
Read: references/ict_smc_methodology.md
```

This reference contains the exact definitions used by every step below:
swing points, HH/HL vs LH/LL, BOS vs CHoCH, inducement/liquidity sweeps,
Order Block vs Breaker Block, the full validated sequence, dynamic stop
loss/target placement, and common errors to avoid.

### Step 1: Receive the Chart and Establish Context

1. Confirm receipt of the chart image(s).
2. Identify the instrument and the timeframe(s) shown.
3. Establish which timeframe is HTF (structure) and which is the intended
   entry timeframe. Use what the user states; otherwise infer from the chart
   and state the assumption explicitly.
4. Note any specific question the user asked (e.g., "is this BOS valid?",
   "where would my stop go?").

### Step 2: Determine HTF Trend Structure (HH/HL or LH/LL)

Following `references/ict_smc_methodology.md` Section 2:

1. Identify the structural swing highs and lows on the HTF, working from the
   most recent price action backward.
2. Classify the sequence: `BULLISH` (HH/HL), `BEARISH` (LH/LL), or `NEUTRAL`
   (overlapping range).
3. Cite the specific swings that justify the classification. Never state a
   bias without pointing to the swings behind it.

### Step 3: Confirm BOS (Break of Structure)

Following `references/ict_smc_methodology.md` Section 3:

1. Identify the most recent structural swing point in the direction of the
   HTF bias.
2. Check whether a candle has **closed** beyond it (bullish BOS above a
   swing high in an HH/HL sequence; bearish BOS below a swing low in a
   LH/LL sequence).
3. If the break is against the prevailing structure instead, flag it as a
   CHoCH and re-run Step 2 with the post-CHoCH swings before proceeding.
4. Note whether the break was a clean displacement or a marginal close, and
   whether it left a Fair Value Gap behind.
5. If no closing break exists yet, classify sequence status as
   `NOT_YET_FORMED` and stop here.

### Step 4: Identify Inducement (Liquidity Sweep)

Following `references/ict_smc_methodology.md` Section 4:

1. Examine the pullback that follows the BOS.
2. Look for a minor swing high/low, or equal highs/lows (EQH/EQL), sitting
   between the BOS point and the likely Breaker Block.
3. Confirm a sweep: a wick trades through that minor level and closes back
   on the other side (a stop run).
4. Classify as `SWEPT`, `NOT_YET_SWEPT`, or `NO_INDUCEMENT_VISIBLE`. If no
   inducement is visible, explicitly lower confidence rather than omitting
   the check — the absence of a sweep is itself a data point.

### Step 5: Check the Breaker Block Return

Following `references/ict_smc_methodology.md` Section 5:

1. Identify the last opposing-direction candle (or block of candles)
   immediately preceding the impulsive leg that produced the BOS from
   Step 3.
2. Confirm that leg broke the candle's origin structural swing — this is
   what makes it a Breaker Block rather than an untested Order Block.
3. Define the zone's boundaries as the high and low of that origin
   candle/block.
4. Check whether price has returned to trade inside that zone, and whether
   it reacted there (rejection wick, or a lower-timeframe BOS confirming the
   reaction).

### Step 6: Synthesize Sequence Status

Following `references/ict_smc_methodology.md` Section 6, classify the overall
sequence as one of:

- `NOT_YET_FORMED` — no BOS yet.
- `DEVELOPING` — BOS confirmed, but inducement not yet swept or price hasn't
  reached the Breaker Block.
- `SEQUENCE_COMPLETE` — HTF bias → BOS → inducement swept → visible reaction
  inside the Breaker Block, all present and in order.
- `INVALIDATED` — price has closed fully through the Breaker Block's far
  boundary without reacting. Re-derive structure from the current chart
  rather than mechanically searching for the "next" Breaker.

### Step 7: Develop the Trade Idea (only if SEQUENCE_COMPLETE)

Following `references/ict_smc_methodology.md` Section 7:

1. **Stop loss**: below the Breaker Block's low for a long, above its high
   for a short, with a buffer sized qualitatively to the instrument's visible
   volatility — never a fixed universal value.
2. **Target**: the next structural swing high/low or external liquidity pool
   in the trade's direction, described relative to visible chart structure.
3. **Invalidation**: a closing break beyond the Breaker Block's far
   boundary — the same level as the stop.
4. State a confidence level (High/Medium/Low) based on: BOS clarity, whether
   inducement was cleanly swept, whether the Breaker Block reaction is
   confirmed or still forming, and HTF/entry-TF alignment.

If the sequence is not complete, do not produce a trade idea — state which
stage is missing and what would need to happen for the sequence to
progress.

### Step 8: Generate the Analysis Report

Create a markdown report using the template structure:

```
Read and use as template: assets/ict_smc_analysis_template.md
```

**File Naming Convention**: Save as
`ict_smc_analysis_[SYMBOL]_[YYYY-MM-DD].md` in the `reports/` directory
(create it if it does not exist).

### Step 9: Quality Assurance

Before finalizing the report, verify:

1. ✓ Every level cited (swings, BOS trigger, liquidity pool, Breaker Block
   boundaries, stop, target) traces back to a specific feature of the
   current chart — no static or round-number levels.
2. ✓ BOS/CHoCH calls are based on a closing break, not a wick.
3. ✓ The Breaker Block's origin swing was confirmed broken before labeling
   it a Breaker (not an untested Order Block).
4. ✓ The inducement check was performed explicitly, even when the result is
   `NO_INDUCEMENT_VISIBLE`.
5. ✓ Sequence status accurately reflects which of the four stages are
   actually present, in order.
6. ✓ A trade idea is only given when `SEQUENCE_COMPLETE`.
7. ✓ Stop loss is placed beyond the Breaker Block boundary, never at a static
   S/R level.
8. ✓ Confidence level and its reasoning are stated explicitly.

## Common Analysis Errors and How to Avoid Them

See `references/ict_smc_methodology.md` Section 8 for the full list
(wick-as-BOS, Order Block/Breaker Block confusion, skipping the inducement
check, anchoring to stale/round-number levels, declaring the sequence
complete prematurely, and treating CHoCH as BOS). Review it before finalizing
any report.

## Example Usage Scenarios

**Example 1: Full Sequence Check**
```
User: "Check this EUR/USD 1H chart for an ICT setup, 4H is my HTF."
[Provides chart image]

Analyst:
1. Reads ict_smc_methodology.md
2. HTF (4H) structure: HH/HL → BULLISH
3. BOS: closing break above prior structural swing high → confirmed, clean displacement
4. Inducement: minor swing low swept on the pullback → SWEPT
5. Breaker Block: bullish breaker (last down-close block before BOS impulse),
   origin swing broken, price returned and printed a rejection wick inside the zone
6. Sequence status: SEQUENCE_COMPLETE
7. Trade idea: Long, stop below breaker low, target at next structural swing high
8. Saves ict_smc_analysis_EURUSD_2026-07-26.md
```

**Example 2: Developing Setup, No Trade Idea Yet**
```
User: "Is this a valid BOS on Bitcoin's 4H chart?"
[Provides chart image]

Analyst:
1. Reads ict_smc_methodology.md
2. HTF structure: LH/LL → BEARISH
3. BOS: closing break below prior structural swing low → confirmed
4. Inducement: pullback still forming, no minor swing/EQH swept yet → NOT_YET_SWEPT
5. Breaker Block: identified (bearish breaker candidate) but price hasn't returned to it
6. Sequence status: DEVELOPING
7. No trade idea given; report states what to watch for (inducement sweep, then
   reaction at the breaker zone) before the setup would qualify
```

**Example 3: Invalidated Breaker**
```
User: "Did my breaker block hold on this Nasdaq chart?"
[Provides chart image]

Analyst:
1. Reads ict_smc_methodology.md
2. Confirms prior HTF bias, BOS, and inducement sweep all still check out
3. Breaker Block: price returned, but closed fully through the zone's far
   boundary without reacting
4. Sequence status: INVALIDATED
5. Report explicitly discards the failed zone and re-derives structure from
   the current chart rather than searching for a replacement mechanically
```

## Resources

This skill includes the following bundled resources:

### references/ict_smc_methodology.md

Comprehensive methodology covering:
- The dynamic-only assessment principle (no static S/R, ever)
- Swing point identification (fractal definition)
- HTF trend structure (HH/HL vs LH/LL vs range)
- BOS vs CHoCH (closing break requirement)
- Inducement / liquidity sweep identification (EQH/EQL, minor swings)
- Order Block vs Breaker Block distinction and boundary definition
- The full validated 4-stage sequence and its status classifications
- Dynamic stop loss / target placement rules
- Common analysis errors and how to avoid them

**Usage**: Read this file before conducting any ICT/SMC chart analysis to
ensure consistent, dynamic, sequence-correct interpretation.

### assets/ict_smc_analysis_template.md

Structured template for ICT/SMC analysis reports, covering HTF structure,
BOS/CHoCH, inducement, Breaker Block return, sequence status, trade idea (if
applicable), and confidence/caveats.

**Usage**: Use this template structure for every analysis report.

## Special Notes

### Why This Skill Refuses Static Support/Resistance

Static horizontal levels (round numbers, previously eyeballed lines) do not
account for how market structure evolves bar-by-bar. ICT/SMC methodology
treats every level as a function of the most recent swing structure,
liquidity pools, and Breaker Block boundaries — all re-derived at analysis
time. If a user asks for "the support level," redirect them to the dynamic,
structure-derived levels this skill produces instead, and explain why a
fixed level would not be reliable.

### Practical Application

Every analysis should answer:
- **Structure**: What is the HTF bias, and what swings support it?
- **Confirmation**: Has a BOS actually occurred, by a closing break?
- **Trap**: Has the inducement liquidity been swept, or is it still resting?
- **Entry Zone**: Has price returned to the Breaker Block, and did it react?
- **Risk**: Where does the stop go (Breaker Block boundary), and what
  invalidates the idea?
