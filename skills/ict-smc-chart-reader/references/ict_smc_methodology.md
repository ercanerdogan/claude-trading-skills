# ICT / Smart Money Concepts Methodology

This reference is the authoritative definition of every term and rule used by
the `ict-smc-chart-reader` skill. All four analysis stages (HTF trend, BOS,
inducement, Breaker Block return) must use these exact definitions so that
repeated analyses of the same chart stay internally consistent.

## 0. The Governing Principle: Dynamic-Only Assessment

**No level in this methodology is ever fixed.** Every swing high/low, BOS
trigger, liquidity pool, Breaker Block boundary, stop loss, and target is
re-derived from the price structure **visible in the current chart at the
time of analysis**.

- Never reuse a level identified in a prior analysis of the same instrument as
  if it were a permanent support/resistance line. Market structure evolves
  every time new bars print; a swing high that mattered last week may be
  irrelevant (or already invalidated) today.
- Never anchor to round numbers, psychological levels, or previously-drawn
  horizontal lines. If a level coincides with a round number, that is a
  coincidence to note, not a reason to trust it.
- Every time this skill runs, re-derive structure from scratch using only what
  is visible in the supplied chart image(s). Treat the prior report (if any)
  as historical narrative, not as a source of current levels.
- If the user references "the support at X" from a previous conversation,
  re-verify it against the current chart before using it. If price structure
  has moved on, say so explicitly and discard the stale level.

## 1. Swing Points (the building block of everything else)

A **swing high** is a bar (or cluster of bars on the visible timeframe) whose
high is greater than the highs of the bars immediately before and after it —
a local peak. A **swing low** is the mirror: a local trough whose low is
lower than the bars on either side.

- Use a fractal definition (typically 3-bar, sometimes 5-bar for noisy
  charts): the middle bar's extreme must not be exceeded by its immediate
  neighbors on both sides.
- Always re-identify swings from the rightmost, most recent price action
  first, then work backward only far enough to establish the structural
  sequence needed for the current read (see Sections 2-4). Do not import
  swings from memory.
- Minor (lower-significance) swings and major (structural) swings coexist on
  the same chart. Structural swings are the ones that define HH/HL or LH/LL
  sequences (Section 2). Minor swings are candidates for inducement
  (Section 4).

## 2. HTF Trend Structure (HH/HL vs LH/LL)

Determine directional bias from the sequence of structural swing points on
the higher timeframe (HTF) visible in the chart, or the timeframe the user
identifies as HTF relative to their intended entry timeframe.

- **Bullish structure**: a sequence of **Higher Highs (HH)** and **Higher
  Lows (HL)** — each new structural swing high exceeds the prior one, and each
  new structural swing low holds above the prior one.
- **Bearish structure**: a sequence of **Lower Highs (LH)** and **Lower Lows
  (LL)** — the mirror image.
- **Range / consolidation**: swing highs and swing lows overlap with no clean
  directional sequence. Classify as `NEUTRAL` and lower confidence in the
  final assessment — do not force a bias onto a ranging chart.
