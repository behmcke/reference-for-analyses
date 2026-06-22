# stats-cookbook

**📖 Live site: https://behmcke.github.io/stats-cookbook/**

A searchable cookbook of applied-statistics code snippets, accumulated while
completing required coursework for my degree in applied statistics. Each task is
its own page — code plus the language meant to accompany the analysis —
browsable by course and by cross-cutting concept tags (Model Fitting,
Diagnostics, Model Selection, Visualization, Resampling & Simulation, and more).

The site is built with [Quarto](https://quarto.org) and published to GitHub
Pages. This repository holds the source; the rendered site lives on the
`gh-pages` branch.

## Contents

### Regression
Stat 6450. Ohio State University. Applied Regression Analysis.
* Simple Linear Regression · Multiple Linear Regression · ANOVA Table
* Check Assumptions · Check Model Fit · Added Variable Plot
* Outliers & Influential Points · Transformations · Indicator Variables
* Interaction Terms · Piecewise Linear Model · Model Selection
* Simple Logistic Regression · Multiple Logistic Regression

### Experiments
Stat 6410. Ohio State University. Design and Analysis of Experiments.
* Assign Random Treatments · Sample Size · Conservative Estimate of Variance
* Test Treatment Effect · Check Assumptions · Transformations
* Choosing Contrasts · Family-wise Confidence Intervals
* Fit 2-Way Complete Model · Interaction Terms · Linear and Quadratic Trends
* Impossible to Interpret

### Time Series
Stat 6550. Ohio State University. Statistical Analysis of Time Series.
* ACF and PACF · Check Assumptions · Detrending · Deseasoning
* Fit ARIMA Model · Forecasting

### Multivariate
Stat 6560. Ohio State University. Applied Multivariate Analysis.
* Descriptive Statistics · Check Normality · Principal Component Analysis

### Statistical Computing
Stat 6730. Ohio State University. Introduction to Statistical Computing.
* Examine Dataframe · Sampling · Simulation Basics · Monte Carlo Simulation
* Bootstrap Estimate · Bootstrap Confidence Interval · Permutation Tests
* Cross-Validation · `purrr`
* Bar Plots · Pie Chart · Boxplots (ggplot) · Scatter (ggplot)
* Facet Grid · `geom_smooth` · Manual Smoothing

## Repository layout

```
index.qmd                  landing page (filterable task listing)
_quarto.yml                site config (navbar, search, listing)
tasks/<course>/*.qmd       one page per analysis task
data/                      vendored datasets + provenance (data/README.md)
```

## Building locally

```sh
quarto preview          # live-reload preview
quarto render           # render the full site to _site/
quarto publish gh-pages # render and publish to GitHub Pages
```

---

*Organized using [Claude Code](https://claude.com/claude-code).*
