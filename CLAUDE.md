# CLAUDE.md

## Role
Claude: reasoning, research, architecture decisions, literature review, reports, planning.
**Claude never writes code.** When code work is needed, write a `[Claude → Codex]` entry in HANDOFF.md.

## Project in one paragraph

KMITL (International Aviation Industry College) × Royal Thai Army, funded by NBTC (กทปส) Section 52(2). We build a Φ-OTDR DAS fiber-optic intrusion detection system for Thailand's forested border regions. Buried fiber detects ground-coupled vibrations; an AI classifier (CNN / Hybrid NN, deployed on FPGA) identifies the event type (human, vehicle, animal, background) and alerts operators via web dashboard and mobile app. Pilot: 5 km at กองกำลังสุรศักดิ์มนตรี. No public Thai fiber vibration dataset exists — we collect our own. MOU with military required before field work. Full context: **PRODUCT.md**. Technical contract: **AGENTS.md**.

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
