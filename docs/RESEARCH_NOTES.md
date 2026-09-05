# GhostSpace — Research Notes

## Purpose of this document
Tracks the research foundation behind GhostSpace's metrics, and translates each academic/industry concept into the specific, transparent measurement GhostSpace actually uses. Several sources below are cited at the level of their public abstract, article, or industry summary rather than a full re-implementation — that's flagged per entry.

## Language discipline
This project distinguishes three tiers of claim — observed spatial change, association within a sequence, and causal claim — and operates only at the first two. See `PROJECT_BRIEF.md`'s "Measurement Philosophy" for the full statement; it applies to every metric and every source cited here.

## Foundational sources

1. **SkillCorner Open Data**
   - Link: https://github.com/SkillCorner/opendata
   - Relevance: primary data source — tracking coordinates, Dynamic Events (including off-ball run classifications), and phases of play.
   - Use in GhostSpace: source of tracking data; Dynamic Events may help identify/validate candidate runs.
   - `[ ]` TBD: after downloading, validate the files locally — coordinate orientation, missing/interpolated frames, player-identity quality (ID swaps, substitutions), match metadata completeness, and which Dynamic Events are usable for candidate-sequence selection.

2. **SkillCorner — "Game Intelligence: Analysing Off-Ball Runs"**
   - Link: https://skillcorner.com/articles/game-intelligence-analysing-off-ball-runs
   - Relevance: industry framing for why off-ball movement is measurable and valuable; reviewed as a public article, not an internal methodology document.
   - Use in GhostSpace: context and motivation; SkillCorner's run taxonomy is a reference point, not necessarily reused verbatim.

3. **Fernández & Bornn — "Wide Open Spaces: A statistical technique for measuring space creation in professional soccer"** (MIT Sloan Sports Analytics Conference, 2018)
   - Link: https://barcainnovationhub.fcbarcelona.com/investigation/wide-open-spaces-a-statistical-technique-for-measuring-space-creation-in-professional-soccer/
   - Relevance: the core conceptual distinction GhostSpace is built on — a run's value isn't just where a player ends up (occupation), it's how much useful space the run creates for the team (generation). Motivates the actual-vs-ghost comparison.
   - Use in GhostSpace: conceptual framing only, reviewed at the level of the published paper/summary above. GhostSpace does not implement their full pitch-control/space-creation surface model — it uses simpler proxies.

4. **Spearman — "Beyond Expected Goals"** (12th MIT Sloan Sports Analytics Conference, 2018)
   - Citation: William Spearman. "Beyond Expected Goals." MIT Sloan Sports Analytics Conference, 2018. Author, title, venue, and year confirmed via the author's own reference to the talk; a stable public PDF/link was not confirmed while drafting this note.
   - Link: **pending** — add a confirmed link before this appears in any public write-up.
   - Relevance: shows how off-ball positioning changes scoring probability, and formalizes pitch control as a way to quantify space.
   - Use in GhostSpace: conceptual reference for the spatial-control/accessible-space approximation, reviewed at the level of public talks/summaries, not a re-implementation. GhostSpace uses a much simpler, transparent proxy rather than full pitch control + shot-probability modelling.

5. **Teranishi, Tsutsui, Takeda & Fujii — "Evaluation of Creating Scoring Opportunities for Teammates in Soccer via Trajectory Prediction"** (2022)
   - Citation: Masakiyo Teranishi, Kazushi Tsutsui, Kazuya Takeda and Keisuke Fujii. "Evaluation of Creating Scoring Opportunities for Teammates in Soccer via Trajectory Prediction." 2022. arXiv:2206.01899.
   - Link: https://arxiv.org/abs/2206.01899
   - Relevance: the closest academic analogue to GhostSpace's actual-vs-ghost comparison — comparing a player's actual trajectory against a predicted reference trajectory to evaluate the opportunity created for teammates.
   - Use in GhostSpace: conceptual template for the ghost baseline, reviewed at the level of the paper's abstract/framing. GhostSpace's ghost scenario is a much simpler, transparent geometric heuristic (freeze), not a trained predictive model — see `PROJECT_BRIEF.md`'s "ghost baseline: what it is and is not."

6. **StatsBomb — "StatsBomb Launch New 360 Metrics: Line-Breaking Passes and Ball Receipts in Space"**
   - Link: https://statsbomb.com/articles/soccer/statsbomb-launch-new-360-metrics-line-breaking-passes-and-ball-receipts-in-space-2/
   - Relevance: gives GhostSpace ready-made, interpretable metric definitions that scouts/analysts already recognize.
   - Use in GhostSpace: adapt nearest-defender-distance-style thinking directly; use ball-receipt-in-space and line-breaking-pass concepts to inform the (stretch) "progressive passing option" metric and the accessible-space measurement. Reviewed as a public article, not StatsBomb's internal implementation.

7. **"The Right Place at the Right Time: Advanced Off-Ball Metrics for Exploiting an Opponent's Spatial Weaknesses in Soccer"** (MIT Sloan Sports Analytics Conference)
   - Link: https://www.sloansportsconference.com/research-papers/the-right-place-at-the-right-time-advanced-off-ball-metrics-for-exploiting-an-opponents-spatial-weakenesses-in-soccer
   - Relevance: background on quantifying space and exploiting defensive weaknesses via off-ball metrics, including dynamic (not just static) defensive coverage.
   - Use in GhostSpace: background only, reviewed at the level of the paper's public abstract/summary. The MVP will likely use a static-distance or simple time-to-reach proxy rather than a full dynamic pitch-control model.

## GhostSpace's v1 metrics (transparent, interpretable)

**Observed sequence measurements** (real tracking data only — no ghost involved):
- Defender displacement during the selected time window.
- Change in the benefiting teammate's nearest-defender distance across the real sequence.

**Ghost sensitivity measurements** (actual vs. ghost — computed only where the metric genuinely depends on the altered position; see below):
- Change in the spatial-control/accessible-space approximation.
- Potentially, passing-lane geometry.

**Stretch (only if it can be defined and validated responsibly within the MVP timeframe):**
- Change in progressive passing options.

## The ghost baseline: what it is and is not
See `PROJECT_BRIEF.md` for the full statement. The v1 default is a **runner-only freeze** — a transparent geometric sensitivity analysis that alters only the runner's position while holding everyone else on their observed trajectories. An **illustrative paired freeze** (runner + one selected defender) is documented as optional future exploration, not a decision required for the MVP. Neither method simulates real alternate behaviour or proves causality.

## Open methodological questions (to settle before implementation)
- `[ ]` **Which defender(s) to report** for observed displacement during the window.
- `[ ]` **Nearest-defender distance** — straight-line Euclidean distance, or adjusted for defender speed/orientation?
- `[ ]` **Spatial-control/accessible-space approximation** — a hand-defined zone-value grid (e.g. weighted by distance to goal / central channel), or a lightweight pitch-control model (e.g. Voronoi tessellation)?
- `[ ]` **"Progressive passing option"** (stretch) — exact definition/threshold, and whether it can be validated responsibly in the available time; cut if not.
- `[ ]` **Analysis time window** — how many seconds before/after the run count as "the sequence," consistently applied to both actual and ghost versions.
- `[ ]` **SkillCorner Open Data validation** — coordinate orientation, missing/interpolated frames, player-identity quality, match metadata completeness, and which Dynamic Events are usable — to be documented once the data is actually inspected.

**Deferred, not an MVP blocker:** whether the illustrative paired-freeze ghost method would meaningfully improve the case study — revisit after inspecting candidate sequences.
