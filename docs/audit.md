# Phase 0 — TFM2 Asset and Rules Audit

**Status: TEMPLATE — not yet filled in.**
Owner: Game Mechanics Analyst. Reviewer: Research Lead. Approver: Human Director.

Fill every section. If something cannot be verified, write it under
"Not verified" rather than guessing. An honest gap is a valid result; an
invented rule is a defect that corrupts every later phase.

Every claim needs a source: a URL, an absolute file path, or a reproducible
observation procedure. A claim with no source does not go in the verified
sections.

---

## 1. PRIORITY — Does a ban/pick → win/loss API exist?

This is the single highest-value question in Phase 0. Reinforcement learning
needs a reward signal. If no trustworthy source maps a draft to an outcome,
the project is guesswork, not research.

An earlier work session on this machine was titled around "an API that returns
win/loss when you give it TFM2 ban/pick choices". Establish whether that thing
is real.

Answer these, each with evidence:

- Does such an API, service, or callable simulator exist? Yes / No / Unclear
- If yes: where is it, how is it called, what does it return, what are its
  limits (rate, determinism, version)?
- If it is a past idea rather than a built artifact, say so plainly.
- Can the TFM2 game itself be driven headlessly to produce match outcomes?
- What evidence was examined — paths searched, sites checked, commands run?

**Conclusion:** <!-- fill in -->

---

## 2. TFM2 ban/pick rules

From primary sources only: the game itself, official documentation, or game
data files. Community wiki content is acceptable **only** when it is marked as
such and the uncertainty is recorded.

| Rule | Value | Source | Confidence |
| --- | --- | --- | --- |
| Ban/pick order | | | |
| Number of bans per side | | | |
| Number of picks per side | | | |
| Asymmetry between sides (first/second) | | | |
| Character / class pool size | | | |
| Duplicate pick or ban allowed? | | | |
| Role or position constraints | | | |
| Visibility of opponent choices | | | |
| Terminal condition of a draft | | | |

Confidence values: `verified` (primary source), `likely` (secondary source),
`unverified` (assumption — must also be listed in section 6).

---

## 3. Win/loss source candidates

Assess each option. State what exists, not what would be convenient.

### (a) In-game battle simulator
Accessible? How? Headless? Deterministic? Version-locked?

### (b) Real match-record dataset
Replays, published statistics, community data dumps. Size, licence, format,
freshness.

### (c) Self-built battle approximation
Always possible, but it is an approximation. If this is the only route, say so
clearly — every number it produces is a model estimate, never a win rate.

**Recommendation with reasoning:** <!-- fill in -->

---

## 4. Game installation and data files

- Is TFM2 installed on this machine or the Windows host? Path?
- Are game data files readable? Format?
- Is there a mod API, a scripting hook, or an exposed data layer?
- Searches performed (list commands and paths, including the ones that found
  nothing):

---

## 5. Public resources

Datasets, open-source projects, research papers, community tools. For each:
URL, what it provides, licence, and how current it is.

Drafting-AI work on other games (MOBA draft research, for example) counts here
when the method transfers — note the transfer argument.

---

## 6. Not verified

Everything that could not be confirmed. This section feeds directly into the
Phase 1 risk list, so be exhaustive rather than tidy.

---

## 7. Recommendation to the Research Lead

- Is Phase 1 safe to start? What changes about its design?
- What is the biggest single risk?
- If no trustworthy outcome source exists, what should the first milestone be
  reduced to?
