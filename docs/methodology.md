# Methodology

> Documentation only. This page describes what the model does at the level of the published
> literature. It does not contain source code, weights, calibration anchors, or the ingestion
> pipeline, and those are not going to be published.
>
> It does contain the limits. Read those before the method — they are the reason this page exists.

---

## 1. What the model produces

For a match that has not kicked off, the model produces:

- a probability for each of the three outcomes (home win / draw / away win);
- an expected goals figure for each side;
- a ranked short list of likely score lines;
- a **risk score** — see §4;
- a **confidence** value, which is a ranking score and not a calibrated probability. See §5.3.

All of it is written to a **pre-match snapshot** at least 60 minutes before kick-off, and the
snapshot is what the open ledger scores. See [`open-ledger.md`](open-ledger.md).

## 2. Six paths, fused

The model is not one model. Six paths each produce a view of the same match, and the views are
combined with a fixed per-mode weight vector.

| Path | What it looks at | Literature it comes from |
|---|---|---|
| **1. Consensus pricing** | Implied probabilities recovered from how a match is priced across multiple sources, with the built-in margin removed | Shin (1992, 1993); Štrumbelj (2014) on Shin vs. simple normalisation |
| **2. Recent results** | Form over a recent window for both sides, and head-to-head history | Standard; Elo-family and rating-update literature |
| **3. Goal rates** | A goal-scoring model producing a score-line distribution, which is then collapsed to win/draw/loss | Maher (1982); Dixon & Coles (1997) |
| **4. Dispersion** | How much the sources in path 1 disagree with each other | Treated as an uncertainty signal, not a directional one |
| **5. Cross-source agreement** | Whether two structurally different ways of quoting the same match imply the same thing | — |
| **6. Divergence index** | How far the path-1 probability sits from the model's own, expressed through a standard log-optimal growth formula | Kelly (1956), used **only** as an internal feature and a risk flag |

Two things worth being explicit about, because they are the first questions a reviewer asks:

- **Path 1 dominates.** In the mode the ledger runs on, the consensus-pricing path carries by some
  distance the largest weight. The remaining five paths are corrections to it. Anyone assuming the
  model is mostly a repackaging of consensus pricing is closer to right than wrong, and §5.1 says so
  in numbers.
- **Path 6 is never surfaced as a recommendation.** It is a feature and a risk flag. We do not
  publish it, we do not act on it, and we do not tell anyone what to do with it.

### 2.1 De-vigging with Shin

Prices carry a built-in margin, so the implied probabilities across three outcomes sum to more than
one. The naive fix is to divide through by the sum. Shin's model instead assumes a fraction *z* of
volume comes from better-informed participants and solves for the underlying probabilities given
that assumption. We solve for *z* numerically per match and fall back to simple normalisation when
the solve fails.

Why it matters to a reviewer: simple normalisation systematically distorts long-shot outcomes, which
in football means the draw and the weaker side. Shin is the standard correction in the literature and
is one of the few places where our choice is defensible on grounds other than "it scored better".

### 2.2 Poisson with a Dixon–Coles correction

Path 3 estimates a scoring rate for each side, builds the score-line matrix under a Poisson
assumption, and sums the cells into win/draw/loss. Plain independent Poisson is known to
under-predict low-scoring draws — 0–0 and 1–1 specifically — and Dixon & Coles (1997) correct the
four lowest-score cells with a dependence parameter ρ. We apply that correction with a ρ that varies
by match context rather than a single global constant.

The score-line list on a match page comes from this matrix, not from a separate model.

## 3. Modes

Six preset weight vectors exist (they differ in how much they lean on the pricing path versus the
statistical paths, and in which corrections are on). They are a user-facing feature.

**The ledger is locked to one mode — `balanced` — and stays there.** If the record could be reported
from whichever of six modes looked best that week, it would not be a record. A change of mode or of
model starts a new ledger; the old one stays published. See [`open-ledger.md`](open-ledger.md) §6.

In practice the six modes agree with each other on the top-ranked outcome in the large majority of
matches, so treating them as six independent opinions would be misleading. We don't.

## 4. The risk score

The risk score summarises how much the six paths disagree about a match, together with how dispersed
the sources within path 1 are. High risk means the inputs are inconsistent, not that an upset is
coming.

What it is **for**: telling you when to trust the match page less. What it is **not** for: telling
you to do anything. There is no action attached to it anywhere in the product.

## 5. Limits

This is the section we would want to read first if we were you.

### 5.1 We are about level with the simplest baseline

Across the 473 matches on our public record (27 Aug – 27 Sep 2026), the model's top call was right
274 times — 57.9%. Over the same matches, always siding with the pre-match favourite would have been
right 272 times — 57.5%. That is a gap of 0.4 percentage points, which is nothing. So we won't sell
you an accuracy number. We'll publish every call before kick-off and let you count.

Where the two do separate is at the top of the confidence range: when the model's confidence is 65 or
above, 88 of 110 came in. That is one band, on one sample, and it is the only part of the record we
would ask anyone to look at twice — see §5.5 on why even that is not yet resolvable.

