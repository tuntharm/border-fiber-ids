# CLAUDE.md — Border Fiber IDS

Auto-loaded by Claude Code every session. Keep this file self-contained so any collaborator or AI assistant can orient from it alone.

---

## Project context

**Partners:** King Mongkut's Institute of Technology Ladkrabang (KMITL) × Thai government border security.

**Goal:** Detect and classify ground-coupled vibration events caused by illegal border crossings, contraband drops, and related activity in remote forested border regions of Thailand. The sensor medium is buried fiber-optic cable deployed along the perimeter.

**Sensing modality — WORKING ASSUMPTION: Distributed Acoustic Sensing (DAS)**
DAS interrogates the fiber with laser pulses and measures Rayleigh backscatter phase shifts to reconstruct vibration at every gauge-length section along the cable. Each section becomes a virtual sensor, giving thousands of simultaneously sampled channels along a single fiber strand.
> **⚠ FLAG THIS SECTION** if the sensing modality is confirmed or changes (e.g., FBG arrays, Φ-OTDR variant, hybrid). Update channel-count assumptions, physics notes, and I/O module accordingly.

**PI background:** The PI has prior experience in structural health monitoring (SHM) with sensor-based impact detection and classification. SHM analogues (modal analysis, short-time windowing, feature banks from accelerometer signals) are welcome and useful here — the genuinely new piece is the long spatial dimension along the fiber.

---

## Event taxonomy

Starting taxonomy — will evolve as labeled data accumulates:

| Label | Description |
|---|---|
| `human_footstep` | Single person or small group on foot |
| `vehicle_light` | Motorcycle, ATV, light truck |
| `vehicle_heavy` | Truck, heavy machinery |
| `digging` | Manual or mechanical excavation |
| `dropped_object` | Impact from a dropped item (contraband drop) |
| `animal` | Wildlife movement (deer, boar, etc.) |
| `environmental` | Rain, wind, distant road traffic — **background / negative class** |

When adding new labels: update this table, `configs/baseline.yaml` → `data.classes`, and retrain baselines.

---

## Technical approach

**End-to-end pipeline:**

```
interrogator hardware
    → raw DAS data (HDF5 / .sgy / .tdms)
    → denoise  (bandpass, SVD/modal filtering)
    → segment  (sliding window, trigger-based)
    → feature extraction  (spectrograms, waveforms, spatial features)
    → classifier  (CNN / CRNN / spatio-temporal model)
    → spatio-temporal localization along fiber
    → alert
```

**Baseline models to compare:**

1. **2D-CNN** — per-channel mel/log spectrograms as image input.
2. **CRNN** — CNN front-end for spectral features + BiLSTM for temporal context across windows.
3. **Spatio-temporal model** — treats fiber position as a second spatial axis; full (time × space) tensor input to a 3-D or factored conv architecture.

Metrics: per-class precision / recall / F1, macro-F1, confusion matrix, and localization error (meters along fiber) where ground truth is available.

---

## Tech stack

| Role | Library |
|---|---|
| Deep learning | PyTorch |
| Signal processing | numpy, scipy, obspy |
| File I/O | h5py (HDF5); later .sgy, .tdms via wrappers |
| Classical baselines & metrics | scikit-learn |
| Config management | PyYAML |
| Visualization | matplotlib |
| Progress bars | tqdm |
| Linting / formatting | ruff |
| Type checking | mypy |
| Testing | pytest |

Python ≥ 3.11 required (match `pyproject.toml`).

---

## Repo conventions

- **One config per experiment.** Configs live in `configs/`. Copy and edit `baseline.yaml` for each new experiment.
- **Experiment outputs** go in `experiments/<YYYY-MM-DD>_<short-name>/`. This directory is gitignored; store a pointer in the experiment config or a notebook.
- **Type hints required** on all public functions. Run `mypy src/` before committing.
- **Signal functions must document** in their docstring:
  - `sample_rate` (Hz)
  - `n_channels` (number of DAS channels / virtual sensors)
  - input/output units (e.g., nm/s, rad, dB)
- **No data in the repo.** See Sensitive data policy below.
- Subpackage layout: `src/<package>/__init__.py` is the public API surface; heavy implementation goes in `src/<package>/<module>.py`.

---

## Sensitive data policy

This is border-security research. Handle accordingly:

- **Never commit** raw fiber recordings, GPS coordinates, labeled intrusion clips, or any file that could reveal deployment topology.
- `data/raw/`, `data/processed/`, `data/labels/`, `data/synthetic/`, and `experiments/` are all gitignored. Only `.gitkeep` placeholders are tracked.
- Binary data patterns (`*.h5`, `*.hdf5`, `*.sgy`, `*.npy`, `*.pt`, etc.) are globally gitignored as a second line of defense.
- `docs/reports/` is for external-stakeholder documents. Review every file in this directory before sharing or pushing.
- When in doubt, do not commit. Ask first.

---

## Open questions

Fill these in as partners confirm decisions:

| # | Question | Status |
|---|---|---|
| 1 | Sensing modality confirmed? (DAS / FBG / Φ-OTDR variant?) | ⬜ Open |
| 2 | Fiber type and burial depth? (SM / MM; conduit vs. direct) | ⬜ Open |
| 3 | Interrogator hardware? (Make/model, sample rate, gauge length, channel spacing) | ⬜ Open |
| 4 | Deployment topology? (Total fiber length, terrain, repeater locations) | ⬜ Open |
| 5 | Primary detection targets for v1? (Humans only, or vehicles too?) | ⬜ Open |
| 6 | Labeling strategy? (Manual annotation, triggered recording, synthetic?) | ⬜ Open |
| 7 | Real-time latency requirement? (Alert within N seconds?) | ⬜ Open |
| 8 | Compute environment for inference? (Edge device, on-prem server, cloud?) | ⬜ Open |

---

## Directory map

```
src/io/             File readers: HDF5, .sgy, .tdms → common tensor format
src/preprocessing/  Denoising, bandpass, normalization, windowing
src/features/       Spectrogram, waveform, spatial feature extractors
src/models/         CNN, CRNN, spatio-temporal architectures
src/training/       Training loops, loss functions, schedulers
src/evaluation/     Metrics, confusion matrix, per-class reports
src/localization/   Fiber-position estimation from multi-channel output
src/pipeline/       End-to-end orchestration (offline and streaming)
configs/            One YAML per experiment
experiments/        Gitignored run outputs (checkpoints, logs, results)
notebooks/          Exploratory analysis (not for production code)
docs/literature/    Papers and bibliography.bib
docs/reports/       External-stakeholder documents (review before sharing)
docs/meetings/      Meeting notes
tests/              pytest test suite
```
