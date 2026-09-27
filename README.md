# AI Ball — Football data you can check

> **Ringkasan (BM):** AI Ball ialah laman data bola sepak. Kami menyiarkan bacaan model untuk setiap perlawanan
> **sebelum sepak mula**, kemudian menyemaknya selepas perlawanan dalam lejar terbuka yang sesiapa sahaja boleh kira
> semula. Setiap nombor di halaman perlawanan boleh dijejaki ke sumbernya. Repo ini mengandungi **dokumentasi sahaja** —
> kaedah kerja, peraturan lejar, dan cara anda boleh menyumbang atau membetulkan data kami.
> Untuk maklumat sahaja. Bukan nasihat. 18+.

AI Ball is AI football match analysis for people who want to see where a number came from.
Live site: **https://aiball.samagent.ai**

> **This repository contains documentation only. The product source code is not open source.**
> No code, scripts, configuration or data files are published here, and none are planned.
> What is here: how the method works at the level of the public literature, the rules our open
> ledger runs on, and how to work with us.

---

## What this is

Three things, in order of how much we care about them:

1. **An open ledger.** From **10 October 2026** we publish the model's read on every match in a
   pre-announced scope (Premier League + UEFA Champions League, phase 1) *before kick-off*, and we
   check every one of them afterwards in public. Nothing is picked after the fact. Misses are not
   folded away. Rules: [`docs/open-ledger.md`](docs/open-ledger.md).

   **There is already something to check.** A public record has been running since 27 August 2026 —
   473 matches as of 27 September, 274 of them right, with every miss listed in the same table and
   the pre-match-favourite baseline printed next to it:
   **https://aiball.samagent.ai/en/record** . Since 21 September the pre-match figures are locked and
   archived automatically before kick-off. The October ledger tightens the scope and the rules; it
   does not start the counting.
2. **A match page where every number is traceable.** Recent form, head-to-head, injuries, goal
   estimates — each one shows what it was computed from, not just a figure.
3. **A method we are willing to have picked apart.** Shin de-vigging, a Poisson goal model with a
   Dixon-Coles low-score correction, six weighted paths fused into one read.
   Overview: [`docs/methodology.md`](docs/methodology.md), including what it *cannot* do.

## Why we think this is different

Most football prediction sites publish an accuracy figure and no way to verify it. We are doing the
opposite: **we are not selling you an accuracy number — we are publishing every call and letting you
count.**

One thing we have to be exact about, because it is the whole point of the page. Of the matches on the
open record today, only those from **21 September 2026** onwards were snapshotted automatically before
kick-off — 62 of 480 at the time of writing. The earlier 418 were written into the record after the
fact from the figures stored in our database. They are the same numbers, but they are not
independently proven to pre-date the match, and we are not going to pretend otherwise. From 21
September the lock is automatic; the October ledger inherits that guarantee.

Concretely:

| Common practice | What we do |
|---|---|
| A headline accuracy % with no sample, no date range, no method | A per-match ledger with the snapshot timestamp, the scope, and the denominator stated up front |
| Only the wins get posted | Every match in the published scope is in the ledger; misses are listed in the same table, not in a footnote |
| No baseline | Every ledger figure is printed next to the same matches scored by *always siding with the pre-match favourite* |
| "Our AI says…" with no visible inputs | Each figure on a match page links back to the rows it came from |
| Silent model changes | A model or mode change starts a **new** ledger; the old one stays visible |

We have published the limits of the method as plainly as the method itself. See the **Limits**
section of [`docs/methodology.md`](docs/methodology.md). If that section reads like it was written by
someone trying to talk us out of overclaiming — it was.

## Screenshots

*(placeholder — images to be produced by our asset team before first publish; do not publish this
section empty.)*

| File | What it must show | Notes |
|---|---|---|
| `docs/img/01-match-page.png` | A single match page in **English**, full height, showing the traceable-number panels (form, H2H, injuries, goal estimate) | English or Malay UI only |
| `docs/img/02-open-ledger.png` | The open ledger page: snapshot time, model read, result, right/missed, running total, favourite baseline column | Must include at least one visible **miss** |
| `docs/img/03-number-provenance.png` | Close crop of one figure expanded to show the rows behind it | The whole point of the repo in one image |
| `docs/img/04-malay-ui.png` | The same match page in Malay | Shows we are not an English-only site |

Rules for whoever cuts these: English or Malay interface only; no pricing or staking panels anywhere
in frame; no Chinese UI; annotate in English; 2x resolution, PNG, < 500 KB each.

## Quick start

There is nothing to install, and nothing to clone.

