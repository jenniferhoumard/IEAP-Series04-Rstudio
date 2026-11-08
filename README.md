# IEAP-Series04-Rstudio

**Linear and non-linear regressions** - IEAP Series 04 group assignment

We investigate two questions with R and Quarto:

1. **How does random noise affect linear regression?** We simulate three data
   sets that share the same linear relationship but contain increasing levels of
   noise, and compare the fitted models.
2. **Which mathematical model best describes heart rate recovery after
   exercise?** Using two real recordings from a chest strap (a 3h10m recording
   and a 7.5-min recording), we compare linear, mono-exponential and
   bi-exponential models of the post-exercise decrease in heart rate.

## Authors

| Student          | Parts of the report                                                         |
| :--------------- | :-------------------------------------------------------------------------- |
| Jennifer Houmard | Random noise and linear regression (01), conclusions (05), repository setup |
| Sarah Durand     | Literature and hypotheses (02), 3h10m recording (03)                        |
| Alexis Lagarde   | 7.5-min recording (04), Git workflow (06)                                   |

## Main results

- **Random noise:** as noise increases, R² decreases and the uncertainty of
  the estimated slope and intercept increases, but over 1 000 repeated
  simulations the estimates stay centred on the true values: noise makes the
  regression less precise, not biased.
- **Heart rate recovery:** the **mono-exponential model** is our preferred
  model for the post-exercise decrease in heart rate. It fits much better than
  the linear model, has interpretable parameters, and gives time constants of
  about 1–3 min (≈ 49–63 s on the 7.5-min recording, ≈ 91–174 s on the 3h10m
  recording). The bi-exponential model does not identify a real second recovery
  phase: its slow component behaves like a linear drift, which can be described
  more simply by adding a linear term to the mono-exponential model.

## Repository structure

```
IEAP-Series04-Rstudio.qmd       master document (header, setup, one include per section)
IEAP-Series04-Rstudio.md        Markdown export of the report (readable on GitHub)
IEAP-Series04-Rstudio_files/    figures used by the Markdown export
one_page_submission.qmd         one-page critical analysis (PDF uploaded on Moodle)
_quarto.yml                     Quarto project options (code runs from the project root)
references.bib                  bibliography (BibTeX)
styles/apa.csl                  APA citation style
sections/
  01_random_noise.qmd             random noise and linear regression
  02_literature_and_hypotheses.qmd  literature review and hypotheses
  03_recovery_3h10m.qmd           analysis of the 3h10m recording
  04_recovery_7m30s.qmd           analysis of the 7.5-min recording
  05_model_comparison.qmd         conclusions and model choice
  06_git_workflow.qmd             our Git workflow strategy
  figures/                        explanatory figures for section 01
data/
  cardio-recovery-3h10m/          3h10m recording (RR, pulse, ECG, acceleration…)
  cardio-recovery-7m30s/          7.5-min recording (same signals)
```

## How to render the report

**Requirements**

- [R](https://cran.r-project.org/) (≥ 4.1) and, ideally,
  [RStudio](https://posit.co/download/rstudio-desktop/)
- [Quarto](https://quarto.org/docs/get-started/) (≥ 1.4)
- The R packages used in the report:

  ```r
  install.packages(c("tidyverse", "lmtest", "patchwork", "jsonlite", "knitr", "rmarkdown"))
  ```

- A LaTeX distribution for the PDF output (the report uses `xelatex`). If you
  do not have one, install TinyTeX once from a terminal:

  ```bash
  quarto install tinytex
  ```

**Render**

Open `IEAP-Series04-Rstudio.Rproj` in RStudio, open
`IEAP-Series04-Rstudio.qmd` and click **Render**.

The `_quarto.yml` file sets `execute-dir: project`, so the code of every
section runs from the project root and the relative paths to `data/` work.
The simulations use fixed random seeds, so the results are reproducible.

## Data

The two heart rate recordings are publicly available recordings from an Aidlab
chest strap ([aidlab.com](https://www.aidlab.com)), distributed under the MIT
licence. Each folder contains:

- `rr.csv`: time between two successive heartbeats (ms), our main signal;
- `pulse.csv`: heart rate computed by the device (bpm, about every 5 s);
- `acceleration.csv`: 3-axis acceleration (g), used to detect movement;
- `ecg.csv`, `respiration.csv`, `body-position.csv`;
- `dataset.json` and `README.txt`: metadata (start time, units, signal quality).

Timestamps are Unix milliseconds (UTC). No activity labels are provided, so
exercise and recovery periods were identified from heart rate and movement.

## Git workflow

- One section file per part of the report, each with a single owner, so the
  branches never conflict.
- One branch per section, named `<number>_<topic>` (e.g. `04_recovery_7m30s`),
  created from an up-to-date `main`.
- Each branch is merged into `main` through a **pull request** reviewed by the
  other members, with "Create a merge commit" so that every commit stays visible.
- Commit messages describe one step each (e.g.
  `Check RR artefacts and recording gaps`).

See section 06 of the report, _Team workflow using Git_, for details and for
what we learned.

## License

This project is licensed under the GNU General Public License v3.0 - see
[`LICENSE`](LICENSE). The heart rate data are distributed by Aidlab under the
MIT licence.
