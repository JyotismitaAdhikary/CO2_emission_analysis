# Growth vs Emissions: Which Countries Have Decoupled?

A data analysis project that cleans global CO₂ and GDP data **with SQL**, then uses **unsupervised learning** to find patterns in how countries' economies and emissions have moved together — or apart — from 1995 to 2022.

> **Headline question:** *Which countries have grown their economy while cutting CO₂ emissions and what distinguishes them from those that haven't?*

The whole analysis lives in a single, self-contained Jupyter notebook: [`co2_decoupling_analysis.ipynb`](co2_decoupling_analysis.ipynb).

---

## Why this is interesting

"Decoupling" is a central idea in climate-and-growth debates. We usually assume a bigger economy means more emissions but some countries have broken that link. This project finds them, classifies every country by *how* its emissions and economy moved together, and groups countries into natural types with clustering.

---

## What's inside

The notebook walks through the analysis step by step, with the reasoning explained alongside the code:

1. **Load** the raw dataset into SQLite as text (*stage raw, transform later*).
2. **Clean it with SQL** drop aggregate rows (e.g. "World", "Asia"), cast types, turn blank strings into real NULLs.
3. **Build decoupling features** one row per country (CO₂ change, GDP change, coal share, per-capita level) using SQL window functions, plus a four-way decoupling classification.
4. **Cluster** countries with k-means to surface natural groups (with winsorizing + standardizing).
5. **Visualise** the decoupling pattern in a single GDP-vs-CO₂ chart.
6. **Bonus** compare territorial vs consumption-based emissions to reveal which "green" countries have simply *offshored* their emissions.

The SQL cleaning is the centerpiece. It's done in SQL on purpose, since that's the skill the project is meant to demonstrate.

---

## Dataset

Our World in Data - *Complete CO₂ and Greenhouse Gas Emissions* (one CSV, ~50,000 rows, 79 columns, country-by-year).

- Source: [github.com/owid/co2-data](https://github.com/owid/co2-data)
- License: Creative Commons BY (free to use and publish with attribution)
- Suggested attribution: *Global Carbon Budget (2025) with major processing by Our World in Data.*

**You don't need to download anything** the notebook fetches the CSV automatically on first run.

---

## How to run

```bash
# 1. Install dependencies
pip install pandas numpy scikit-learn matplotlib notebook

# 2. Open the notebook
jupyter notebook co2_decoupling_analysis.ipynb
```

Then choose **Kernel → Restart & Run All**. The notebook downloads the dataset, runs the SQL cleaning in an in-memory SQLite database, and produces every table and chart inline.

---

## Key findings

Of 164 countries with 25+ years of data (1995–2022):

| Decoupling class | Countries | Meaning |
|------------------|-----------|---------|
| Relative decoupling | 86 | GDP and CO₂ both grew, but CO₂ grew slower |
| Coupled growth | 38 | CO₂ grew as fast as (or faster than) GDP |
| Absolute decoupling | 38 | GDP grew while CO₂ **fell** — the goal |
| GDP decline | 2 | Economy shrank (can't credit policy for the CO₂ drop) |

The clearest absolute decouplers were **Denmark, the UK, Sweden and Finland** — wealthy economies that shifted away from coal. The k-means clustering split countries into four recognisable types: mature low-coal economies, fast-growing developing emitters, coal-heavy industrial economies, and high-per-capita petrostates.

> **Honest caveat:** some big "decouplers" (Ukraine, Moldova, Romania) fell largely because of post-Soviet industrial collapse, **not** clean-energy policy. The notebook flags this rather than overclaiming.

---

## Limitations & next steps

- **Endpoint sensitivity** — comparing only the first and last year ignores the path between; a per-country regression slope would be more robust.
- **Outliers** — a few tiny-base countries have huge % changes; the clustering winsorizes them and the chart zooms past two off-scale points (both documented).
- **Choosing k** — k = 4 was picked for interpretability; an elbow or silhouette analysis would justify it more rigorously.
- **Causation** — decoupling ≠ good policy; pairing with energy-mix data would help separate genuine transitions from industrial decline.

---

*Built with SQLite, pandas, and scikit-learn.*
