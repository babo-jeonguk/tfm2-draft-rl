# Phase 0 — TFM2 Asset and Rules Audit

**Status:** Preliminary audit, 2026-10-08. **Owner:** Game Mechanics Analyst. **Reviewer:** Research Lead. **Approver:** Human Director.

Evidence lens: primary-source-first; version drift is recorded. Search scope is finite and is not proof that no private or undocumented artifact exists.

## 1. Does a ban/pick → win/loss API exist?

**Conclusion: No documented public API or service accepting a TFM2 draft and returning a match outcome was found.** This is a bounded negative finding, not proof that no private tool exists. The earlier phrase about “an API that returns win/loss” remains a past idea rather than a confirmed artifact in the checked research repository: its only contents were the scaffold and this audit template (repository path `~/projects/tfm2-draft-rl`; `find ~/projects/tfm2-draft-rl -maxdepth 3 -type f -print`).

The official Team Samoyed mod documentation describes an in-game stable native mod API and simulation callbacks. Its API reference describes simulation state, match hooks, and determinism requirements, but the examined docs do not document a supported external callable runner taking draft input and returning a game result: [stable API reference](https://github.com/teamsamoyed/TeamfightManager2Mod/blob/main/docs/stable-api-reference.md), [mod documentation](https://github.com/teamsamoyed/TeamfightManager2Mod). The reference's determinism statement applies to simulation callbacks and says to derive randomness from the provided seed; it does not establish that an unmodified match can be launched headlessly or that the whole game is deterministic.

Headless execution is **unverified**. No game installation was found in the searched Windows Steam/Program Files and user AppData locations or WSL Steam directories (commands listed in §4). The official mod docs demonstrate extensibility and simulation hooks, but no headless execution instructions were found in the searched documentation.

**Evidence searched:**

- Local repository: `find ~/projects/tfm2-draft-rl -maxdepth 3 -type f -print`; repository search `rg -n -i 'api|win.?loss|simulat|teamfight manager|tfm2' ~/projects/tfm2-paperclip/.paperclip-state/instances/default/projects` (workspace records, with irrelevant Paperclip logs excluded from conclusions).
- Machine install locations: `find /mnt/c/Users/<user> -maxdepth 7 \( -iname '*Teamfight*Manager*2*' -o -iname 'TeamfightManager2.exe' \) -print`; `find /mnt/c/Program\ Files\ \(x86\)/Steam/steamapps -maxdepth 5 -iname '*Teamfight*' -print`; `find ~/.steam ~/.local/share/Steam -maxdepth 5 -iname '*Teamfight*' -print`; and a second bounded search of `/mnt/c/Users/<user>/AppData/Local/Programs`, `/mnt/c/Users/<user>/AppData/Roaming/TeamSamoyed`, `/mnt/c/Program Files`, `/mnt/c/Program Files (x86)`, and both WSL Steam roots.
- Web searches: `Teamfight Manager 2 official Steam gameplay draft ban pick`; `Teamfight Manager 2 API simulator dataset GitHub`; `Teamfight Manager 2 match data API`; `Teamfight Manager 2 Steam Workshop mod API simulator`; and GitHub-scoped searches for datasets and the official mod API. Relevant primary results: [official Steam announcement](https://steamcommunity.com/app/3009300/announcements/), [official stable mod API docs](https://github.com/teamsamoyed/TeamfightManager2Mod).

## 2. TFM2 ban/pick rules

Only the carded 3/2 sequence below is currently verified. The official Steam announcement states: “Both teams ban 3 champions and pick 3 champions each, then ban another 2 each and pick their remaining 2 champions.” It labels this option “5 Cards (3/2).” Source: [Team Samoyed Steam announcement](https://steamcommunity.com/app/3009300/announcements/). The announcement is a game-developer source, but applies specifically to that setting; it does not establish a universal draft format or the latest live build’s default.

| Rule | Value | Source | Confidence |
| --- | --- | --- | --- |
| Ban/pick order | In “5 Cards (3/2)”: both sides ban 3; both sides pick 3; both sides ban 2; both sides pick their remaining 2. Exact alternating turn order within a round is not stated in the announcement. | [Official Steam announcement](https://steamcommunity.com/app/3009300/announcements/) | verified for sequence/counts in named setting; sub-order unverified |
| Number of bans per side | 5 total in the named setting (3 + 2). | [Official Steam announcement](https://steamcommunity.com/app/3009300/announcements/) | verified for named setting |
| Number of picks per side | 5 total in the named setting (3 + 2); “remaining 2” implies five intended roster slots for that setting. | [Official Steam announcement](https://steamcommunity.com/app/3009300/announcements/) | verified for named setting |
| Asymmetry between sides (first/second) | Not stated by source. | [Official Steam announcement](https://steamcommunity.com/app/3009300/announcements/) | unverified |
| Character / class pool size | Not stated by the primary source examined; changes with game version/mods are possible. | Not verified; see §6 | unverified |
| Duplicate pick or ban allowed? | Not stated by source. | Not verified; see §6 | unverified |
| Role or position constraints | Not stated by source. | Not verified; see §6 | unverified |
| Visibility of opponent choices | Not stated by source. | Not verified; see §6 | unverified |
| Terminal condition of a draft | The named sequence describes 5 picks per side, but lock-in behavior, invalid-draft handling, and terminal-state API are not described. | [Official Steam announcement](https://steamcommunity.com/app/3009300/announcements/) | unverified beyond count |

The game’s official Steam page describes five roles (top, jungle, mid, bot, support), but does not specify draft legality constraints; this is not evidence that picks are role-locked. Source: [official Steam store page](https://store.steampowered.com/app/3009300/Teamfight%20Manager%202/).

## 3. Win/loss source candidates

### (a) In-game battle simulator

The game is described by its official store page as a single-player esports management simulation with MOBA-style matches. Official developer mod docs expose stable simulation callbacks and match hooks. The mod API reference says simulation callbacks must be deterministic given inputs and a provided seed. Sources: [Steam store page](https://store.steampowered.com/app/3009300/Teamfight%20Manager%202/), [official mod docs](https://github.com/teamsamoyed/TeamfightManager2Mod), [stable API reference](https://github.com/teamsamoyed/TeamfightManager2Mod/blob/main/docs/stable-api-reference.md).

However, this machine has no detected install in the searched locations, and we did not verify that the simulator can be driven headlessly, invoked from an external program, or supplied arbitrary draft choices. Therefore it is a promising investigation target, not an available reward source yet. Determinism of the overall match outcome is also unverified.

### (b) Real match-record dataset

Searches found TFM2 community league tracking software and roster/database packs, but no public, licensed collection of actual TFM2 match records with draft and outcome fields was confirmed. The repository `tiramisuzu701/teamfightmanager2-competition` describes a user-operated league tracker and manual game/match logging, not an existing published dataset: [project README](https://github.com/tiramisuzu701/teamfightmanager2-competition). The real-world roster repository is a database/roster mod, not TFM2 match outcomes: [tfm2-real-teams-and-rosters](https://github.com/eminyilmazz/tfm2-real-teams-and-rosters).

Dataset size, license for records, freshness, export format, and match-draft coverage are consequently unknown. Community records from a tracker could be a future collection route, subject to access, permission, and validation; no such dataset is recommended as currently available.

### (c) Self-built battle approximation

A model could be built from observed rules and chosen abstractions, but no TFM2-validated approximation exists in this repository. Its outputs would be **model estimates**, not match results or win rates. It can support software experiments only after explicit model design and should not be used as a trustworthy RL reward until validated against actual game outcomes.

### Recommendation

Prioritize obtaining a game installation and testing whether its official match engine can be driven reproducibly through supported SDK hooks or UI automation. This is the only candidate plausibly grounded in actual TFM2 outcomes, but currently remains unverified. Do not begin outcome-reward RL yet. Use a self-built approximation only as a clearly labeled exploratory model if the Research Lead and Human Director approve a reduced milestone.

## 4. Game installation and data files

- **Installation:** No TFM2 executable or app data was found in the bounded Windows/WSL locations listed in §1. This does not rule out another disk, Windows user, library, or host outside the mounted paths.
- **Readable game data:** No local game files were available to inspect. Official mod documentation states packaged builds contain `bundle.game_data` and describe using `TFM2ModUploader.exe` to unpack the base bundle into `mods/base_unpacked`; that is a documented workflow, not a local observation. Source: [official workshop upload docs](https://github.com/teamsamoyed/TeamfightManager2Mod/blob/main/docs/workshop-upload.md).
- **Mod API:** Yes, official developer documentation for a mod API exists. It supports data and asset mods and native Rust code; current stable mod docs describe draft/build hooks, simulation callbacks and match hooks. Source: [official TeamfightManager2Mod docs](https://github.com/teamsamoyed/TeamfightManager2Mod). This finding establishes mod extensibility, not an external win/loss endpoint or headless harness.
- **Commands and paths searched:** `find ~/projects/tfm2-draft-rl -maxdepth 3 -type f -print`; `find /mnt/c/Users/<user> -maxdepth 7 ...`; `find /mnt/c/Program Files (x86)/Steam/steamapps -maxdepth 5 -iname '*Teamfight*'`; `find ~/.steam ~/.local/share/Steam -maxdepth 5 -iname '*Teamfight*'`; and `find` over Windows AppData Local Programs/Roaming TeamSamoyed plus both Program Files roots. Searches were read-only.

## 5. Public resources

| Resource | What it provides | License/access and freshness | Transfer/use note |
| --- | --- | --- | --- |
| [Team Samoyed official mod docs](https://github.com/teamsamoyed/TeamfightManager2Mod) | First-party mod APIs, draft hooks, match/simulation hooks, data formats. | Public GitHub repository; a license for all docs/code was not established in this audit. Docs are current enough to describe stable API and version caveats; exact game-version match must be checked before use. | Directly relevant to inspecting game mechanics and possible instrumentation, but does not prove headless callable simulation. |
| [Steam developer announcement](https://steamcommunity.com/app/3009300/announcements/) | Patch notes including “5 Cards (3/2)” ban/pick counts. | Public official Steam page; applies to the announcement’s patch/setting. | Primary source for this one configuration. Recheck in the installed current build. |
| [TFM2 competition tracker](https://github.com/tiramisuzu701/teamfightmanager2-competition) | Hostable league site with manually entered match results/drafts. | Public source repo; no license or rights to an aggregate match dataset confirmed. README says admins log games optionally. | Could support prospective collection if organizers consent; not an existing outcome corpus. |
| [Real-World LoL Rosters 2026](https://github.com/eminyilmazz/tfm2-real-teams-and-rosters) | Custom TFM2 roster database built using real esports rosters/statistics. | Public GitHub project/releases; license not confirmed here. Current release claims 2026 roster/stat inputs. | Not actual TFM2 match outcomes; do not treat source-game statistics as TFM2 outcomes. |
| [BPCoach](https://arxiv.org/abs/2311.05912) | Visual analytics for professional MOBA hero drafting. | Public research paper; license/data availability not assessed here. Published 2023. | Some draft representation/analysis methods may transfer because both tasks select a team composition from a roster before a match. Its MOBA competition rules, player execution and outcome data do not transfer directly to TFM2’s simulated management game. |
| [Which Heroes to Pick?](https://arxiv.org/abs/2012.10171) | Neural networks and tree search for MOBA hero drafting. | Public paper; license/data availability not assessed here. Published 2020. | Search/optimization ideas may transfer; learned outcome model requires TFM2-specific data and validation. |

Web searches (recorded in §1) also surfaced unrelated or weakly sourced SEO/community wiki pages. They were not used to verify mechanics. No public TFM2 actual-match dataset with confirmed license, size, freshness, and draft-linked outcomes was found in the search sweep.

## 6. Not verified

- Existence of any private, local, or undocumented ban/pick → outcome API outside the searched repository and public web results.
- A supported way to launch or run TFM2 matches headlessly; whether UI automation or a native mod can set arbitrary draft choices and read final outcomes.
- Whole-match determinism and whether identical draft/team/tactics/seed reproduce identical outcomes. The official API’s deterministic callback contract is narrower than this question.
- Current default draft setting, all available draft settings, and whether “5 Cards (3/2)” is the only or default format.
- Exact alternating order/side priority of bans and picks, first/second-side asymmetry, and whether side selection changes order.
- Current champion/class pool size, including version, custom mods, and career-specific availability.
- Whether same champion can be selected by both teams, whether bans can overlap, and how duplicate/invalid submissions are resolved.
- Whether champions are locked to roles, can flex, or are assigned to lanes after selection; what legality checks apply to team composition.
- What each side can see during each draft stage (simultaneous vs sequential bans, hidden choices, recommendations, opponent information).
- Draft terminal condition and tie/error handling beyond the source’s count of five picks per side.
- An existing TFM2 match-record/replay dataset, its record count, draft coverage, schema, license, access, freshness, and exportability.
- Licenses for all community repositories listed in §5; public visibility alone is not a usage license.
- Current exact game patch on the Windows host and whether data files are readable in an installation available to the team.

## 7. Recommendation to the Research Lead

- **Phase 1:** A rule-only drafting state machine should not start until the Human Director reviews this audit, per project gate. After approval, implement only the named and verified 3/2 counts as a provisional setting; obtain in-game observation or data evidence for turn order and legality before claiming those details.
- **Outcome design:** Phase 1 must not assume a usable win/loss reward. First investigate installing/running the game and instrumenting its official simulator. A draft-only environment can be specified separately from outcome simulation, consistent with repository policy.
- **Biggest single risk:** No accessible and validated source currently maps candidate drafts to trustworthy match outcomes, so an RL reward may be unavailable.
- **If no outcome source is confirmed:** Reduce the first milestone to a verified draft-rule environment plus baseline/action analysis; report no win rate and make no claim of match competitiveness. Seek separate approval before adopting a self-built battle approximation as an exploratory model.

**Recommendation:** Research Lead should review and route this audit to the Human Director. The highest-value next action is securing a game installation and running a small, reproducible feasibility test of SDK/UI-driven matches; treat headless execution and outcome extraction as open questions until observed.
