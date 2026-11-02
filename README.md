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

### How the rotation rate was changed

Unlike the F2000climo experiments in Assignment 1, the fully coupled B1850 configuration requires the planetary rotation rate to be modified consistently across all relevant model components, including the atmosphere (CAM6), ocean (POP2), and sea ice (CICE).

The rotation rate was modified in two places:

1. **CAM namelist (`user_nl_cam`):** The `omega` parameter was set to the target planetary rotation rate for each experiment.
2. **Shared source code (`shr_const_mod.F90`):** A modified copy of `shr_const_mod.F90` was placed in `SourceMods/src.share/` for each perturbed case. The value of `SHR_CONST_SDAY` was changed to ensure that the shared planetary rotation constant was consistent with the modified rotation rate.

In CESM, the shared angular rotation frequency is defined as:

`SHR_CONST_OMEGA` = 2π / `SHR_CONST_SDAY`

The default sidereal day is 86,164 seconds. Therefore, the sidereal day was doubled for the 0.5× rotation experiment and halved for the 2.0× rotation experiment.

| Case | CAM namelist (`omega`) | `SHR_CONST_SDAY` (s) |
|---|---:|---:|
| Control (1.0×Ω₀) | 7.292115e-5 | 86164.0 |
| 0.5×Ω₀ | 3.6460575e-5 | 172328.0 |
| 2.0×Ω₀ | 1.4584230e-4 | 43082.0 |

The control case retained the default source code and namelist settings. For the perturbed cases, both the CAM namelist and the shared source constant were modified to maintain consistency across the coupled model components.

The modifications were verified by:
- Checking the resulting CAM namelist values in `CaseDocs/atm`.
- Inspecting `SHR_CONST_SDAY` in `SourceMods/src.share/shr_const_mod.F90` using `grep`.
- Confirming from the CESM build log that the modified source file was compiled.

No other source-code modifications were made specifically to change the planetary rotation rate.

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
- `Analysis*.ipynb`: Analysis and figures.

## Reproducing the analysis

1. Set `RUN_BASE` at the top of the relevant notebook to your CESM archive directory.
2. Install the required Python packages (e.g., `xarray`, `numpy`, `matplotlib`, and `cartopy`).
3. Run all notebook cells from top to bottom.

**Note:** Each assignment uses a different CESM configuration and experiment setup. Refer to the corresponding assignment section when reproducing or interpreting the results.
