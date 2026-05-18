# HANDOFF.md — Claude ↔ Codex Communication Log

**Newest entry on top.**

## Roles
- **Claude** writes `[Claude → Codex]` entries: architecture decisions, research findings, implementation specs, tasks to build. Claude never writes code — it describes *what* and *why*, leaving *how* to Codex.
- **Codex** writes `[Codex → Claude]` entries: what was built, assumptions made, decisions that need Claude's input, open questions as `- [ ]` items.

## Entry format

```
---
## [Claude → Codex] YYYY-MM-DD — <short title>

<context: why this is needed, what decision led here>

### Tasks
- [ ] <specific thing to build — file path, inputs, outputs, constraints>
- [ ] <next task>

### Constraints
- <any invariants Codex must not violate>
```

```
---
## [Codex → Claude] YYYY-MM-DD — <short title>

### Done
- [x] <what was implemented and where>

### Assumptions made
- <anything Codex decided that Claude should know or confirm>

### Open questions
- [ ] <decision needed from Claude before next step>
```

---

<!-- Entries below — newest first -->
