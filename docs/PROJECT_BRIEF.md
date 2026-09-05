# GhostSpace — Project Brief

## Status
- Branch: `docs/product-definition`
- Repository: skeleton only (empty module stubs under `src/soccer_analysis/`, no implementation yet)
- This document defines product direction as of 2026-09-04.

## Purpose
GhostSpace is a soccer analytics portfolio project that measures the hidden value of off-ball movement. It exists to demonstrate applied technical ability (spatial analytics, data engineering, and — later — computer vision) while producing insights that are legible and useful to non-technical football people — coaches, scouts, analysts.

## Central Question
"What spatial changes occurred during an off-ball run, and what does a transparent ghost comparison reveal about the runner's contribution?"

## Audience & Goals
- Primary: portfolio piece for job search / interviews. The project direction was informed by feedback from a professional club analyst.
- Must be shareable publicly (LinkedIn) and understandable to a non-technical football audience — not only interesting to technical specialists.
- Must let the author defend every technical, mathematical, and football decision in an interview setting.

## Measurement Philosophy: What GhostSpace Does and Does Not Claim
Every metric and every piece of narration sits at one of three tiers:

1. **Observed spatial change** — a fact about the tracking data in this one sequence (e.g. "the nearest defender to Player X was 2.1m closer at t=4s than at t=0s"). The only tier v1 measures directly.
2. **Association within the sequence** — a plausible link between the run and a spatial change occurring around the same time. Phrased as coincidence/co-occurrence, never as causation.
3. **Causal claim** — that the run *caused* a defender's movement, a teammate's separation, or a later outcome. **GhostSpace v1 does not make claims at this tier** — a single sequence, with no controlled comparison and no counterfactual defender/teammate behaviour, cannot support one.

This distinction must be visible in the README, the coach-readable explanation, and any public write-up.

## Long-Term Vision (not built in the MVP)
broadcast footage → player detection & tracking → pitch-coordinate transform → spatial analytics → coach/scout-friendly explanation

The existing repo skeleton (`detection/`, `tracking/`, `transform/`, `team_assignment/`, `video_io/` modules) reflects this eventual pipeline. None of it is required for the MVP.

## MVP: The GhostSpace Run Impact Engine

### How the MVP analysis works
1. Select a SkillCorner sequence.
2. Identify the runner, the possible responding defender, and the benefiting teammate.
3. Define the start and end of the run.
4. Measure the observed changes (real tracking data — no ghost needed).
5. Construct the transparent ghost scenario (runner-only freeze — see below).
6. Recompute only the spatial metrics that are actually compatible with that scenario.
7. Animate and explain the result using observation/association language, never causal language.

### Core deliverables
1. One SkillCorner Open Data match.
2. One selected attacking sequence.
3. One identified, meaningful off-ball run.
4. An animated 2D pitch visual of the sequence, with the run and runner highlighted.
5. Two categories of measurement over the sequence:
   - **Observed sequence measurements** (from the real tracking data alone, no ghost involved): defender displacement during the selected time window, and the change in the benefiting teammate's nearest-defender distance across the real sequence. These describe the real data and its timing relative to the run — nothing more.
   - **Ghost sensitivity measurements** (actual vs. ghost, computed only where the metric genuinely depends on the altered position): the spatial-control/accessible-space approximation, and potentially passing-lane geometry.
6. A concise, coach-readable explanation covering both categories, framed as observation/association per the Measurement Philosophy above — not as a causal claim.

### Stretch (not required for MVP completion)
- A "progressive passing option" metric — only if it can be defined and validated responsibly within the week. Cut, not shipped half-done, if not.

### The ghost baseline: what it is and is not
The ghost scenario is a transparent geometric sensitivity analysis: it alters a player's position relative to what was actually observed, holds everyone else on their real observed trajectories, and recomputes only the metrics that genuinely depend on the altered position(s).

**Default MVP method — runner-only freeze:** only the runner's position is altered (e.g. held at their pre-run position); the benefiting teammate and every defender stay on their observed trajectory. This is the simplest and most defensible option, and it's what v1 uses. Because the teammate and defenders are unchanged, this method only supports the ghost-sensitivity measurements above — it cannot produce a meaningful actual-vs-ghost difference for defender displacement or the teammate's nearest-defender distance, since those players are identical in both scenarios by construction.

**Optional future exploration — illustrative paired freeze:** the runner and one carefully selected, associated defender (e.g. the player marking the runner) are both held at their pre-run positions, while everyone else follows their observed trajectory. This is not required to start the MVP and isn't decided now — it's worth reconsidering only after inspecting the data, if it would genuinely improve the case study. It would let the frozen defender's distance to the benefiting teammate differ between actual and ghost, but relies on a stronger assumption (that this specific defender would not have moved without the run) and must be presented as an **illustrative scenario**, not predicted player behaviour, a realistic alternate trajectory, or proof of causality.

Neither method simulates how the defence or teammates would truly have behaved without the run.

### Explicitly out of scope for v1
- Automatic analysis of arbitrary broadcast footage (the CV proof of concept stays separate and later)
- Full-match or multi-match player rankings (the "Scout view" aggregate)
- Deep reinforcement learning or learned trajectory-prediction models
- Production deployment / hosted app
- Causal claims of any kind (see Measurement Philosophy)
- Automatic (LLM-generated) tactical narration
- Every module in the current skeleton being implemented

### Two eventual views (only one is in scope now)
- **Coach view** (the MVP target): explain one sequence — what changed, who benefited, and what that might mean — worded as observation/association, not causation.
- **Scout view** (explicitly post-MVP): aggregate whether a player repeatedly generates useful off-ball movement across players/matches. Not attempted, prototyped, or scoped into the first week — it turns a single-sequence case study into a multi-match statistical claim, a different and much larger undertaking.

## What the SkillCorner-Based MVP Actually Demonstrates
This MVP is a data/analytics project built on tracking data someone else collected. It demonstrates:
- Python engineering
- Tracking-data ingestion and validation
- Data engineering (schema design, cleaning, reproducible pipelines)
- Spatial analytics (geometry, distance/space measurements)
- Data science (metric design, sensitivity analysis)
- Visualisation (the animated pitch view)
- Soccer interpretation (choosing a meaningful sequence, explaining it correctly)
- Communication (coach-readable output, public write-up)

It does **not** demonstrate building a computer-vision tracking system — SkillCorner already solved that. Computer vision is a planned second-stage integration (see Long-Term Vision), not something this MVP should be described as having proven.

## Data Source Strategy
- MVP: SkillCorner Open Data (public tracking data, Dynamic Events, phases of play) — reliable, pre-solved coordinates, avoids blocking the analytical product on solving computer vision first.
- Later: a small computer-vision proof of concept extracts positions from a short broadcast clip into the *same internal schema* used for the SkillCorner MVP, so the analytics layer doesn't need to be rewritten.

## Constraints & Deliverables
- Roughly one week of concentrated work for the MVP.
- Deliverables:
  - A polished README
  - One short animated demo suitable for LinkedIn
  - Reproducible code and tests
  - A methodology, assumptions and limitations write-up (including the Measurement Philosophy and ghost-baseline caveats above)
  - One coach-readable sequence explanation
  - (Optional, not required) a walkthrough notebook
- No hosted application is part of the MVP.

## Working Norms
- The user is sole git author; changes are proposed as diffs, not committed, pushed, merged, or opened as PRs by the assistant.
- Technical, mathematical, and football reasoning is explained so the user can defend it in an interview.
- For core analytical logic (metric definitions, ghost-baseline construction, space models), options and reasoning are presented; the user makes the call.
