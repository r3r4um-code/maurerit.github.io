---
title: "Organizing eve-market-calc: From 50 Issues to a Roadmap"
date: 2026-05-04T12:00:00-04:00
draft: false
tags: ["project-management", "eve-online", "process"]
---

eve-market-calc had 50+ issues, 20 labels, zero milestones, and no answer to "what are we shipping?"

90 minutes later, that was fixed. Here's what happened.

## The Problem

The app is real. It's been under active development for weeks with solid specs: recursive BOM expansion, invention cost modeling, 2-phase cart flow, market data. But the work lived in a flat issue list. No structure. No roadmap. No way to answer what comes next.

The mission is clear: replace the CEO's spreadsheets for EVE Online monthly manufacturing. Find opportunities, assign builds, tune to market data, export to LMEve. But everything beyond that MVP was scattered across conversations, not tracked anywhere.

## Milestones First

Before touching a single issue, I mapped the full product vision into six milestones with real scope:

| # | Milestone | What It Does |
|---|-----------|-------------|
| 1 | **Prospector** | v1.0 MVP — Replace spreadsheets. Find opportunities, assign builds, export to LMEve. |
| 2 | **Agent Runner** | Industry job tracking — members see what they're building and what's in progress. |
| 3 | **Citadel** | Harden vs-industry: H2→PostgreSQL, K8s, Ansible, inventory tracking. |
| 4 | **Cyno Link** | Connect planning → warehouse. Send build assignments to vs-industry automatically. |
| 5 | **Blueprint Original** | Refactor vs-industry to pull build data from the SDE instead of manual config. |
| 6 | **Warp** | Java→Go migration for warehouse backend. Low priority, no functional benefit yet. |

The naming is deliberate. Every EVE player knows what a cyno does — it bridges two points in space. That's what Cyno Link does between planning and warehouse. Prospector scans for value. Citadel fortifies. Blueprint Original pulls from the source of truth. The names carry the meaning; no need to explain each one separately.

## Map What Exists

With milestones defined, I audited all 21 open issues and assigned each to its milestone.

Prospector got the bulk: 16 issues covering recursive BOM expansion, invention tasks, purchase list exports, assignment sharing, and related specs and bugs.

The rest were thin:

- **Agent Runner:** 1 issue (blueprint import from ESI). Necessary, not central.
- **Citadel:** 1 issue (CI/CD workflows). Important but missing the actual hardening work.
- **Cyno Link:** 1 issue (purchase list integration). Only half the story.
- **Blueprint Original:** 2 issues (blocklist upstream). Not the SDE refactor itself.

Clear picture of coverage and gaps.

## Find the Gaps

This is where it got useful. With everything mapped, the missing work became obvious:

- **Prospector** missing: automated SQL insert transformation (last manual step in LMEve pipeline) and market volume tuning.
- **Agent Runner** missing: actual job tracking view members would see, ESI corporation industry job pipeline.
- **Citadel** missing: H2→PostgreSQL migration, K8s manifests, Ansible, inventory dashboard — all explicit requirements.
- **Cyno Link** missing: full assignment workload push, API contract spec.
- **Blueprint Original** missing: actual SDE refactor issue, table alignment spec for sharing between apps.

I created 12 new issues to fill each gap. Each assigned to the correct milestone, tagged Status/Need More Info for review.

## The Final Picture

| Milestone | Issues | Key Deliverables |
|-----------|:------:|------------------|
| Prospector | 18 | BOM expansion, invention, purchase list, market tuning, LMEve automation |
| Agent Runner | 3 | ESI corp jobs, job tracking view, blueprint import |
| Citadel | 5 | H2→PG, Kubernetes, Ansible, inventory dashboard, CI/CD |
| Cyno Link | 3 | Assignment workload push, purchase list integration, API contract |
| Blueprint Original | 4 | SDE refactor, table alignment, blocklist upstream |
| Warp | 0 | Intentionally empty |
| **Total** | **33** | |

## Why This Works

1. **Direction is obvious.** Prospector is the active milestone. Everything in it moves toward 1.0. No guessing.

2. **The issue list is actually usable.** Filter by milestone. See your 18 items. Work through them. Done scanning 50+ to find what matters.

3. **New contributors understand the vision in 30 seconds.** Milestones page. Full product roadmap. No guesswork.

4. **Dependencies are explicit.** Cyno Link depends on Citadel (warehouse must be hardened before integration). Blueprint Original depends on Prospector (SDE tables must exist before vs-industry consumes them). These were implicit; now they're structural.

5. **Nothing disappears.** The gap analysis surfaced 12 issues that existed only in conversation. Now they exist in the tracker, labeled, assigned, reviewable.

## The Labels Were Already Good

The repo had a solid labeling system before this:

- **Kind:** Bug, Feature, Enhancement, Documentation, Security, Testing
- **Priority:** Critical, High, Medium, Low
- **Status:** Abandoned, Blocked, Need More Info, Needs Spec, Needs Spec Reviewed
- **Reviewed:** Confirmed, Duplicate, Invalid, Won't Fix
- **Compat:** Breaking

Combined with milestones, you now have a complete project management stack. Labels for categorization, milestones for roadmap grouping, a clear path from idea to shipped.

## What's Next

Prospector is the immediate priority. 18 issues, several marked Priority/Critical with Reviewed/Confirmed — specced and ready to build.

The 12 new issues carry Status/Need More Info — they need review, refinement, and potentially specs before implementation. Intentional. They're placeholders for conversations that need to happen first.

Warp sits at zero. The Go migration is a distant goal. No sense cluttering the board until the milestones above it are stable.

---

## How This Actually Happened

This entire session was done through a Gitea API agent — milestones created, issues mapped, gaps identified, new issues written, all in about 90 minutes. The tools exist. The hard part isn't the API calls; it's the conversation that surfaces the real priorities and boundaries.

If you're staring at a flat issue list in your own repo, here's the playbook:

1. **Talk through the vision** until you can name the milestones
2. **Map existing work** to those milestones
3. **Ask "what's missing?"** for each one
4. **Create the gaps,** flag them for review
5. **Repeat quarterly** as the roadmap shifts

The collaboration model matters. An agent can execute the mechanics — creating issues, tagging them, organizing them into structure — but *you* decide what structure makes sense. The agent asks the hard questions. You answer based on what the product actually needs. Then the agent executes your vision at scale.

View the result: [git.r3r4um.online/r3r4um/eve-market-calc/milestones](https://git.r3r4um.online/r3r4um/eve-market-calc/milestones)

---

**Bottom line:** Structure enables motion. You can now answer "what are we shipping next?" and actually ship it without arguing about priorities or discovering forgotten dependencies mid-sprint.

90 minutes well spent.
