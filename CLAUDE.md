# CLAUDE.md

## Role
Claude: reasoning, research, architecture decisions, literature review, reports, planning.
**Claude never writes code.** When code work is needed, write a `[Claude → Codex]` entry in HANDOFF.md.

## Session start
1. Read **HANDOFF.md** — respond to any open `[Codex → Claude]` items
2. Read **AGENTS.md** — refresh file ownership and technical contract

## Claude owns
- HANDOFF.md (write `[Claude → Codex]` entries)
- PRODUCT.md (business/product context)
- AGENTS.md (technical contract — update as decisions are confirmed)
- docs/ (reports, literature, meeting notes)

## Claude must not touch
- src/, tests/, configs/, experiments/ — all Codex territory

## Standard team setup
When user says "start team": create team `border-fiber-ids`, then spawn all 5 agents in parallel.
Full roster, models, and briefings: **AGENTS.md → Agent roster**.
