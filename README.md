# 12.321
# Assignment 1: Rotation Rate, Cloud Cover and Albedo in CESM

Configuration files and analysis code for a CESM sensitivity test: how do 0.5x and
2.0x planetary rotation rates change cloud fraction and albedo?

## Model configuration

| Item | Value |
|---|---|
| CESM version | 2.1.5 |
| Compset | F2000climo |
| Resolution | f19_f19_mg17 |
| Run length | 3 years: 1 year, then a 2-year extension from restart |
| Spin-up discarded | first 12 months |
| Cases | control (Ω0 = 7.292115e-5 s^-1), 0.5×Ω0 (3.6460575e-5), 2.0×Ω0 (1.4584230e-4) |

## How the rotation rate was changed

The rotation rate was changed through the CAM namelist (`user_nl_cam`); no source
code was modified (no SourceMods). Each experiment adds one line:

| Case | `user_nl_cam` entry | Value (s^-1) |
|---|---|---|
| control | (none, default) | 7.292115e-5 |
| 0.5×Ω0 | `echo "omega = 3.6460575e-5` | 3.6460575e-5 |
| 2.0×Ω0 | `echo "omega = 1.4584230e-4` | 1.4584230e-4 |

## Repository contents

- `config/` : modified source files and namelist settings for each case
- `Analysis.ipynb` : analysis and figures
- `figures/` : figures used in the report

## Reproducing the analysis

1. Set `RUN_BASE` at the top of the notebook to your CESM archive directory.
2. Install the packages: [xarray, numpy, matplotlib, cartopy, ...].
3. Run all cells from top to bottom.
