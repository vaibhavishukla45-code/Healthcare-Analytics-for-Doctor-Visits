# Healthcare Analytics for Doctor Visits

A cross-sectional survey dataset capturing individual-level health, demographic,
and insurance information used to study the determinants of doctor visit frequency.
It is widely used in health econometrics and count-data regression research.

---

## File

| Property | Value |
|----------|-------|
| Filename | `1776250375-P2-Healthcare Analytics for Doctor Visits.csv` |
| Rows | 5,190 |
| Columns | 13 (1 index + 12 features) |
| Missing Values | None |
| Target Variable | `visits` |

---

## Column Descriptions

| Column | Type | Range / Values | Description |
|--------|------|----------------|-------------|
| `visits` | Integer | 0 – 9 | **Target.** Number of doctor visits in the past two weeks |
| `gender` | Categorical | `male`, `female` | Sex of the respondent |
| `age` | Float | 0.19 – 0.72 | Age, normalised (divide by 100 to get approximate decade; e.g. 0.19 ≈ 19 years) |
| `income` | Float | 0.00 – 1.50 | Annual household income, normalised |
| `illness` | Integer | 0 – 5 | Number of illnesses in the past two weeks |
| `reduced` | Integer | 0 – 14 | Number of days activity was reduced due to illness in the past two weeks |
| `health` | Integer | 0 – 12 | Self-assessed health status score (higher = worse health) |
| `private` | Binary | `yes` / `no` | Whether the individual holds private health insurance |
| `freepoor` | Binary | `yes` / `no` | Whether the individual receives free government healthcare (low income) |
| `freerepat` | Binary | `yes` / `no` | Whether the individual receives free healthcare as a repatriate / veteran |
| `nchronic` | Binary | `yes` / `no` | Presence of a non-limiting chronic condition |
| `lchronic` | Binary | `yes` / `no` | Presence of a limiting (activity-restricting) chronic condition |

---

## Target Variable — `visits`

Number of times the respondent visited a doctor in the **past two weeks**.

| Visits | Count | % of Dataset |
|--------|------:|-------------:|
| 0 | 4,141 | 79.8% |
| 1 | 782 | 15.1% |
| 2 | 174 | 3.4% |
| 3 | 30 | 0.6% |
| 4 | 24 | 0.5% |
| 5+ | 39 | 0.8% |

> The heavy concentration at zero makes this a **zero-inflated count** variable,
> well-suited to Poisson regression, Negative Binomial regression, or
> Zero-Inflated models.

---

## Key Statistics

### Continuous Features

| Feature | Min | Mean | Median | Max | Std Dev |
|---------|----:|-----:|-------:|----:|--------:|
| `visits` | 0 | 0.30 | 0 | 9 | 0.80 |
| `age` | 0.19 | 0.41 | 0.32 | 0.72 | 0.20 |
| `income` | 0.00 | 0.58 | 0.55 | 1.50 | 0.37 |
| `illness` | 0 | 1.43 | 1 | 5 | 1.38 |
| `reduced` | 0 | 0.86 | 0 | 14 | 2.89 |
| `health` | 0 | 1.22 | 0 | 12 | 2.12 |

### Categorical Features

| Feature | Yes | No |
|---------|----:|---:|
| `gender` (female) | 2,702 (52.1%) | — |
| `private` | 2,298 (44.3%) | 2,892 (55.7%) |
| `freepoor` | 222 (4.3%) | 4,968 (95.7%) |
| `freerepat` | 1,091 (21.0%) | 4,099 (79.0%) |
| `nchronic` | 2,092 (40.3%) | 3,098 (59.7%) |
| `lchronic` | 605 (11.7%) | 4,585 (88.3%) |

---

## Suggested Use Cases

| Task | Approach |
|------|----------|
| **Doctor visit frequency prediction** | Poisson / Negative Binomial regression |
| **Zero-inflated count modelling** | ZIP / ZINB models |
| **Healthcare demand analysis** | Descriptive statistics, EDA |
| **Insurance impact study** | Comparing `private` vs `freepoor` vs `freerepat` groups |
| **Chronic condition effect** | Group comparison (`nchronic`, `lchronic`) |
| **Feature importance** | Random Forest, Gradient Boosting, or Logistic (binary visit) |

---

## Notes on Encoding

- **`age`** and **`income`** are **normalised** continuous values, not raw years/dollars.
  Multiply `age` by 100 to recover approximate values (e.g. 0.45 ≈ 45 years old).
- **Binary columns** (`private`, `freepoor`, `freerepat`, `nchronic`, `lchronic`) are
  stored as strings (`"yes"` / `"no"`); encode to 0/1 before modelling.
- The **row index** column (first column, unnamed) is a sequential integer ID and
  should be dropped before analysis.

---

## Quick Load (Python)

```python
import pandas as pd

df = pd.read_csv("1776250375-P2-Healthcare Analytics for Doctor Visits.csv",
                 index_col=0)

# Encode binary columns
binary_cols = ["private", "freepoor", "freerepat", "nchronic", "lchronic"]
df[binary_cols] = (df[binary_cols] == "yes").astype(int)

# Encode gender
df["gender"] = (df["gender"] == "female").astype(int)

print(df.shape)   # (5190, 12)
print(df.head())
```

---

## Dataset Summary

- **5,190** individuals surveyed
- **Zero missing values** across all columns
- **12 features** covering demographics, health status, insurance coverage, and chronic conditions
- **Target (`visits`)** is a non-negative integer count — 79.8% of respondents had 0 visits
