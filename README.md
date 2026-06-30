
# Jharkhand Financial Inclusion Intelligence Platform

A data-driven decision-support platform analysing financial literacy and financial inclusion across Jharkhand, India — built for citizens, government officials, and NGOs.

**Live demo:** https://kavikar29003.github.io/jh-fin-inclusion-platform/


---

## What this is

Jharkhand has reached near-universal formal banking access (79.6% of women have bank accounts), but access has not translated into financial awareness or protection. This project tests a specific hypothesis — that financial inclusion gaps are driven by low education/literacy — against real government data, and finds that hypothesis is **wrong**.

Across all 24 Jharkhand districts, an OLS regression (literacy → insurance coverage) returns **R² = 0.006**: literacy explains essentially none of the variation in insurance coverage. Ranchi, the state capital with the highest women's literacy (76%), ranks 21st of 24 districts for health insurance coverage. The real driver is policy delivery and awareness campaign reach, not education level — a finding with direct implications for how government and NGO outreach should be targeted.

## What it does

The platform has six modules:

1. **Citizen Financial Readiness Assessment** — an 8-question self-assessment producing a personalised readiness score, strengths/gaps breakdown, and top recommendation, benchmarked against 48 real pilot survey respondents from Jharkhand.
2. **Personalised Scheme Recommender** — matches a citizen's age, income, occupation, and existing coverage to eligible government schemes (PMSBY, PMJJBY, PMJDY, APY, PPF, PMKMY, NPS) using real eligibility rules.
3. **District Financial Inclusion Risk Index** — a composite risk score across all 24 districts (Insurance 40%, Literacy 20%, Internet access 20%, Bank access 20%), with click-through intervention recommendations per district.
4. **Predictive Analytics** — an OLS linear regression (fitted in Python with scikit-learn) testing whether literacy predicts insurance coverage at the district level, with full predicted-vs-actual gap analysis.
5. **AI Insight Cards** — six key findings auto-extracted from the district-level data.
6. **Campaign Recommendation Engine** — select a district and target group; the engine recommends the highest-impact intervention type based on that district's specific gap profile (awareness gap vs. delivery gap vs. digital-access gap).

The full interface is available in **English and Hindi**, since financial literacy tools in India are almost always English-only — excluding the rural population they are meant to serve.

## Data sources

All figures are from public government sources:

- **NFHS-5 (2019–21) and NFHS-4 (2015–16) district fact sheets**, International Institute for Population Sciences (IIPS), Ministry of Health & Family Welfare — district-level insurance coverage, literacy, urban/rural breakdowns.
- **PMJDY state-wise accounts data**, data.gov.in (23 Feb 2024 snapshot) — Pradhan Mantri Jan Dhan Yojana account totals, normalised by state population.
- **Government scheme guidelines**, Department of Financial Services, Ministry of Finance — PMSBY, PMJJBY, PMJDY, APY, PPF, NPS terms and eligibility.
- **Pilot survey, n=48** — an original survey of friends, family, and peers in Jharkhand, used to calibrate the assessment's scoring weights (via correlation with self-rated financial confidence) and to discover financial-literacy personas via K-Means clustering (k=3).

## Methodology notes (for technical reviewers)

- **Regression**: fitted in Python using `sklearn.linear_model.LinearRegression` on all 24 NFHS-5 district data points (X = women's literacy rate, y = health insurance coverage). Result: intercept = 55.43, coefficient = −0.0601, R² = 0.006. This low R² is reported as-is and is the substantive finding — it was not adjusted, hidden, or reframed to look stronger.
- **Clustering**: K-Means (k=3) run on 5 category scores (savings, insurance, investment, budgeting, digital banking) from 48 real pilot respondents. Cluster centroids are hard-coded in the dashboard from that one-time analysis; the clustering itself is real, but the pilot sample (n=48) is small and results should be read as indicative, not statistically generalisable.
- **Risk index weights** (Insurance 40%, Literacy 20%, Internet 20%, Bank access 20%) are researcher-assigned based on domain reasoning — insurance coverage is weighted highest because it is the primary outcome variable the index is built to explain. This is standard practice for composite indices (e.g. similar to how HDI assigns component weights) but is not itself derived from a statistical procedure.
- **Campaign engine expected-impact figures** (e.g. "8–12% coverage increase") are illustrative estimates based on observed gains in comparable districts post-NFHS-4, not output from a formal projection model.

## Tech stack

Single self-contained HTML file — no build step, no backend, no dependencies beyond Chart.js (loaded from CDN). Runs entirely client-side; all data is embedded in the file.

- HTML / CSS / vanilla JavaScript
- [Chart.js 4.4.1](https://www.chartjs.org/) for all visualisations
- No frameworks, no build tools — open `index.html` directly in any browser

## Running locally

```bash
git clone https://github.com/YOUR-USERNAME/jh-fin-inclusion-platform.git
cd jh-fin-inclusion-platform
open index.html   # macOS
# or just double-click index.html
```

No installation required. An internet connection is only needed to load the Chart.js library from CDN; all data and logic is embedded in the file.

## Project background

Built as part of an MSc Data Science scholarship application, originating from a personal observation: despite working adjacent to India's insurance industry, the author did not feel confident in their own financial literacy. That gap motivated testing whether financial exclusion in Jharkhand is primarily an education problem (it is not) versus an awareness/delivery problem (the data strongly suggests it is).

## License

This project uses publicly available government data (NFHS-5/NFHS-4, PMJDY, data.gov.in) which is open for research and non-commercial use. The codebase is shared under the MIT License — see [LICENSE](LICENSE).

## Author

Kajal Kumari — B.C.A., Stella Maris College | Programmer Tata Consultancy Services .

