# 12.321
# Planetary Rotation and Climate Feedbacks in CESM

Configuration files, model settings, and analysis code for a series of CESM experiments investigating how planetary rotation rate influences cloud cover, planetary albedo, and climate behavior under different model configurations.

The project currently consists of two experiments:

- **Assignment 1:** Rotation-rate sensitivity experiments using F2000climo with prescribed sea surface temperatures.
- **Assignment 2:** Rotation-rate sensitivity experiments using B1850 with a fully coupled climate system.

## Assignment 1: Rotation Rate, Cloud Cover and Albedo in CESM

Investigates how changes in planetary rotation rate (0.5× and 2.0× Earth's rotation rate) affect cloud fraction and planetary albedo in an atmosphere model configuration with prescribed sea surface temperatures.

### Model configuration

| Item | Value |
|---|---|
| CESM version | 2.1.5 |
| Compset | F2000climo |
| Resolution | f19_f19_mg17 |
| Run length | 3 years: 1 year, followed by a 2-year extension from restart |
| Spin-up discarded | First 12 months |
| Cases | Control (Ω₀ = 7.292115e-5 s⁻¹), 0.5×Ω₀ (3.6460575e-5), 2.0×Ω₀ (1.4584230e-4) |

### How the rotation rate was changed

The rotation rate was changed through the CAM namelist (`user_nl_cam`); no source code was modified (no SourceMods). Each experiment adds one line:

| Case | `user_nl_cam` entry | Value (s⁻¹) |
|---|---|---|
| Control | (none; default) | 7.292115e-5 |
| 0.5×Ω₀ | `echo omega = 3.6460575e-5` | 3.6460575e-5 |
| 2.0×Ω₀ | `echo omega = 1.4584230e-4` | 1.4584230e-4 |

## Assignment 2: Rotation Rate and Climate Response in a Fully Coupled CESM

Investigates how changes in planetary rotation rate affect the climate system when the atmosphere, ocean, and other model components interact in a fully coupled configuration.

Unlike Assignment 1, which uses prescribed sea surface temperatures, this experiment uses a fully coupled pre-industrial climate configuration. This allows the ocean and atmosphere to respond interactively to changes in planetary rotation.

### Model configuration

| Item | Value |
|---|---|
| CESM version | 2.1.5 |
| Compset | B1850 |
| Resolution | f19_g17 |
| Run length | 75 months |
| Spin-up discarded | First 24 months |
| Cases | Control (Ω₀ = 7.292115e-5 s⁻¹), 0.5×Ω₀ (3.6460575e-5), 2.0×Ω₀ (1.4584230e-4) |

### How the rotation rate was changed

[Describe how the rotation rate was modified in the B1850 experiments. Specify whether the same CAM namelist parameter (`omega`) was used and whether any additional configuration changes were required.]

### Key differences from Assignment 1

| Feature | Assignment 1 | Assignment 2 |
|---|---|---|
| Compset | F2000climo | B1850 |
| SST treatment | Prescribed | Interactive / fully coupled |
| Climate configuration | Present-day climatological forcing | Pre-industrial |
| Ocean-atmosphere interaction | No interactive ocean response to atmospheric changes | Fully coupled |
| Main focus | Cloud fraction and planetary albedo | Climate response and coupled feedbacks |

## Repository contents

- `config/`: Modified source files and namelist settings for each experiment.
- `Analysis.ipynb`: Analysis and figures.
- `figures/`: Figures used in the reports.

## Reproducing the analysis

1. Set `RUN_BASE` at the top of the relevant notebook to your CESM archive directory.
2. Install the required Python packages (e.g., `xarray`, `numpy`, `matplotlib`, and `cartopy`).
3. Run all notebook cells from top to bottom.

**Note:** Each assignment uses a different CESM configuration and experiment setup. Refer to the corresponding assignment section when reproducing or interpreting the results.
