# Mod API outcome-source feasibility (Phase 0 addendum)

Date: 2026-10-08. Author: Research Lead. Status: **documentation review only — nothing here is run or tested.**

Question from the project owner: does the official mod repository
(<https://github.com/teamsamoyed/TeamfightManager2Mod>) give us a way to turn a ban/pick result
into a match outcome?

Source examined: shallow clone of that repository, commit `9da2deb11a33022af58bd99a5bad2140f8a47e60`
(2026-09-09). All 27 files were searched; the relevant pages are `docs/stable-api-reference.md`,
`docs/stable-native-mods.md`, and `docs/native-ai-hooks.md`. Section numbers below refer to
`docs/stable-api-reference.md`.

## Short answer

- **No ready-made "draft in → result out" API is documented.** There is no external endpoint,
  command-line runner, or documented headless mode.
- **The documented building blocks may be enough to build one ourselves as an in-game mod.**
  This is a hypothesis. It must be tested with the installed game.

## Documented building blocks

| Need | Documented API | Source |
| --- | --- | --- |
| Force an exact draft | `StableDraftHook::decide_ban` / `decide_pick` returns `Some(champion_id)` to "lock in that exact champion — bypasses scoring and top-k randomness entirely". Illegal ids fall back to normal scoring. | §8-1 |
| Read the draft state | `StableDraftContext`: `phase()`, `available_champions()`, `ally_bans()/enemy_bans()`, `ally_picks()/enemy_picks()`, `champion_briefs()` | §8-1 |
| Get the match winner | Server event `MatchFinished` fires for "**every** match result (player matches and background sims share one path)" with `winner_team_id`, `team1_id`, `team2_id`, scores | §7-6 |
| Winner from replays | `MatchReplay` record has `blue_team_win`, `blue_team` (read-only) | §6-9 |
| Background simulation exists | `sim_origin()` kinds include `ServerPresim` and `Tool`; docs refer to "background pre-sims" | §4-1, §11 |
| Seeded determinism | Sim callbacks must be deterministic from `rng_seed`; `seed()` returns the match seed. Draft scoring is outside the deterministic zone. | §1, §4-1 |
| Draft format setting | Play-option fields include `banpick_style`, `champion_pool`, `simulation_intensity` | §6-2 |
| Platforms | Stable native mods run on Windows, macOS, and Linux. The docs place the SDK "next to the game executable" on Linux. | `stable-native-mods.md` |

## What the docs do not tell us (must be tested)

1. **No documented way to start matches from code.** Matches seem to happen through normal
   management progression (league schedule). Batch or headless execution is not documented.
2. **Whether `decide_*` hooks run for AI-vs-AI background matches.** The docs say the hook
   overrides the draft AI; they do not say which matches use it.
3. **How to export data to an external process.** Documented channels are the game log
   (`host.log` → `%APPDATA%\TeamSamoyed\TeamfightManager2\data\log.log`) and mod save data.
   Native code may be able to write files directly, but that is not documented.
4. **Throughput.** How many matches per real-time minute the game can simulate is unknown.
   This decides whether RL on this signal is practical.
5. **Confounders.** A match result depends on more than the draft: athletes, team strength,
   tactics, item builds, side. Any dataset must record and control these.
6. **Whole-match determinism** for the same draft + teams + seed is not stated.
7. **Linux game build.** The docs imply a Linux executable exists. Whether this Steam title
   runs on Linux / WSL2 is not verified. The machine has no NVIDIA GPU (see README).

## Consequence for the project

This changes the Phase 0 conclusion from "no outcome source found" to
"**a candidate outcome source exists, unverified**": the real game simulator, driven by a
custom mod. If items 1–4 work, the game itself becomes the reward source, and numbers can be
called real simulated match outcomes — not proxy estimates.

Recommended next step: a small feasibility spike with the installed game (see the plan on
Paperclip issue BAB-1). No RL design is fixed before that spike reports.

## Local environment facts (2026-10-08)

- Game (updated 2026-10-08): **installed** by the owner with SteamCMD at `C:\games\tfm2`
  (WSL: `/mnt/c/games/tfm2`). App 3009300, buildid 25769776, `StateFlags 4` (fully installed),
  1.2 GB. No Steam desktop client was found under `Program Files`/`Program Files (x86)`; the
  game ships `steam_api64.dll`, so launching may need a running, logged-in Steam client
  (unverified — spike T1). The game has not been launched yet: no
  `%APPDATA%\TeamSamoyed` folder exists.
- Rust (corrected 2026-10-08): `stable-x86_64-unknown-linux-gnu` **was already installed**
  (rustc 1.98.0, cargo 1.98.0). The earlier "no toolchain" finding was wrong: agent runs set
  `HOME` to a temporary directory, so `rustup` looked in the wrong place.
  Agent shells must export `RUSTUP_HOME=$HOME/.rustup CARGO_HOME=$HOME/.cargo`.
- Windows target `x86_64-pc-windows-gnu`: installed 2026-10-08.
- Cross-linker: `x86_64-w64-mingw32-gcc` (GCC 13-win32) installed by the owner 2026-10-08.
  Verified: a minimal `cdylib` (`#[unsafe(no_mangle)] extern "C" fn ping`) builds with
  `cargo build --release --target x86_64-pc-windows-gnu` into a PE32+ x86-64 DLL that exports
  `ping` (checked with `file` and `x86_64-w64-mingw32-objdump -p`). Cargo default edition is
  2024, so exported symbols need `#[unsafe(no_mangle)]`, not `#[no_mangle]`.
  Not yet verified: that the game loads a WSL-built DLL (spike T1).
- Stable SDK: `/mnt/c/games/tfm2/mod-sdk-stable/` (`base_version.txt` = 0.6.3), with
  `template/` and `mod-api-stable/` (ABI contract levels 1–4 frozen). `decide_ban`,
  `decide_pick`, `MatchFinished` and `ServerPresim` are present in the source.
  Verified: the unmodified `template/`, copied out of the game folder, builds with
  `cargo build --release --target x86_64-pc-windows-gnu` (linker `x86_64-w64-mingw32-gcc`) into
  a PE32+ x86-64 DLL exporting `tfm2_mod_entry_stable` and `tfm2_mod_required_abi_level`.
  Its imports are Windows system DLLs only (no MinGW runtime DLL to ship).
- No build-side blocker remains. Open: whether the game loads this DLL (T1).