1. Open **https://aiball.samagent.ai**
2. Pick a match from the list. Team search works in English — type `Lill` and you get Lille.
3. On the match page, open any number and follow it to the rows it came from.
4. From 10 Oct, open the **open ledger** page and check yesterday's calls against yesterday's results.
5. Free accounts see a 15-day history window; registered accounts see more, and founding members see
   more again (see [`COLLABORATION.md`](COLLABORATION.md)).

The site is in English, Malay, Thai and Chinese. We are a small team working out of Malaysia, so
Malay and English are the two we care most about getting right — see the translation section in
[`COLLABORATION.md`](COLLABORATION.md).

## Method, in one paragraph

Six independent paths produce a win/draw/loss view of a match; they are fused with per-mode weights
into a single read. One path recovers implied probabilities from consensus pricing using
**Shin (1993)**; one uses recent results; one is a **Poisson** goal model with a **Dixon-Coles**
low-score correction and a dynamic rho; the remaining three are agreement and dispersion corrections
across sources. A risk score summarises how much the paths disagree. There are six preset weight
modes; the ledger is locked to **balanced** so the record cannot be shopped between modes.
Full write-up, and the parts we are not happy with: [`docs/methodology.md`](docs/methodology.md).

We do not publish the weight table, the calibration anchors, the ingestion pipeline or any source
code. We do publish enough for you to tell us the method is wrong, which is the part we actually want.

## Open ledger

Starts **10 October 2026**, the weekend the Premier League returns.

- **Scope, phase 1:** every Premier League and UEFA Champions League match that appears on our site.
  The scope is announced before it starts and can only change with 7 days' notice, never retroactively.
- **In the denominator:** in-scope, completed, and with a snapshot written **≥60 minutes before
  kick-off**.
- **Listed but not counted:** postponed, abandoned, or snapshot missing. A missing snapshot is our
  fault and is counted separately as ours.
- **"Right" means:** the outcome the model ranked highest happened. The model almost never ranks a
  draw first, so **draws are almost always recorded as misses**. We are telling you this before you
  find it.
- **Daily posts say "N of M", not a percentage.** A percentage over 1–5 matches is noise.
- **Nothing is edited or deleted after posting.** Corrections go underneath; the original stays up.

Full rules: [`docs/open-ledger.md`](docs/open-ledger.md).

## Roadmap

Dates are intentions, not promises. Items move. Everything below happens on the site or in this
documentation; none of it is a plan to publish source code.

| When | What |
|---|---|
| Oct 2026 | Public ledger page live on the site; scoring-rules post pinned 7 days before the start date |
| Oct 2026 | Malay interface review pass with a native reviewer |
| Nov 2026 | First four-week ledger review published in full, including the biggest miss |
| Nov 2026 | Calibration report: when the model says 70%, how often does it happen |
| Q4 2026 | Ledger download (CSV) from the ledger page, so you can recompute our published figures yourself |
| Q4 2026 | A public issue thread per language for interface-wording feedback (Malay, Thai, Filipino) |
| Demand-driven, no date | A read-only data endpoint. We are collecting requirements first — see the API section in [`COLLABORATION.md`](COLLABORATION.md). Tell us what you would call and we will size it |

Not on the roadmap, on purpose: tipping, staking tools, "who should I pick" answers.

## How to get involved

Five ways, all described properly in [`COLLABORATION.md`](COLLABORATION.md):

- **Data corrections** — wrong team-name mapping, wrong fixture, wrong league label. Open a
  [data correction issue](.github/ISSUE_TEMPLATE/data-correction.yml).
- **Translation** — Malay, Thai and Filipino wording for the interface and match write-ups, reviewed
  in issues against what the live site currently shows.
- **Method review** — tell us our calibration or our ledger denominator is wrong. This is the one we
  want most. Open a [feature/method issue](.github/ISSUE_TEMPLATE/feature-request.yml).
- **Content creators** — if you write or stream about football data.
- **Students and researchers** — including data and analytics clubs at UM, Sunway and elsewhere; we
  can supply a defined data extract under a short agreement, plus a supervisor-facing project scope.

Questions that don't fit an issue: **1qazxsw2sky5@gmail.com**

## License

Documentation in this repository is **CC BY 4.0** — see [`LICENSE`](LICENSE). Reuse it, quote it,
translate it; just say where it came from. We chose CC BY rather than a software licence such as MIT
because everything here is prose and specification rather than code, and CC BY is the licence that
actually says what we mean about text: attribution required, derivatives allowed, no extra conditions.
It also travels well — a student quoting our ledger rules in a report, or a maintainer quoting us in
a curated list, is covered without asking us.

## Disclaimer

Football data for information only. We don't take bets, don't link to any betting operator, and
don't give betting advice. 18+.

*Data bola sepak untuk maklumat sahaja. Kami tidak menerima pertaruhan, tidak memaut kepada mana-mana
pengendali, dan tidak memberi nasihat. 18+.*
