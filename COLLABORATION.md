# Working with AI Ball

> This repository contains documentation only. The product source code is not open source, and there
> is no plan to publish it. Everything below is about working **with us**, not about contributing to
> a codebase.

Five ways to work with us. Each one says who it suits, how to start, and what you get back.
If none of them fit, write to **1qazxsw2sky5@gmail.com** and say what you had in mind.

One rule that applies to all of them: we are a football **data** site. We don't take bets, don't link
to any operator, and don't give betting advice. Requests that amount to "tell me what to pick" get a
polite no, every time, from everyone here.

---

## 1. Data corrections

**Suits:** anyone who noticed something wrong while using the site. You do not need to be a developer.

The most common real problems, in the order we see them:

- **Team name mapping.** Our source data is not in English. Club names are mapped to an English name
  for every non-Chinese interface. Mappings for smaller clubs, and for clubs that changed name or
  were promoted recently, are the ones that go stale. Symptoms: a club shown under an old name, an
  odd transliteration, or a club you can't find by typing its English name into search.
- **Fixture errors.** Wrong kick-off time, wrong competition label, a match listed in the wrong round,
  a postponed match still shown as scheduled.
- **League labelling.** A competition shown under a name nobody uses in English, or two competitions
  collapsed into one label.
- **Ledger errors.** A match that should be in scope and isn't, a result recorded wrongly, a snapshot
  time that doesn't match what we posted. These get priority over everything else in this list.

**How to start:** open a [data correction issue](.github/ISSUE_TEMPLATE/data-correction.yml). Include
the match URL or the club as it appears on the site, what is wrong, and what it should be. A
screenshot helps. One issue per problem — it makes them easier to close and easier for you to track.

**What you get back:** a reply from a human within two working days saying whether we can reproduce
it. Fixes ship with the next data update and the issue says when. Anyone whose correction we ship
gets founding-member access (see §6) if they want it, and credit in the issue. If you would rather
not be credited, say so and we won't.

---

## 2. Translation — Malay, Thai, Filipino

**Suits:** native or fluent speakers who follow football in that language. Football vocabulary is the
hard part, not general fluency: we need the words fans actually use, not the dictionary ones.

Where the interface stands today: the site runs in English, Malay, Thai and Chinese. Club and
competition names are shown in English across all non-Chinese interfaces. Malay is the one we most
want a second opinion on, because Malaysia is where most of our early users are, and because a
clumsy Malay phrasing in a football context reads worse than plain English does.

Filipino is not live yet. If you want to shape it before it exists, that's the most useful time.

**How to start:** open an issue titled `[i18n] <language> — <screen or phrase>`, quote the wording the
live site currently shows, and give the wording you would use instead plus one line on why. We
review wording in the issue thread against the live site; we do not publish the string files here.

A note on tone, so we don't waste your time: our wording avoids anything that reads as a tip, a pick,
or a promise. If a phrase would be more natural with a word from that world, tell us — but we will
probably choose the more awkward phrasing on purpose, and we'll say so.

**What you get back:** founding-member access, credit in the issue and in the release note for that
language, and — for anyone who does a sustained pass over a whole language — a named line in the
site's about page if you want one. We are not offering payment for translation work at this stage and
we would rather say that up front than let you find out later.

---

## 3. Method review

**This is the one we want most.**

**Suits:** anyone who has built or evaluated a football model — xG, Poisson/Dixon-Coles variants,
Elo, market-derived baselines — or who works on forecast evaluation generally. Also students who
have just learned to compute a Brier score and want something real to point it at.

What we are asking you to attack:

- **Our calibration claim.** We say the honest measure of a probabilistic model is whether 70% events
  happen about 70% of the time. Tell us our binning is wrong, our sample is too small to say anything,
  or our reliability curve is being read charitably.
- **Our ledger denominator.** Read [`docs/open-ledger.md`](docs/open-ledger.md) and try to find a way
  the rules let us flatter ourselves. Scope definition, the ≥60-minute snapshot rule, what we exclude
  and how we count exclusions, how we handle a mid-season scope change.
- **Our baseline.** We score every ledger figure against "always side with the pre-match favourite".
  If you think that is the wrong baseline, or a suspiciously easy one, say so and name a better one.
- **The method itself.** [`docs/methodology.md`](docs/methodology.md) describes six fused paths at the
  level of the public literature. Tell us where fusing correlated paths is buying us nothing, or where
  the low-score correction is doing less than we think.

