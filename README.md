# Energy Systems — Group Assignment 2026

Build a two-node **capacity expansion + dispatch model** from scratch and analyse the value of
transmission. The assignment is worth **5 bonus points** (Task 1 = 3 pts, Task 2 = 2 pts).

## Quick start

You need [pixi](https://pixi.sh) (a fast conda-based package manager). If you don't have it:

```bash
curl -fsSL https://pixi.sh/install.sh | sh   # macOS / Linux
# or on Windows: winget install prefix-dev.pixi
```

Then in the repo root:

```bash
pixi install      # set up the environment (first time only, ~2 min)
pixi run lab      # open JupyterLab in your browser
```

Open `students/assignment.ipynb` and work from top to bottom.

## Repo layout

```
├── pixi.toml              # reproducible environment (conda-forge, pinned)
├── pixi.lock
└── students/
    ├── assignment.ipynb   # your notebook — fill in the TODO cells
    └── data/
        ├── tech_data.csv          # techno-economic parameters (costs, lifetimes, efficiencies)
        ├── load.csv               # demand per node (north, south) + season label
        └── capacity_factors.csv   # wind/solar capacity factors per node + season label
```

**Do not modify the CSV files.** They are the shared input data — every group works from
identical inputs.

## Assignment overview

| | |
|---|---|
| **Scenarios** | A — transmission line as a free investment variable; B — line fixed to zero |
| **Nodes** | North (DK, wind-rich) and South (ES, solar-rich) |
| **Generation** | Wind, Solar |
| **Storage** | H₂ (long-duration) — *separate* electrolyser and fuel-cell power capacities; Battery (short-duration) — *single symmetric* inverter power capacity |
| **Time series** | 4 representative weeks (winter/spring/summer/autumn), 3-hourly → 224 snapshots |
| **Solver** | HiGHS (open-source, included) — `solver_name="highs"` |

## Input data

- **Capacity factors and demand** come from a 49-node European PyPSA network (2019 weather
  year). The windiest node (DK1) is *North*, the sunniest (ES0) is *South*. The year is
  represented as four seasonal weeks at 3-hourly resolution (224 snapshots total). Demand is
  scaled to representative levels (north ≈ 30 GW, south ≈ 45 GW mean).
- **Costs** in `tech_data.csv` are aligned with a `costs_2050` dataset. Annualise them
  yourself using the annuity formula at `r = 7%` (Task 1a).

## Modelling notes

- Each snapshot represents Δt = 3 h — use this in the storage SOC dynamics and operating cost.
- Pro-rate the annualised investment to the modelled window with `f = T/8760 ≈ 0.077` so that
  CAPEX and OPEX are on the same time basis and the nodal prices (balance dual ÷ Δt) come out
  in realistic EUR/MWh.

## Submission

Submit a **PDF report (max 8 pages)** plus your completed `assignment.ipynb` via ISIS by
**14 June 2026, 23:59**. The notebook must be reproducible (run top-to-bottom without errors).
Presentation: **17 June 2026 at 10:00** (mandatory for the bonus).
