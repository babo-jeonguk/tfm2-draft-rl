# Repository conventions — tfm2-draft-rl

These rules apply to anyone working in this repository.

## Scientific integrity

1. **Do not invent game mechanics.** Every TFM2 rule written in `docs/` names
   its source: a URL, an absolute file path, or a reproducible observation
   procedure. No source means it goes in a "Not verified" section.
2. **Separate the two simulations.** Drafting simulation and match-outcome
   simulation are different things with different confidence levels. Keep them
   in different modules. Do not let outcome code leak into environment code.
3. **"Win rate" is a reserved word.** Use it only for numbers derived from real
   match outcomes. Output from an approximation model is a "model estimate",
   and the surrounding text must say which model produced it.
4. **No invented results.** Never write a metric that was not produced by code
   that actually ran. Paste the real command output.

## Reproducibility

- Fix every random seed. Record it in the experiment output.
- Every experiment writes `experiments/<run-id>/` containing its config, its
  seed, and its metrics.
- Any command written in a document must run exactly as written. Check it.

## Code

- Python 3.12. Standard library first; add a dependency only when it earns its
  place, and pin it.
- `pytest` for tests. New behaviour arrives with a test.
- No GPU code paths. This machine has no NVIDIA GPU (verified 2026-10-08).
- Keep the package importable: `tfm2_draft/` holds the library, `scripts/`
  holds entry points.

## Safety

- Do not delete or overwrite files you did not create. If a file is in the way,
  stop and report it.
- Never write credentials, tokens, or API keys into files or logs.
- Do not install paid services or launch long training runs. Those need Human
  Director approval through the Research Lead.
- Stay inside this repository. Do not modify Paperclip state directories or
  unrelated projects.

## Git

- Remote: `origin` = https://github.com/babo-jeonguk/tfm2-draft-rl (public).
- Work on `main`. Small, focused commits with a message that says what changed
  and why. Run `git pull --rebase` before you commit, and `git push` after.
- Do not force-push. Do not rewrite history.
- Commit author is `babo-jeonguk <dnrtoki@gmail.com>`, set in the repo
  `.git/config`. Do not override it (no `-c user.*`, `--author`, or
  `GIT_AUTHOR_*`). Older commits keep their agent addresses; that is expected.
- Do not write personal machine paths in committed files. Use `~` for the
  Linux home and `/mnt/c/Users/<user>` for the Windows user folder.
- **This repository is public.** Never commit credentials, game binaries
  (`.exe`, `.dll`), extracted game assets, save files, or proprietary game
  data. Commit source, docs, and small result files only.
