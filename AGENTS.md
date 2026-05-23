# AGENTS.md — Technical Contract

## Agent roster

| Agent | Model | Spawned by | Role |
|-------|-------|-----------|------|
| `critic` | opus | Claude | Stress-tests decisions (read-only) |
| `litreview` | haiku | Claude | Literature search, NotebookLM |
| `reporter` | sonnet | Claude | Reports for KMITL / Thai gov |
| `ml` | sonnet | Codex | Model code, training, evaluation |
| `signals` | sonnet | Codex | Signal processing, feature extraction, I/O |

Briefings: `.claude/agents/<name>.md` (Claude Code stubs) — full briefing text is embedded there.

### Standard team startup
Spawn all 5 in parallel with `team_name: border-fiber-ids`. Models as above. Orient each with: Φ-OTDR DAS confirmed, FPGA inference target, KMITL × Royal Thai Army partner, NBTC funder.

---

## File ownership

| Path | Owner | Notes |
|------|-------|-------|
| `src/` | Codex | All implementation code |
| `tests/` | Codex | pytest suite |
| `configs/` | Codex | One YAML per experiment |
| `experiments/` | Codex | Gitignored run outputs |
| `docs/literature/` | Claude (litreview) | bibliography.bib + topic summaries |
| `docs/reports/` | Claude (reporter) | Review before sharing |
| `docs/meetings/` | Claude | Meeting notes |
| `HANDOFF.md` | Both | Communication log |
| `PRODUCT.md` | Claude | Business/product context |
| `AGENTS.md` | Claude | This file — technical contract |

---

## Shared data schema

All I/O readers must return a common dict:
```python
{
    "data":            np.ndarray,   # shape [n_channels, n_samples], float32
    "sample_rate":     float,        # Hz
    "channel_spacing": float,        # metres along fiber
    "units":           str,          # e.g. "rad", "nm/s", "counts"
    "file_path":       str,
}
```
Hardware constants (sample rate, gauge length, channel spacing) are **never hardcoded** — always read from config or file header.

---

## Repo conventions

- Python ≥ 3.11. Type hints on all public functions.
- Signal functions must document `sample_rate`, `n_channels`, input/output units in docstring.
- No data in repo: `*.h5`, `*.hdf5`, `*.sgy`, `*.npy`, `*.pt`, GPS coords are gitignored.
- One YAML config per experiment in `configs/`. Copy `baseline.yaml` as starting point.
- Subpackage public API in `src/<pkg>/__init__.py`; implementation in `src/<pkg>/<module>.py`.
- Linting: `ruff`. Type checking: `mypy src/`. Both must pass before marking a task done.
- No comments explaining what code does — only non-obvious WHY.

---

## Tech stack

| Role | Library |
|---|---|
| Deep learning | PyTorch |
| Signal processing | numpy, scipy, obspy |
| File I/O | h5py; .sgy / .tdms via wrappers |
| Classical ML / metrics | scikit-learn |
| Config management | PyYAML |
| Linting / typing | ruff, mypy |
| Testing | pytest |

---

## Confirmed signal processing pipeline (from NBTC proposal Section 13)

```
Φ-OTDR interrogator
  → laser pulse into fiber → Rayleigh backscatter phase retrieval
  → raw vibration time-series [n_channels × n_samples]
  → Adaptive Filtering      (time-varying noise; adapts parameters to incoming data)
  → Wavelet Analysis        (time-frequency decomposition; detects events at different freq)
  → Fourier Transform       (time → frequency domain; SNR improvement)
  → Signal Image            (Spectrogram or Vibration Pattern Map)
  → CNN / Hybrid NN         (pattern recognition → event classification)
  → position + event type
  → Web dashboard / Mobile alert (RBAC)
```

**ML model candidates (per proposal):**
- ANN — fast, real-time capable
- CNN — learns spatial/spectral features from signal images; primary candidate
- RNN — temporal dependencies across windows
- Hybrid NN (CRNN) — CNN front-end + RNN; primary production candidate

**FPGA constraint:** model must fit FPGA resource budget (no cloud, no edge CPU). Avoid architectures with large memory footprints or dynamic control flow. Prefer fixed-size convolutions.

---

## Project risks (from NBTC proposal Section 15)

1. Hardware damage in harsh field environment → reduced system performance
2. False alarms from non-target vibrations (rain, nearby traffic, wind)
3. Environmental factors (temperature, humidity, obstructions) reducing accuracy — requires sufficient dataset diversity
4. Soil density variation across deployment area affects signal propagation and analysis
5. Installation errors affecting reliability and alert trustworthiness

---

## Open hardware questions

| # | Question | Status |
|---|---|---|
| 1 | Sensing modality | ✅ Φ-OTDR DAS (Rayleigh backscatter) |
| 2 | Fiber type / burial depth | ⬜ Open |
| 3 | Interrogator make/model, sample rate, gauge length | ⬜ Open — FPGA processing confirmed |
| 4 | Deployment topology (full length, terrain) | 🟡 5 km pilot at Suranaree Military Force area |
| 5 | Labeling strategy | 🟡 Real-field data collection, supervised learning |
| 6 | Real-time latency bound | ✅ Real-time required (FPGA for low latency) |
