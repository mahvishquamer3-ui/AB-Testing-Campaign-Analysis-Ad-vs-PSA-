# Marketing A/B Test: Ad vs PSA Campaign

Statistical analysis of a marketing A/B test on 588,101 users to determine whether showing an advertisement increases purchase conversion compared with a Public Service Announcement (PSA).

## Business Problem

A marketing team ran a randomized experiment: most users saw the ad (test group), a small control group saw a PSA in the same placement. Two questions:

1. Was the campaign successful?
2. How much of the conversion can be attributed to the ads?

## Dataset

Marketing A/B testing dataset (`AB testing new marketing data.csv`), 588,101 rows, no missing values.

| Column | Description |
|---|---|
| `user id` | Unique user ID |
| `test group` | `ad` (564,577 users) or `psa` (23,524 users) |
| `converted` | Whether the user bought the product |
| `total ads` | Number of ads seen |
| `most ads day` | Day of week with the most ads seen |
| `most ads hour` | Hour of day (0-23) with the most ads seen |

## Approach

1. **Data checks:** missing values, duplicate user IDs, group balance (about 96% ad / 4% PSA).
2. **EDA:** conversion by group; ad volume and ad-group conversion rate by day and hour.
3. **Primary test:** two-sided two-proportion z-test on `converted` (alpha = 0.05).
4. **Effect size:** absolute and relative lift with a 95% confidence interval.
5. **Secondary test:** Shapiro-Wilk showed `total ads` is non-normal, so a Mann-Whitney U test compared ad exposure between groups.

## Key Results

| Metric | Ad | PSA |
|---|---|---|
| Users | 564,577 | 23,524 |
| Conversions | 14,423 | 420 |
| Conversion rate | 2.55% | 1.79% |

- **Z-test:** z = 7.37, p = 1.71e-13, so the conversion difference is statistically significant.
- **Absolute lift:** about +0.77 percentage points (95% CI roughly 0.60 to 0.94 pp).
- **Relative lift:** about +43% over the PSA baseline.
- **Mann-Whitney U on `total ads`:** p = 4.69e-11, so ad and PSA users have different exposure distributions.

## Business Insight

Assuming users were randomly assigned, the ad campaign raised conversion by roughly 0.77 percentage points over the PSA baseline. Ad volume peaks at particular days and hours; the notebook reports conversion by day and hour descriptively, but does not test whether timing causes higher conversion.

## Limitations

- Random assignment is assumed, not verified (no pre-experiment covariates in the data).
- The PSA control group is only about 4% of users, so its conversion rate is estimated less precisely.
- No revenue or cost data, so ROI and profit per conversion cannot be estimated.
- `total ads` is measured after assignment and should not be treated as a pre-treatment covariate.

## Tech Stack

Python, pandas, NumPy, Matplotlib, Seaborn, SciPy, statsmodels.

## How to Run

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels jupyter
jupyter notebook AB_Testing_Campaign_Analysi.ipynb
```

## Repository Structure

```
.
├── AB_Testing_Campaign_Analysis_corrected.ipynb
├── AB testing new marketing data.csv
└── README.md
```
