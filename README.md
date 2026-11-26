# Global Well-Being Analysis with PCA and K-Means Clustering

**English** · [Bahasa Indonesia](README.id.md)

A multivariate analysis in R that reduces and clusters 145 countries on well-being indicators from the 2025 World Happiness Report (147 initial countries, 2 dropped for missing values). The repository includes an interactive browser slide deck, the full report PDF, and the reproducible R script.

- **Online slides:** https://febrianggi.github.io/global-well-being/
- **Downloads:** [`Slide.pdf`](Slide.pdf) · [`Paper.pdf`](Paper.pdf)

## Key findings

- **Variable screening:** Out of six initial indicators, `Generosity` was removed because its KMO value was **0.36** (below the 0.5 threshold). On the remaining five indicators, Bartlett's test of sphericity is strongly significant (chi-square **271.634**, `p` ≈ `1.5 × 10^-52`), confirming adequate correlation for PCA.
- **Three principal components explain 88.172% of total variance** (PC1 eigenvalue **2.821**):
  - **PC1:** General well-being (`Log GDP per capita`, `Social support`, `Healthy life expectancy`).
  - **PC2:** Dominated by `Perceptions of corruption`.
  - **PC3:** Dominated by `Freedom to make life choices`.
- **Two global well-being clusters (`k = 2`):** The Elbow method suggests `k = 4`, while the Silhouette method favours `k = 2` (average width **0.386**). Using `k = 2`:
  - **Cluster 1 (67 countries):** Lower average scores across the five well-being indicators.
  - **Cluster 2 (78 countries):** Higher average scores across the five well-being indicators.

## How to read the online slides and paper

Open https://febrianggi.github.io/global-well-being/ (served from [`index.html`](index.html)) to step through all **25 slides** (`slides/01.webp` to `slides/25.webp`) directly in the browser:

- Use the on-screen arrows (`‹` / `›`) or keyboard arrow keys to move slide by slide.
- Click **All slides** to open the thumbnail grid and jump directly to a specific chart (KMO/Bartlett checks, scree plot, biplot, Elbow/Silhouette selection, or cluster map/profiles).
- Click **Full screen** for presentation view, or use the header links to download [`Slide.pdf`](Slide.pdf) and [`Paper.pdf`](Paper.pdf).

**Language note:** The slide deck, full paper, and R script comments/console output are written in Indonesian, while variable names and statistical terms (`well-being`, `clustering`, `eigenvalue`) stay in English.

## Variables and workflow

The analysis starts with six variables from the World Happiness Report (`Log GDP per capita`, `Social support`, `Healthy life expectancy`, `Freedom to make life choices`, `Generosity`, and `Perceptions of corruption`) and runs on the five retained variables after dropping `Generosity`.

[`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) executes five steps end to end:

1. **Data preparation:** Drop rows with missing values (147 to 145 countries) and standardize with z-scores.
2. **Assumption checks:** KMO, Bartlett's test of sphericity, boxplots, and Mahalanobis distance for outliers.
3. **PCA:** Eigenvalue table, scree plot, loading plot, biplot, and component equations.
4. **K-Means on PCA scores:** Compare `k` with Elbow and Silhouette (`set.seed(42)`).
5. **Cluster profiles:** Compute original-scale variable means per cluster and list member countries.

## How to run locally

### 1. View the web slides locally

Serve the repository root over HTTP so the browser loads the 25 WebP slides cleanly:

```bash
python -m http.server 8000
```

Open http://localhost:8000 in your browser. Stop the server with `Ctrl+C`.

### 2. Reproduce the R analysis

Install R and the required packages:

```r
install.packages(c(
  "tidyverse", "factoextra", "FactoMineR",
  "psych", "cluster", "corrplot"
))
```

The raw dataset is not bundled in this repository. Download the 2025 World Happiness Report data from https://data.worldhappiness.report and save it as a CSV using a semicolon (`;`) delimiter and comma (`,`) decimal separator with columns in this order:

| Column | Content |
|---|---|
| 1 | Year |
| 2 | Rank |
| 3 | Country name |
| 4 to 9 | Log GDP, Social support, Healthy life expectancy, Freedom, Generosity, Perceptions of corruption |

Open [`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) in RStudio and run it. A `file.choose()` dialog will prompt for the CSV file. With `seed = 42`, K-Means reproduces the 67/78 country split.

## Repository structure

| Path | Contents |
|---|---|
| [`index.html`](index.html) | Browser slide viewer |
| `slides/` | 25 WebP presentation slides |
| [`Slide.pdf`](Slide.pdf) | Full presentation deck (PDF, Indonesian) |
| [`Paper.pdf`](Paper.pdf) | Written report (PDF, Indonesian) |
| [`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) | End-to-end R script |

## Data source

Helliwell, J., Layard, R., Sachs, J., De Neve, J.-E., Aknin, L., & Wang, S. (2026). *World Happiness Report*. University of Oxford: Wellbeing Research Centre.

The raw data belong to the publisher; check the World Happiness Report terms of use before redistributing the data file.

## Limitations

- Cross-sectional 2025 snapshot only, so year-to-year changes are not analysed.
- Compares PCA + K-Means only; alternative clustering methods such as hierarchical clustering or DBSCAN are not tested.
- An average silhouette width of 0.386 indicates moderate separation, so boundaries between the two clusters are not sharp.
