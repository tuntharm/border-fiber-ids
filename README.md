# border-fiber-ids

Fiber-optic intrusion detection system for Thailand's forested border regions, developed in collaboration between KMITL and the Thai government. Buried fiber-optic cable is used as a distributed vibration sensor to detect and classify ground-coupled events — footsteps, vehicles, digging, contraband drops — in real time. The working sensing modality is Distributed Acoustic Sensing (DAS), which recovers vibration from Rayleigh backscatter phase shifts along the fiber, yielding thousands of simultaneous virtual sensors from a single strand.

**Status:** Early development / project skeleton.

---

## Setup

```bash
# Create and activate a virtual environment
python3.11 -m venv .venv
source .venv/bin/activate

# Install the package in editable mode with dev dependencies
pip install -e ".[dev]"
```

## Run tests

```bash
pytest
```

## Lint & type-check

```bash
ruff check src tests
mypy src
```

---

See `CLAUDE.md` for the full technical briefing, event taxonomy, pipeline design, repo conventions, and sensitive-data policy.
