# The open ledger — rules

> Documentation only. These are the rules the ledger runs on. They are published **before** the
> ledger starts, on purpose, so that changing them later is visible.
>
> Start date: **10 October 2026.** Nothing before that date counts.

The point of this document is to make it hard for us to flatter ourselves. If you find a way the
rules below still let us do that, that is exactly the thing we want to hear about — see
[`../COLLABORATION.md`](../COLLABORATION.md) §3.

---

## 1. Scope — announced first, then everything in it counts

**Phase 1 scope (from 10 Oct 2026):** every **Premier League** and **UEFA Champions League** match
that appears on our site.

- The scope is published before the period it applies to.
- There is no "we're not covering this one" option. Every in-scope match enters the ledger.
- Scope changes are announced in a weekly review **at least 7 days before** taking effect, and are
  **never retroactive**. A match already played cannot be moved in or out.
- We do not have Malaysian or Philippine domestic leagues in our data, so they are not in scope. We
  are not going to reach for national-team fixtures to manufacture local relevance either.

The scoring-rules post goes up, and is pinned, **7 days before** the start date.

## 2. What is in the denominator

A match is **counted** when all three are true:

1. it is in the published scope;
2. it has finished;
3. a pre-match snapshot exists, written **at least 60 minutes before kick-off**.

A match is **listed but not counted** when it was postponed, abandoned, or the snapshot is missing.

Missing snapshots are our failure, not a neutral exclusion. They are counted separately and the
count is published in the weekly review, with the reason. If that number is not near zero, the
ledger is not worth much and we would rather you could see that.

There is no human step anywhere in the selection. The ledger is generated from the snapshots; the
only manual act is mirroring it to social channels, and a channel going quiet for a day changes
nothing about the ledger.

## 3. What "right" means

**The outcome the model ranked highest happened.** Three outcomes, one ranked first, did it occur.

That is the entire definition. Consequences we would rather state than have discovered:

- **Draws are almost always recorded as misses.** The model ranks a draw first in only a small
  fraction of matches. We are not scoring draws leniently, not scoring "close" as partial credit, and
  not excluding draws from the denominator.
- **A 52% call and a 91% call count the same** in the N-of-M figure. Confidence bands are reported
  separately, as a full table, once there are enough matches per band to mean anything.
- **Score-line lists are not scored here.** The ledger scores the three-way call only.
- No alternative scorings ("we'd have been right if…") appear in the ledger.

## 4. Denominators, baselines and how figures are reported

**Every figure is published next to the same matches scored by always siding with the pre-match
favourite.** Not as a footnote — in the adjacent column, always, including in weeks where it makes us
look worse.

This is the part most records leave out, and leaving it out is what makes a record meaningless. A
win rate with no baseline is not evidence of anything.

Reporting rules:

| Cadence | What is published |
|---|---|
| **Daily** | "N of M" for the day, plus the running total since the start date, plus the favourite baseline for the same matches. **No daily percentage** — over 1–5 matches a percentage is noise dressed as information |
| **Weekly** | Full week: matches in scope, right/missed, percentage (week and cumulative), favourite baseline alongside, a confidence-band table including the low bands, and the biggest miss of the week described in full |
| **Per match** | Snapshot timestamp, the three probabilities as they stood, the result, right or missed |

Percentages appear only where the sample supports them: cumulative figures and weekly reviews, never
on a single day.

## 5. The biggest miss goes first

When the post-match write-up has to pick one match, the rule is fixed in advance:

> If any match that day had the model at **65% or higher and it was wrong**, that match is the one
> written up. Multiple such matches: the highest-confidence one. No such match: the
> highest-confidence match of the day, whatever the result.

The write-up uses the same template, the same length and the same tone whether the call was right or
wrong. No softening adjective, no "but", no explanation of why this one doesn't really count.

## 6. What happens when we change something

- **Model or mode change → new ledger.** The old ledger stays published, with its end date and the
  reason for the change. Figures from two ledgers are never added together.
- The ledger runs on one mode (`balanced`) and does not move between modes. See
  [`methodology.md`](methodology.md) §3.
- **Nothing is edited or deleted after publication.** A wrong figure gets a correction underneath it,
  dated; the original stays visible. Deleting one wrong entry makes every other entry unverifiable.
- **A bad run changes nothing.** Not the scope, not the definition of "right", not the cadence. The
  only thing that stops the ledger is a platform or legal problem, and if that happens we will say so.

## 7. How to check us

Everything needed to recompute our published figures is on the ledger page: the match list, the
snapshot timestamps, the probabilities as snapshotted, the results, and the baseline column. A
downloadable CSV of the ledger is on the roadmap so you can do it without copying from a table.

If your recomputation disagrees with ours, open an issue. Ledger errors are the highest-priority
class of issue we accept, above feature requests and above everything else in
[`../COLLABORATION.md`](../COLLABORATION.md) §1.

## 8. What the ledger is not

- Not a tip sheet, a selection service, or a signal feed.
- Not a claim to beat anyone. See [`methodology.md`](methodology.md) §5.1.
- Not a basis for any financial decision. It exists so that a claim about a football model can be
  checked by the people reading it, which is currently rare, and that is the only job it has.

## Disclaimer

Football data for information only. We don't take bets, don't link to any betting operator, and
don't give betting advice. 18+.
