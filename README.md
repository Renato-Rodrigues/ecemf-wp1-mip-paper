# Milestones to Net Zero: Intermodel Insights on the EU’s 2040 Climate Targets

Data, code, and results companion repository for the paper:

> **Milestones to Net Zero: Intermodel insights on the EU’s 2040 climate targets**  
> *Pietzcker et al.*

## Goal & Purpose

The goal of this repository is to store all necessary information, data, and code to make the paper results fully reproducible and to serve as an official companion repository to the manuscript and its Supplementary Information.

## Contents

```
data/WP1_iiasa_2026_08_03.xlsx   scenario results from the IIASA ECEMF Scenario Explorer, reduced
                                 to the 96 variables the code reads, region EU27 & UK (*), the five
                                 scenarios used, and the 5-year time steps from 2005 on
data/historical.mif              historical data (UNFCCC, Ember, IEA, IRENA, BP; EUR, 1990-2030)
prepare_data.R                   reads both files and derives the variables the figures need
paperFigures.Rmd                 results table, then the nine published figures
output/png, output/svg           the figures, written on every render
```

## Running

```r
rmarkdown::render("paperFigures.Rmd")
```

Requires R with `tidyr`, `dplyr`, `quitte`, `ggplot2`, `grid`, `gridExtra`,
`ggh4x`, `ggrepel`, `mip` and `svglite`.

Rendering writes `paperFigures.html` (results table plus the figures) and the
individual png and svg files under `output/`.

## Figures

| File | Used as |
| --- | --- |
| `Figure 1. Emissions panel` | Manuscript Figure 1 |
| `Figure 2. Electricity generation from wind and solar` | Manuscript Figure 2 |
| `Figure 3. Share of electricity and hydrogen in final energy consumption` | Manuscript Figure 3 |
| `Figure 4. Carbon-based final energy and energy imports` | Manuscript Figure 4 |
| `Figure 5. Sectoral emissions and carbon capture and storage` | Manuscript Figure 5 |
| `Figure 6. Economic indicators` | Manuscript Figure 6 |
| `SI Figure 1. Electricity generation` | SI Figure 1 |
| `SI Figure 2. Capacity mix of power plants` | SI Figure 2 |
| `SI Figure 3. Carbon Capture by origin of carbon` | SI Figure 3 |

## License

The underlying code/dataset supporting the findings of this study has been deposited in a git repository (https://github.com/Renato-Rodrigues/ecemf-wp1-mip-paper) and on Zenodo (URL) under a CC BY-NC 4.0 license. 

The code/data is freely available to readers for non-commercial replication and academic research purposes. For commercial use inquiries and licensing terms, please contact the corresponding author.

## Authors

Renato Rodrigues, Robert Pietzcker
