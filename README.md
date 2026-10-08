# tfm2-draft-rl

Research repository for Babo.Lab: a ban/pick (drafting) decision agent for
**Teamfight Manager 2 (TFM2)**.

Long-term goal: a reproducible, competitive RL-based drafting agent.
First milestone: a scientifically sound foundation that makes later
improvements measurable.

## Status

**Phase 0 — asset and rules audit. Nothing is implemented yet.**

No TFM2 code, simulator, or dataset was found on this machine before this
repository was created. Everything here is built from scratch, and no game
mechanic is treated as known until a primary source confirms it.

## Milestone plan

| Phase | Goal | Output |
| --- | --- | --- |
| 0 | Audit TFM2 rules, win/loss sources, available assets | `docs/audit.md` |
| 1 | Drafting environment with verified rules + tests | `tfm2_draft/env/`, `tests/`, `docs/env.md` |
| 2 | Random and heuristic baseline agents | `tfm2_draft/agents/` |
| 3 | Reproducible evaluation pipeline and one full experiment | `scripts/evaluate.py`, `experiments/`, `docs/results.md` |
| 4 | Technical report and RL roadmap | `docs/report.md` |

A phase does not start until the previous phase is reviewed and approved by
the Human Director.

## Hardware constraint

This machine has **no NVIDIA GPU**. Verified on 2026-10-08: the WSL2
passthrough device `/dev/dxg` exists, but `libcuda.so` and `nvidia-smi` do
not, and the only graphics device on the Windows host is `Intel(R) UHD
Graphics`. CUDA is therefore not available.

All plans assume **CPU only: 8 cores, ~7.6 GB RAM**. Any need for external
compute goes to the Human Director as a separate approval.

## Scientific rules for this repository

1. Do not invent game mechanics. Every rule in `docs/` carries a source
   (URL, file path, or observation procedure).
2. Keep **drafting simulation** and **match-outcome simulation** separate in
   the code. They are different things with different confidence levels.
3. Never label a number a "win rate" unless it comes from real match
   outcomes. An approximation model produces "model estimates", and the text
   must say so.
4. Record config, random seed, model version, and baseline for every
   experiment.
5. Unverified items stay in a "Not verified" section. An empty section is a
   better result than a guess.

## Layout

```
docs/          research documents (audit, env spec, results, report)
tfm2_draft/    package source (env/, agents/)
tests/         pytest suite
scripts/       runnable entry points (evaluation, experiments)
experiments/   per-run outputs: config, seed, metrics
```

## Setup

Not yet applicable. Setup instructions are written in Phase 1, together with
the first code that needs them, and must be runnable as written.
