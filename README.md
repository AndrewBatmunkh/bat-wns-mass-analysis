# Big Brown Bat Mass Changes During White-Nose Syndrome Invasion

Analyzing how White-Nose Syndrome (WNS) affects body mass in Big Brown Bats (*Eptesicus fuscus*) using Generalized Linear Mixed Models with a Gamma distribution — a methodological improvement over the original published study's linear mixed model approach.

**EEB313: Quantitative Methods in R for Biology** — University of Toronto  
**Authors:** Yunjung Jo, Victoria Cordova-Morote, Andrew Batmunkh  
**Instructors:** Mete K. Yuksel & Zoë Humphries

## Overview

White-Nose Syndrome, caused by the fungus *Pseudogymnoascus destructans*, has devastated North American bat populations since 2006. Using 30 years of capture data (30,497 records across 3,797 sites), we modeled how sex, pregnancy status, and disease stage predict bat mass — and demonstrated that a Gamma-distributed GLMM better captures the biological reality of body mass data than the normal-distribution LMM used in the original study.

## Key Findings

- Males weighed approximately **87.3%** of female mass (p < 2e-16)
- Pregnant bats had **18.8% higher** mass than non-pregnant females (p < 2e-16)
- Slight post-invasion decline in log-mass observed across groups
- Pregnant bats were marginally more affected pre-invasion (−0.0264 units, p = 0.016)
- No significant sex × disease interaction — WNS affects males and females similarly
- **Site-specific** random effects (AIC = 132,567) outperformed year (138,845) and state (137,431) models

## Methodology

### 1. Data Wrangling
- Simplified reproductive status into pregnant / non-pregnant
- Collapsed disease time steps into pre-invasion / post-invasion
- Grouped bats by sex, age, and reproductive status combinations

### 2. Distribution Selection
- Explored Normal, Gamma, and other distributions via maximum likelihood estimation
- Gamma distribution best captured the strictly positive, right-skewed nature of body mass data

### 3. Model Selection
- Compared three random effect structures using AIC:

| Model | Random Effect | AIC |
|-------|--------------|-----|
| Model 1 | Year | 138,844.5 |
| Model 2 | Site | **132,567.2** |
| Model 3 | State | 137,431.1 |

- Site-specific model selected as best fit

### 4. Final Model
- **GLMM** with Gamma family and log link function
- Fixed effects: sex, pregnancy status, disease group, and interactions
- Random intercept: site_mask (3,797 unique capture sites)
- Fitted using `glmer()` from the `lme4` package in R

## Data

- **Source:** Simonis et al. (2022) — [Dryad Repository](https://doi.org/10.5061/DRYAD.NGF1VHHVV)
- **Records:** 30,497 individual bat captures
- **Time span:** 1990–2020
- **Geography:** Eastern United States

| Variable | Description |
|----------|-------------|
| `mass` | Body mass in grams (response variable) |
| `sex` | Male / Female |
| `age` | Adult / Juvenile |
| `repstat` | Reproductive status |
| `disease_time_step` | Pre-invasion, invasion, epidemic, establishment |
| `site_mask` | Anonymized capture site ID |
| `year` | Year of capture |
| `state` | U.S. state of capture |
| `county_centroid_lat/lon` | Capture site coordinates |

## Project Structure

```
bat-wns-mass-analysis/
├── data/
│   └── bat_data.csv
├── analysis.Rmd                 # Full analysis in R Markdown
├── analysis.pdf                 # Knitted output
├── paper.pdf                    # Final written report
└── README.md
```

## Setup & Usage

```r
# Required packages
install.packages(c("tidyverse", "lme4", "ggplot2", "knitr"))

# Open analysis.Rmd in RStudio and click "Knit"
```

## References

- Simonis, M. C. et al. (2023). Long-term Exposure to an Invasive Fungal Pathogen Decreases *Eptesicus fuscus* Body Mass with Increasing Latitude. *Ecosphere*, 14(2), e4426.
- Davy, C. M. et al. (2017). Conservation Implications of Physiological Carry-Over Effects in Bats Recovering from White-Nose Syndrome. *Conservation Biology*, 31(3), 615–624.
- Lo, S., & Andrews, S. (2015). To transform or not to transform: Using generalized linear mixed models to analyse reaction time data. *Frontiers in Psychology*, 6, 1171.

## Tech Stack

R · lme4 · tidyverse · ggplot2 · R Markdown
