# ML-GA

Updated: 2026-03-16

A single-file, browser-based genetic algorithm (GA) simulation where a population of agents (dots) evolves movement DNA to reach a target while avoiding an obstacle.

## Files
- `MLGA101.html` — the full app (UI + simulation logic).
- `MLGA101.versions.json` — semantic version history and per-version profile defaults.

## Run locally
Open directly in a browser, or serve with a simple local server:

```bash
python -m http.server 8000
```

Then visit:

- `http://localhost:8000/MLGA101.html`

## Controls
- **Mutation**: mutation rate for child DNA.
- **Population**: number of agents per generation.
- **Speed**: simulation steps per animation frame.
- **Restart**: reinitialize with current slider values.

## How learning works
1. Each generation, every agent follows `DNA_LENGTH` acceleration vectors.
2. Fitness is based on distance to target, with a strong bonus for reaching target early and penalty for crashes.
3. Parents are selected with weighted probability by fitness.
4. Children are created via crossover + mutation, with elitism preserving top performers.

The cyan **best trail** is the highest-fitness agent trail seen across all generations (global best-so-far).

## Versioning
- Current app version: `v1.2.0`.
- Click the version in the footer to open the changelog modal.
- Clicking a version applies that profile and keeps the modal open so past and future versions remain visible.

