# Milestones to Net Zero: Intermodel Insights on the EU’s 2040 Climate Targets

Data, code, and results companion repository for the paper:

> **Milestones to Net Zero: Intermodel insights on the EU’s 2040 climate targets**  
> *Pietzcker et al.*

Repository: <https://github.com/Renato-Rodrigues/ecemf-wp1-mip-paper>  
Archived release: Zenodo DOI *to be added when the release is archived* (see [Citation](#citation))

## Goal & Purpose

The goal of this repository is to store all necessary information, data, and code to make the paper results fully reproducible and to serve as an official companion repository to the manuscript and its Supplementary Information.

The paper is a model intercomparison (ECEMF Work Package 1). The scenarios were computed by ten energy-system and integrated assessment models (IMAGE, PRIMES, PROMETHEUS, REMIND, TIAM-ECN, WITCH, LIMES, MEESA, OSeMBE, Euro-Calliope) and submitted to the ECEMF Scenario Explorer hosted by IIASA. **This repository therefore contains the model *outputs* (scenario results), not the models themselves.** The R code here harmonises those outputs, computes every number quoted in the manuscript and draws all figures. Information on the models are given in [Models used to produce the scenario data](#models-used-to-produce-the-scenario-data).

## Contents

```
data/WP1_iiasa_2026_08_03.xlsx   scenario results from the IIASA ECEMF Scenario Explorer, reduced
                                 to the 96 variables the code reads, region EU27 & UK (*), the five
                                 scenarios used, and the 5-year time steps from 2005 on
data/historical.mif              historical data (UNFCCC, Ember, IEA, IRENA, BP; region EUR = EU27 + UK,
                                 1990-2024) in REMIND's historical.mif format
prepare_data.R                   reads both files and derives the variables the figures need
paperFigures.Rmd                 results table, then the nine published figures
paperFigures.html                rendered output of paperFigures.Rmd (results table plus figures)
output/png, output/svg           the figures, written on every render
renv.lock                        exact versions of R and of every R package used for testing
LICENSE                          GNU AGPL v3 (code)
CITATION.cff                     citation metadata for this repository
```

## 1. System requirements

### Software dependencies

| Dependency | Tested version | Notes |
| --- | --- | --- |
| R | 4.3.2 | any R ≥ 4.3 is expected to work |
| Pandoc | 3.8.3 | needed by `rmarkdown`; bundled with RStudio |
| rmarkdown | 2.32 | CRAN |
| knitr | 1.52 | CRAN |
| tidyr | 1.3.1 | CRAN |
| dplyr | 1.2.1 | CRAN |
| ggplot2 | 4.0.3 | CRAN |
| gridExtra | 2.3 | CRAN |
| ggh4x | 0.3.0 | CRAN |
| ggrepel | 0.9.6 | CRAN |
| svglite | 2.1.3 | CRAN |
| scales | 1.4.0 | CRAN |
| readxl | 1.5.0 | CRAN (used by `quitte` to read the xlsx file) |
| quitte | 0.3151.0 | PIK, <https://github.com/pik-piam/quitte> (`calc_addVariable`, `calcCumulatedDiscount`, `read.quitte`, …) |
| mip | 0.155.14 | PIK, <https://github.com/pik-piam/mip> (model and technology colour styles) |

`grid` ships with R. The complete dependency tree (119 packages, with versions and source repositories) is pinned in [`renv.lock`](renv.lock).

### Operating systems

- Tested on: Windows 11 Pro (build 26100), x86-64.
- The code is plain, platform-independent R and is expected to run unchanged on Linux and macOS with the same package versions. Those platforms were not tested for this release.

### Hardware

No non-standard hardware is required. The tests ran on a laptop (Intel Core i7-1365U, 32 GB RAM). A render peaks at about 0.4 GB of RAM (measured), and the outputs need about 5 MB of disk space.

## 2. Installation guide

### Instructions

1. Install R (≥ 4.3) from <https://cran.r-project.org>.
2. Install Pandoc from <https://pandoc.org/installing.html>, or install RStudio, which bundles it.
3. Clone or download this repository:
   ```sh
   git clone https://github.com/Renato-Rodrigues/ecemf-wp1-mip-paper.git
   cd ecemf-wp1-mip-paper
   ```
4. Install the R packages. Use **either** option.

   **Option A: latest package versions** (quickest)
   ```r
   options(repos = c(CRAN    = "https://cloud.r-project.org",
                     pikpiam = "https://pik-piam.r-universe.dev",
                     pik     = "https://rse.pik-potsdam.de/r/packages"))
   install.packages(c("rmarkdown", "knitr", "tidyr", "dplyr", "quitte", "ggplot2",
                      "gridExtra", "ggh4x", "ggrepel", "mip", "svglite"))
   ```

   **Option B: exact tested versions** (recommended for strict reproduction)
   ```r
   install.packages("renv")
   renv::restore()   # reads renv.lock in the repository root
   ```

### Typical install time

Estimated, not measured on a clean machine: about 5 minutes on a normal desktop computer when CRAN offers binary packages for your R version, and 15–30 minutes when all 119 packages must be compiled from source (e.g. on Linux, or with Option B). As a reference, installing the 13 top-level packages and missing dependencies from source on the test laptop took 4 min 9 s.

The results are not sensitive to package versions: rendering with the latest versions available on 2026-10-02 (`quitte` 0.3152.0, `mip` 0.156.0, `ggh4x` 0.3.1, `svglite` 2.2.2) gives an identical results table.

## 3. Demo

The full dataset is small (about 0.4 MB), so the demo is the complete analysis.

### Instructions to run on data

From the repository root, run

```r
rmarkdown::render("paperFigures.Rmd")
```

or, from a shell,

```sh
Rscript -e 'rmarkdown::render("paperFigures.Rmd")'
```

### Expected output

- `paperFigures.html`: the table *"Result values quoted in the manuscript and Supplementary Information"* (min, quartiles, median and max across models for every quantitative statement in the paper), followed by all figures.
- `output/png/*.png` and `output/svg/*.svg`: the nine figures listed below (18 files).

Check against the first table rows: the GHG emission reduction vs 1990 (2030-target accounting) has a median of **85 %** in 2040 (interquartile range 83–86 %) and **100 %** in 2050. The table in a fresh render is identical to the committed `paperFigures.html`. Figure files may differ byte-wise across `svglite`/graphics-device versions (anti-aliasing, SVG markup) but are visually identical.

### Expected run time

About 40 seconds on a normal desktop computer (39 s measured on the test laptop listed above, R 4.3.2, Windows 11).

## 4. Instructions for use

### How to run the software on your data

The code reads scenario data in the IAMC format (columns `model, scenario, region, variable, unit, <years>`), as exported from any IIASA Scenario Explorer, and historical data in REMIND's `.mif` format.

1. Download a new snapshot from the ECEMF Scenario Explorer (<https://ecemf.apps.ece.iiasa.ac.at>) as xlsx. Alternatively, write your own model results in the same format.
2. Place the file in `data/` and change the file name in `prepare_data.R`:
   ```r
   df <- suppressWarnings(quitte::read.quitte("./data/<your file>.xlsx")) %>% ...
   ```
3. If your file uses different scenario names, model names or region, adjust the `filter()` call directly below that line (scenarios `WP1 NetZero`, `WP1 NPI`, `WP1 NetZero-LimBio`, `WP1 NetZero-LimNuc`, `WP1 NetZero-LimCCS`; region `EU27 & UK (*)`). Then adjust the model groups `modelsFullSystem` and `modelsElecall` and the `color` list at the top of `prepare_data.R`.
4. Render `paperFigures.Rmd` as in the demo.

Variables that a model does not report are skipped (`calc_addVariable(..., completeMissing = TRUE)`). Model-specific corrections, such as re-labelling syngas, filling missing GDP or bunker emissions, or synthetic LULUCF and non-CO2 trajectories for models without them, are grouped per model in `prepare_data.R` and commented there. The paper's Methods section describes the approach.

### Reproduction instructions

Every quantitative result of the paper is reproduced by the single render command above:

| Manuscript item | Produced by | Output |
| --- | --- | --- |
| Numbers quoted in the text and SI | chunk `results_table` (values computed in the chunks above it) | table in `paperFigures.html` |
| Figure 1 | chunks `figure_1`, `figure 1.b`, `figure 1` | `output/*/Figure 1. Emissions panel.*` |
| Figure 2 | chunk `SE_Electricity_VRE` | `output/*/Figure 2. Electricity generation from wind and solar.*` |
| Figure 3 | chunk `fe_electricity_and_hydrogen_share` | `output/*/Figure 3. Share of electricity and hydrogen in final energy consumption.*` |
| Figure 4 | chunks `Carbon-containing final energy`, `Energy imports`, `Carbon-containing final energy and energy imports` | `output/*/Figure 4. Carbon-based final energy and energy imports.*` |
| Figure 5 | chunks `Emissions per sector`, `Carbon capture scale-up` and the unnamed chunk after them | `output/*/Figure 5. Sectoral emissions and carbon capture and storage.*` |
| Figure 6 | chunks `Carbon Price`, `Mitigation costs`, `figure 6` | `output/*/Figure 6. Economic indicators.*` |
| SI Figure 1 | chunk `secondary_energy_electricity_stacked` | `output/*/SI Figure 1. Electricity generation.*` |
| SI Figure 2 | chunk `capacity_electricity_stacked` | `output/*/SI Figure 2. Capacity mix of power plants.*` |
| SI Figure 3 | chunk `carbon_capture_origin_stacked` | `output/*/SI Figure 3. Carbon Capture by origin of carbon.*` |

Re-running the models themselves is not needed to reproduce the paper. To regenerate the REMIND scenarios, see the next section.

## Models used to produce the scenario data

### REMIND (Potsdam Institute for Climate Impact Research)

The REMIND scenarios in `data/WP1_iiasa_2026_08_03.xlsx` were produced with:

| | |
| --- | --- |
| Model | REMIND – REgional Model of INvestments and Development |
| Version | **v3.6.0** (released 2026-03-27) |
| Source code | <https://github.com/remindmodel/remind/tree/v3.6.0> |
| Archived release | Zenodo, <https://doi.org/10.5281/zenodo.19258991> (all versions: <https://doi.org/10.5281/zenodo.3730918>) |
| License | GNU AGPL-3.0-or-later, with the REMIND License Exception v1.0 |
| Documentation | <https://rse.pik-potsdam.de/doc/remind/3.6.0/>; model description paper (REMIND 2.1): Baumstark et al. (2021), *Geosci. Model Dev.* 14, 6571–6603, <https://doi.org/10.5194/gmd-14-6571-2021> |
| Scenario configuration | [`config/scenario_config_21_EU11_ECEMF.csv`](https://github.com/remindmodel/remind/blob/v3.6.0/config/scenario_config_21_EU11_ECEMF.csv) with region mapping [`config/regionmapping_21_EU11.csv`](https://github.com/remindmodel/remind/blob/v3.6.0/config/regionmapping_21_EU11.csv) |
| Contact | remind@pik-potsdam.de |

Scenario correspondence (REMIND run title → ECEMF scenario name):

| REMIND (`scenario_config_21_EU11_ECEMF.csv`) | ECEMF WP1 scenario |
| --- | --- |
| `xx_WP1_Nzero` | `WP1 NetZero` |
| `xx_WP1_NZero-LimBio` | `WP1 NetZero-LimBio` |
| `xx_WP1_NZero-LimCCS` | `WP1 NetZero-LimCCS` |
| `xx_WP1_NZero-LimNuclear` | `WP1 NetZero-LimNuc` |
| `xx_DIAG-NPI` | `WP1 NPI` |

Re-running REMIND requires GAMS (≥ 39.1) with a CONOPT solver licence (commercial), and R ≥ 4.0. The REMIND developers recommend at least 16 GB of memory and a Core i7-class CPU or better. It also requires the REMIND input data, which are generated with the open-source `madrat`/`mrremind` pipeline. Get in contact with the REMIND team to request for compile model data (remind@pik-potsdam.de). Researchers with access to all data sources can generate the input data themselves. See the [installation tutorial](https://github.com/remindmodel/remind/blob/v3.6.0/tutorials/01_GettingREMIND.md) and [running tutorial](https://github.com/remindmodel/remind/blob/v3.6.0/tutorials/02_RunningREMIND.md).

### Other models

The other nine models (IMAGE, PRIMES, PROMETHEUS, TIAM-ECN, WITCH, LIMES, MEESA, OSeMBE, Euro-Calliope) were run by their respective teams within ECEMF. Their outputs are used here as published on the ECEMF Scenario Explorer. For model descriptions and references, see the paper's Methods section, Supplementary Information, and the ECEMF Scenario Explorer documentation (<https://ecemf.apps.ece.iiasa.ac.at/documentation>).

## Data sources and terms

| File | Source | Terms |
| --- | --- | --- |
| `data/WP1_iiasa_2026_08_03.xlsx` | ECEMF Scenario Explorer (IIASA), snapshot of 2026-08-03 | CC BY 4.0, as published by the model teams |
| `data/historical.mif`, `model` = UNFCCC | UNFCCC GHG inventory submissions | public, attribution |
| `data/historical.mif`, `model` = Ember | Ember electricity data | CC BY 4.0 |
| `data/historical.mif`, `model` = IRENA | IRENA statistics | attribution |
| `data/historical.mif`, `model` = BP | Statistical Review of World Energy | attribution |
| `data/historical.mif`, `model` = IEA | IEA World Energy Balances, aggregated | © OECD/IEA, IEA terms of use (not covered by the CC BY license below) |

## License

- **Code** (`prepare_data.R`, `paperFigures.Rmd`): GNU Affero General Public License v3.0 or later ([`LICENSE`](LICENSE)), the same licence as REMIND.
- **Outputs produced by this code** (`output/`, `paperFigures.html`) and this documentation: Creative Commons Attribution 4.0 International (CC BY 4.0), <https://creativecommons.org/licenses/by/4.0/>.
- **Input data** in `data/` remain under the terms of their original providers (see [Data sources and terms](#data-sources-and-terms)).

## Citation

If you use this repository, please cite the paper and this repository; see [`CITATION.cff`](CITATION.cff). 
When citing the REMIND results, please also cite REMIND v3.6.0 (<https://doi.org/10.5281/zenodo.19258991>).

## Authors

Renato Rodrigues (ORCID [0000-0002-5863-5514](https://orcid.org/0000-0002-5863-5514)), Robert Pietzcker (ORCID [0000-0002-9403-6711](https://orcid.org/0000-0002-9403-6711)), Potsdam Institute for Climate Impact Research (PIK).

Corresponding author: Robert Pietzcker.