**How to start:** open an issue with the `method` label, or write to the email above if you'd rather
not do it in public. Direct, unhedged criticism is fine and preferred. We will not argue with you in
public about a point we can't support.

**What you get back:** a written reply from whoever owns that part of the method, not a form
response. If a review changes what we publish, we say so in the ledger review for that period and
name you unless you'd rather we didn't. Founding-member access on request. We are also happy to be a
named case study in a write-up of yours, including a critical one.

---

## 4. Content creators

**Suits:** people who write newsletters, make videos, or post analysis about football data — any
size, and small is genuinely fine. If you are already explaining numbers to an audience, we would
rather be one of your sources than one of your ads.

**How to start:** email **1qazxsw2sky5@gmail.com** with a link to something you have made. Tell us
what you'd want from us — a data pull for a specific angle, early sight of the weekly ledger review,
or a walkthrough of how a number on a match page is built.

**What we give:** early access to the weekly ledger review before it goes out, help pulling the
specific numbers a piece needs, and a named contact who answers. We do not pay for coverage, we do
not ask for approval over what you write, and we will not ask you to take a post down. If you write
that our model is no better than backing the favourite, that is a thing our own documentation says,
and we are not going to complain about you repeating it.

**What we ask:** don't frame us as a tipping service; say which date and which scope any figure you
quote came from.

---

## 5. Students and research projects

**Suits:** undergraduate and postgraduate data science, statistics and CS students; university data
and analytics clubs, including in Malaysia (UM, Sunway and others) and elsewhere in Southeast Asia;
anyone needing a real dataset with real problems in it for a semester project.

Project shapes that would genuinely help us, and that make defensible coursework:

1. **Calibration audit.** Take our published ledger, compute reliability curves and Brier
   decomposition, and report whether our stated confidence bands mean anything at the sample size
   available. Deliverable: a report. Difficulty: suits a solid final-year project.
2. **Baseline comparison.** Build the simplest reasonable baselines and check how much, if at all,
   a fused model beats them on the same fixtures. We expect the answer to be "not much", and a
   student finding that cleanly is worth more to us than one finding the opposite.
3. **Name-matching across sources.** Matching club names between football data sources is a
   genuinely hard, genuinely unsolved nuisance problem — good for an NLP or record-linkage project.
4. **Malay-language football text.** Readability and terminology of automatically generated match
   write-ups in Malay. Suits a linguistics or HCI angle as much as a CS one.

**How to start:** email **1qazxsw2sky5@gmail.com** from your university address with a paragraph on
what you want to do and your timeline. For a club or a supervised project, we will write a one-page
scope your supervisor can sign off on.

**What you get back:** a defined data extract for the agreed scope under a short written agreement
covering what you may publish and what you may redistribute (our source data comes with its own
licensing constraints, so we cannot hand over raw feeds and we won't pretend otherwise); a named
contact; a 30-minute call at the start and one before you submit; founding-member access for everyone
on the team. We will read your final report. If your findings are unflattering, we will link to them
anyway — that is the entire point of the ledger.

**What we ask:** no scraping of the site. Ask us and we'll give you a clean extract instead.

---

## 6. If you want an API

We do not have a public API today, and we would rather build the right one late than a guessed one
early. So we are collecting requirements first.

**How to register a need:** open an issue titled `[api] <what you are building>` and tell us:

- what you are building, and whether it is personal, academic or commercial;
- the three calls you would actually make, in plain words ("fixtures for a date range with our
  pre-match read", "the ledger rows for a given week");
- what shape you need it in and how often you'd call it;
- whether a scheduled file drop would do instead of a live endpoint.

**What you get back:** your requirement recorded in a public list so you can see who else wants what.
Everyone on that list is told before anything ships and gets access first. We will say no to
anything that amounts to redistributing our source data, because that isn't ours to give.

---

## What founding-member access is

Referred to above as "founding-member access". As currently confirmed, it is:

- the paid tier, free for 90 days;
- a longer history window than a free account (a free account sees a short recent window; this tier
  sees considerably more of the past season — the exact windows are shown on the site);
- a direct group with the people building the product, where the founder is present under their real
  name;
- a vote on what we build next, from a list of things actually on our backlog;
- your name on the founders list, if you opt in — it is opt-in, and skipping it costs you nothing.

That is the full list. There is no referral money, no revenue share, and nothing that depends on
match outcomes. If something else gets confirmed later, it will be added here and dated.

---

## Code of conduct

[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md). Short version: argue with the work, not the person.

## Disclaimer

Football data for information only. We don't take bets, don't link to any betting operator, and
don't give betting advice. 18+.