- Report the HTF bias as `BULLISH`, `BEARISH`, or `NEUTRAL`, and cite the
  specific swing points (described by their position in the chart, e.g. "the
  swing low printed after the prior HH, which held above the swing low before
  it") that support the call. Never state a bias without pointing to the
  swings that justify it.

## 3. BOS (Break of Structure) vs CHoCH (Change of Character)

- **BOS (continuation)**: a **closing** break beyond the most recent
  structural swing point **in the direction of the prevailing HTF bias**.
  - Bullish BOS: a candle closes above the most recent structural swing high
    in an HH/HL sequence.
  - Bearish BOS: a candle closes below the most recent structural swing low
    in an LH/LL sequence.
- **CHoCH (reversal)**: the **first** closing break of a swing point **against**
  the prevailing structure — e.g., in an LH/LL downtrend, a close above the
  most recent lower high. A CHoCH signals the trend may be changing and
  should be flagged separately; it does not feed the bullish/bearish BOS →
  inducement → Breaker sequence below until a new HTF bias has been
  re-established from the post-CHoCH swings.
- **Wicks are not BOS.** A wick that pokes through a swing point and closes
  back inside it is not a structure break — it is very often the inducement
  sweep described in Section 4, not confirmation of continuation.
- Always identify BOS by the **close**, state which specific swing point was
  broken, and note whether the break was a clean displacement (strong-bodied
  candle, often with a Fair Value Gap left behind) or a marginal close — a
  marginal close deserves lower confidence.

## 4. Inducement (the liquidity sweep trap)

Inducement is a **minor** liquidity pool — a relative swing high/low, equal
highs (EQH), or equal lows (EQL) — sitting between the BOS point and the
Breaker Block, engineered to trap traders who react to it as if it were the
real level.

- **Why it exists**: smart money needs opposing liquidity (stop orders resting
  above minor highs / below minor lows) to fill large orders before price
  reverses toward the Breaker Block. Retail traders who buy a minor higher-low
  or short a minor lower-high are "induced" into a position that gets swept
  out.
- **How to identify it on the chart**:
  1. After a BOS, look at the pullback that follows.
  2. Find the first minor swing point that forms during that pullback — often
     a smaller-degree high/low, or two nearly-equal highs/lows (EQH/EQL),
     usually on a lower or intermediate timeframe than the structural swing
     that produced the BOS.
  3. Confirm a sweep: a wick trades through that minor level and closes back
     on the other side of it (a stop run), rather than a clean structural
     break.
- **Dynamic confidence check**: if price travels from the BOS point straight
  to the Breaker Block **without** sweeping any visible minor liquidity along
  the way, treat the setup as lower-confidence — the absence of an inducement
  sweep is itself informative and must be stated in the report, not silently
  skipped.
- Report the inducement as `SWEPT`, `NOT_YET_SWEPT`, or `NO_INDUCEMENT_VISIBLE`,
  with the specific minor level referenced.

## 5. Order Block vs Breaker Block (do not conflate these)

- **Order Block (OB)**: the last opposing-direction candle (or small cluster
  of candles) immediately preceding an impulsive move, **before** its origin
  swing has been broken. An OB is "fresh"/untested — its defining swing has
  not yet failed.
- **Breaker Block**: an Order Block whose **origin structural swing has
  already been broken** (i.e., a BOS or liquidity sweep has traded beyond that
  swing). Breaking the origin swing "breaks" the block and flips its
  polarity:
  - **Bullish Breaker**: the last **down-close** candle (or block of
    down-close candles) immediately before the impulsive leg that produced a
    **bullish BOS**. Once that impulsive leg has broken the prior structural
    swing high, this down-close block — previously a supply/resistance
    zone — flips into a **demand/support** zone on any later return visit.
  - **Bearish Breaker**: the mirror — the last **up-close** candle/block
    immediately before the impulsive leg that produced a **bearish BOS**.
    Once the prior structural swing low is broken, this up-close block flips
    into a **supply/resistance** zone on any later return visit.
- A Breaker Block only becomes actionable once price actually **returns** to
  trade back into that block's high-low range. Until that return happens, it
  is a zone to watch, not a zone to act on.
- Define each Breaker Block's boundaries as the **high and low of the
  origin candle/cluster** — these two prices bound the zone; do not round
  them to nearby levels.

## 6. The Full Validated Sequence

**Bullish sequence** (mirror everything for bearish):

1. HTF structure = HH/HL → `BULLISH` bias.
2. Bullish BOS confirmed by a closing break above the prior structural swing
   high.
3. Price pulls back and **sweeps an inducement** liquidity pool (a minor
   swing low or EQL) below the pullback — trapping late shorts / running
   stops placed just under that minor low.
4. Price then rallies back into the **bullish Breaker Block** (the down-close
   block that preceded the BOS impulse) and reacts there — look for a
   rejection wick or a lower-timeframe BOS confirming the reaction inside the
   zone.

Classify overall sequence status as one of:

- `NOT_YET_FORMED` — HTF bias identified, but no BOS yet.
- `DEVELOPING` — BOS confirmed, inducement not yet swept, price has not
  reached the Breaker Block.
- `SEQUENCE_COMPLETE` — all four stages present in order: HTF bias → BOS →
  inducement swept → price trading inside/reacting at the Breaker Block.
- `INVALIDATED` — price has **closed fully through** the Breaker Block
  (beyond its far boundary) without reacting. The block has failed a second
  time; discard it and re-derive structure from the current chart rather than
  searching for the "next" Breaker Block mechanically.

Do not skip stages. If price reaches the Breaker Block zone without any
identifiable inducement sweep beforehand, say so explicitly and lower
confidence rather than declaring `SEQUENCE_COMPLETE`.

## 7. Stop Loss and Target — Dynamic Placement Only

- **Stop loss**:
  - Long (bullish Breaker): place the stop **below the low of the Breaker
    Block**, with a small buffer sized to the instrument's visible
    volatility on the chart (describe qualitatively — "a few ticks/points
    beyond the block's low, sized to recent bar ranges" — never state a fixed
    universal tick/pip count, since that depends on the instrument and
    timeframe).
  - Short (bearish Breaker): place the stop **above the high of the Breaker
    Block**, with the same volatility-sized buffer.
  - **Invalidation = stop loss.** A closing break beyond the Breaker Block's
    far boundary invalidates the setup; this is the same level as the stop.
- **Target**: the next opposing structural level in the trade's direction —
  typically the next structural swing high (for longs) or swing low (for
  shorts), or the next visible external liquidity pool (equal highs/lows,
  a prior BOS origin swing). State the target relative to visible chart
  structure ("the swing high that produced the most recent bearish reaction"),
  never as a fixed price detached from structure.
- Never suggest a static support/resistance-based stop or target. Every
  number must trace back to a swing point, Breaker Block boundary, or
  liquidity pool identified in Sections 1-6.

## 8. Common Errors and How to Avoid Them

### Error 1: Calling a wick-poke a BOS

**Symptom**: Report claims "bullish BOS" when the candle only wicked above
the swing high and closed back below it.

**Prevention**: BOS requires a **closing** break. A wick-only poke is
frequently the inducement sweep itself (Section 4), not the BOS.

### Error 2: Confusing Order Block with Breaker Block

**Symptom**: Report labels a fresh, untested order block as a "Breaker
Block."

**Prevention**: A Breaker Block requires that its **origin swing has already
been broken**. If the origin swing is still intact, it is an Order Block, not
a Breaker — do not use it for the return-and-react entry logic in Section 6.

### Error 3: Skipping the inducement check

**Symptom**: Report jumps from "BOS confirmed" straight to "price is at the
Breaker Block" without addressing whether any liquidity was swept in
between.

**Prevention**: Explicitly search the pullback between the BOS and the
Breaker Block for a minor swing/EQH/EQL sweep. If none is visible, state
`NO_INDUCEMENT_VISIBLE` and reduce confidence rather than omitting the check.

### Error 4: Anchoring to a stale or round-number level

**Symptom**: Report references "support at 4200" or reuses a level from a
prior analysis without re-checking current structure.

**Prevention**: Re-derive every level from the current chart (Section 0).
Round numbers and prior levels are never a substitute for re-verified swing
structure.

### Error 5: Declaring `SEQUENCE_COMPLETE` prematurely

**Symptom**: Report calls the sequence complete while price is still
approaching the Breaker Block, or before any reaction has occurred inside the
zone.

**Prevention**: `SEQUENCE_COMPLETE` requires visible price reaction (rejection
wick or lower-timeframe BOS) **inside** the Breaker Block range, not merely
proximity to it.

### Error 6: Treating CHoCH as BOS

**Symptom**: Report treats the first break against the prevailing trend as a
continuation BOS.

**Prevention**: A break against the prevailing structure is a CHoCH
(Section 3) — flag it as a possible trend change and require a new HTF bias
determination before applying the bullish/bearish sequence in Section 6.