That sentence is the whole commercial position of this product, and we would rather you heard it from
us than worked it out from our own ledger in week three.

### 5.2 The model barely predicts draws

The top-ranked outcome is a draw in a very small share of matches, while roughly a fifth of real
matches end level. Under the ledger's definition of "right" (§ [`open-ledger.md`](open-ledger.md) §3),
**draws are therefore recorded as misses nearly every time**. This is a structural property of a
model whose dominant path is consensus pricing — the draw is rarely the most likely single outcome
even when it is a live possibility — not a bug we intend to fix by tuning.

It is also why we report *N of M*, and why we publish a favourite-baseline column next to every
figure: both the model and the baseline are penalised identically by draws, which is the only way the
comparison stays honest.

### 5.3 Confidence is a ranking score, not a probability

The `confidence` value is built from how peaked the probability distribution is and how much the
paths agree, then mapped through a piecewise function. It sorts matches sensibly. It has **not** been
demonstrated to mean "this will be right *c* of the time".

Historically, high-confidence matches were mostly heavy favourites, where simply backing the
favourite scored about the same. Any figure of the form "when confidence was above X, we were right
Y% of the time" is therefore uninformative unless the favourite baseline is printed next to it — so
we will only ever publish it as a full band table, with the low bands included, and only once the
forward ledger has enough matches in the high band to say anything at all.

### 5.4 Our historical numbers are not clean, so we are not using them

Predictions in our historical database were recomputed after matches finished. Some of those
recomputations pulled in form data that included the match being predicted, or matches played after
it. That is data leakage, and it inflates any accuracy figure derived from that period.

We are not going to publish a cleaned-up version of that history and ask you to trust the cleaning.
The open ledger starts from a fixed date, contains only snapshots written before kick-off, and is the
only record we will make claims from. Everything before it is, for public purposes, discarded.

If you see an accuracy figure for this product dated before the ledger start date — including in
older marketing copy of ours — treat it as withdrawn.

### 5.5 The sample will be small for a long time

At a few hundred scored matches, the confidence interval on a win-rate difference is wide enough to
swallow any plausible real edge. Concretely: at around 300 matches, a 95% interval spans roughly
±5–6 percentage points. Most of the distinctions people want us to make — better than the baseline?
better than last month? — are not resolvable at that size and will not be for months.

We would rather publish "small sample, we keep counting" for a year than publish a number that
collapses when someone runs the arithmetic.

### 5.6 A negative note on the obvious use

We have backtested the obvious naive use of this output and the result was negative. We are not
going to describe that use, promote it, or help with it. It is stated here because pretending we had
never checked would be the dishonest option.

### 5.7 Coverage

Our data comes from a commercial feed and has the shape that feed has. In particular:

- there are no Malaysian or Philippine domestic league matches on the site today, and we are not
  going to pretend otherwise;
- the ledger's phase-1 scope is Premier League and UEFA Champions League only — see
  [`open-ledger.md`](open-ledger.md) §1;
- club names come to us in a non-English source language and are mapped for display, which is a
  reliable source of small errors. Corrections are welcome — see
  [`../COLLABORATION.md`](../COLLABORATION.md) §1.

### 5.8 What "traceable" means, precisely

It means every figure on a match page can be expanded to the rows it was computed from: which
fixtures counted as recent form, which injuries were counted, what window a rate was taken over.

It does **not** mean we show you the weights, the calibration mapping, or the code. Those are not
published. If your review needs to distinguish "the inputs are wrong" from "the fusion is wrong",
say so in an issue and we will answer the specific question in writing.

## 6. The measure we actually care about

Not win rate. **Calibration**: when the model says 70%, does it happen about 70% of the time?

Calibration does not require beating anyone. It is the claim a probabilistic forecast can honestly
make, it is the one our own documentation can support, and it is measurable from the public ledger by
anyone who wants to check it. A calibration report is on the roadmap for the first period with enough
matches to compute one that isn't noise.

If you want to argue that our calibration claim is also overreaching at this sample size — please do.
[`../COLLABORATION.md`](../COLLABORATION.md) §3.

## References

- Dixon, M.J. & Coles, S.G. (1997). *Modelling Association Football Scores and Inefficiencies in the
  Football Betting Market.* Journal of the Royal Statistical Society: Series C, 46(2), 265–280.
- Maher, M.J. (1982). *Modelling association football scores.* Statistica Neerlandica, 36(3), 109–118.
- Shin, H.S. (1993). *Measuring the Incidence of Insider Trading in a Market for State-Contingent
  Claims.* The Economic Journal, 103(420), 1141–1153.
- Štrumbelj, E. (2014). *On determining probability forecasts from betting odds.* International
  Journal of Forecasting, 30(4), 934–943.
- Kelly, J.L. (1956). *A New Interpretation of Information Rate.* Bell System Technical Journal,
  35(4), 917–926.
- Brier, G.W. (1950). *Verification of forecasts expressed in terms of probability.* Monthly Weather
  Review, 78(1), 1–3.

## Disclaimer

Football data for information only. We don't take bets, don't link to any betting operator, and
don't give betting advice. 18+.
