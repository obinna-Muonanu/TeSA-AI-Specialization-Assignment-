# Creditly Applicant Pool Analysis

![alt text](image-1.png)

## Overview

LoanPay launched **Creditly**, its digital loan application platform, backed by an aggressive marketing campaign designed to drive loan applications. A surge in applications means little and can be dangerous if the campaign disproportionately pulled in high-risk applicants rather than the financially sound customers LoanPay intended to reach.

This notebook profiles the resulting applicant pool (**981 records**) to answer one question for the Risk and Marketing teams before any further lending decisions or campaign spend: **"Who did we actually attract?"**

## Data

The raw dataset (`raw_data.csv`) contains one row per loan application, with the following fields:

| Column | Description |
|---|---|
| `Loan_ID` | Unique identifier per application |
| `Gender` | Applicant gender |
| `Married` | Marital status (Yes/No) |
| `Dependents` | Number of dependents |
| `Education` | Graduate / Not Graduate |
| `Self_Employed` | Yes/No |
| `ApplicantIncome` | Applicant's annual income (₦'000) |
| `CoapplicantIncome` | Co-applicant's annual income (₦'000) |
| `LoanAmount` | Amount requested (₦'000) |
| `Loan_Amount_Term` | Loan duration, in months |
| `Credit_History` | 1 = good credit / no prior defaults, 0 = bad credit / prior default |
| `Property_Area` | Urban / Semiurban / Rural |

## Data Cleaning

Before analysis, the following issues were identified and fixed:

- **Inconsistent gender labels** (`m`, `Male`, `f`, `Female`) standardized to `Male` / `Female`
- **`ApplicantIncome`** cast to `float` to match `CoapplicantIncome`
- **`Credit_History`** cast to a numeric/int type (it's a binary categorical flag)
- **`Property_Area`** trailing whitespace and spelling variants (e.g. `Uban`) normalized to `Urban` / `Semiurban` / `Rural`
- **`Loan_Amount_Term`** cast to int (a duration in months can't be fractional)
- Checked for and confirmed no duplicate rows

### Missing values

Several columns had missing values (`Credit_History` 8.1%, `Self_Employed` 5.6%, `LoanAmount` 2.8%, `Dependents` 2.5%, `Gender` 2.4%, `Loan_Amount_Term` 2.0%, `Married` 0.3%). A `missingno` matrix showed the missingness in `Married` and `Dependents` is correlated.

Because this is an exploratory analysis meant to describe the observed applicant pool — not a predictive model — missing values (particularly `Credit_History`) were **not imputed**, since doing so would introduce assumptions about information that was never actually observed. Instead:
- `Gender` missing values were filled with `"Unknown"` so they could still be counted and visualized
- All other analyses simply **excluded** rows missing the specific field(s) they needed, rather than dropping them from the whole dataset

## Analysis Structure

The notebook (and the accompanying slide deck) walk through four sections:

1. **Overall Credit Quality of the Pool** — What share of applicants have good vs. poor credit histories, and how does that compare to the campaign's benchmark?
2. **Demographics** — Who did the campaign attract in terms of gender, education, marital status, and employment type? Does any segment skew toward poor credit?
3. **Geographic Composition** — Where are applicants located, and does credit quality vary by region?
4. **Income & Loan Sizing Behavior** — How do income and requested loan amount relate to credit history?

Each section pairs a chart with a short written insight in the notebook.

## Key Findings

- **Credit quality beat plan:** 83.6% of applicants have good credit history vs. bad credit's 16.4% — above the campaign's 80% benchmark.
- **Demographics:** the pool is predominantly male (775), graduate (763), married (631), and salaried (807 non-self-employed).
- **Segment risk:** Not-Graduate (21% bad credit) and gender-unidentified applicants (22%, small n=24) skew above the 16.4% pool average; marital status and self-employment barely move the needle.
- **Geography:** applicants are fairly evenly split across Semiurban (35.6%), Urban (34.9%), and Rural (29.6%) areas, with consistent credit quality (82–84% good) across all three.
- **Income & loan size:** average income and average loan amount are nearly identical between good- and bad-credit applicants, and loan term is standardized at 360 months for both — income and loan size alone do not discriminate credit risk in this pool.

## Files

| File | Description |
|---|---|
| `TeSA Assignment (AI Specialization).ipynb` | Full analysis notebook — data cleaning, EDA, and charts |
| `Creditly_Applicant_Pool_Analysis.pptx` | Slide deck summarizing findings and business decisions for Risk & Marketing leadership |
| `cleaned_data.csv` | csv file after cleaning. Note: Still contain missing values

## Tools Used

`pandas`, `numpy`, `matplotlib`, `seaborn`, `missingno`
