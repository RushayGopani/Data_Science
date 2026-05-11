# DS-4: APMS 2014 Data Documentation & NHS Compliance
**AI Mental Health UK Project | Data Science Intern Sprint | 2026-05-04**

---

## Dataset Dictionary
| Dataset                         | Field          | Data_Type   | Source_Table           | Description                                                                          | Notes                                                                                      |
|:--------------------------------|:---------------|:------------|:-----------------------|:-------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------|
| DS1_baseline_stats_model.csv    | Region         | string      | APMS 2014 Table 14.2   | Government Office Region (England). Mapped from former Strategic Health Authorities. | APMS used SHAs; data mapped to GORs for compatibility with NHS Digital Bulletin 2024-25.   |
| DS1_baseline_stats_model.csv    | Sample_N       | integer     | APMS 2014 Table 14.2   | Unweighted survey respondent count for the region.                                   | APMS 2014 England total n=13,319 screened; n=7,546 full interview.                         |
| DS1_baseline_stats_model.csv    | CMD_Prev_Pct   | float       | APMS 2014 Table 2.1    | % of adults with CIS-R score ≥12 (CMD clinical threshold).                           | National figure: 15.7% all adults, 12.2% men, 19.1% women (APMS 2014).                     |
| DS1_baseline_stats_model.csv    | CI_Lower_95    | float       | Derived                | 95% Wilson score CI lower bound on CMD_Prev_Pct.                                     | Wilson interval preferred over normal approx for small samples.                            |
| DS1_baseline_stats_model.csv    | CI_Upper_95    | float       | Derived                | 95% Wilson score CI upper bound on CMD_Prev_Pct.                                     | Wilson interval preferred over normal approx for small samples.                            |
| DS1_baseline_stats_model.csv    | Gender_Gap_Pct | float       | APMS 2014 Table 2.1    | Difference in CMD prevalence: Women% − Men% for that region.                         | National gender gap: 6.9pp. Individual regions vary 6–8pp.                                 |
| DS2_hypothesis_test_results.csv | Test           | string      | Derived                | Unique test identifier and name.                                                     |                                                                                            |
| DS2_hypothesis_test_results.csv | Chi2_stat      | float       | Derived                | Chi-square or t test statistic value.                                                | Null for t-test rows; t_stat column used instead.                                          |
| DS2_hypothesis_test_results.csv | p_value        | float       | Derived                | Two-tailed p-value for the hypothesis test.                                          | Significance threshold α=0.05 throughout. All 5 tests: p<0.05.                             |
| DS3_distribution_analysis.csv   | Age_Band       | categorical | APMS 2014 Table 2.1    | Age group of respondents per NHS APMS standard bands.                                | Age bands follow APMS 2014 / NHS standard groupings.                                       |
| DS3_distribution_analysis.csv   | Gender         | categorical | APMS 2014 Table 2.1    | Gender category as recorded in APMS 2014.                                            | APMS 2014 collected binary gender. Non-binary not separately reported in published tables. |
| DS3_distribution_analysis.csv   | Pct_0-5        | float       | APMS 2014 Table 2.1    | % of respondents with CIS-R score in band 0–5 (minimal/no symptoms).                 | CIS-R 0-5 = no clinically significant symptoms.                                            |
| DS3_distribution_analysis.csv   | Pct_6-11       | float       | APMS 2014 Table 2.1    | % of respondents with CIS-R score in band 6–11 (subthreshold symptoms).              | CIS-R 6-11 = subthreshold; below clinical CMD threshold.                                   |
| DS3_distribution_analysis.csv   | Pct_12-17      | float       | APMS 2014 Table 2.1    | % of respondents with CIS-R score in band 12–17 (moderate CMD).                      | CIS-R ≥12 = clinical CMD threshold. Likely to benefit from intervention.                   |
| DS3_distribution_analysis.csv   | Pct_18+        | float       | APMS 2014 Table 2.1    | % with CIS-R ≥18 (severe CMD symptoms).                                              | CIS-R ≥18 = severe symptoms; almost certainly benefit from treatment.                      |
| DS3_distribution_analysis.csv   | Est_Mean_CIS-R | float       | Derived from Table 2.1 | Estimated mean CIS-R score using band midpoint weighting.                            | Approximation using midpoints [2.5, 8.5, 14.5, 22.0]. Not a direct measurement.            |
| DS3_normality_tests.csv         | Shapiro-Wilk_W | float       | Derived                | Shapiro-Wilk W statistic for normality test.                                         | W close to 1.0 = more normal. All segments: W<0.85 — not normally distributed.             |

---

## Source Table Provenance
|   Chapter |   Table | Title                                                | Used_In                                 | Key_Values                                                                                                | N_Respondents                   |
|----------:|--------:|:-----------------------------------------------------|:----------------------------------------|:----------------------------------------------------------------------------------------------------------|:--------------------------------|
|         2 |     2.1 | Severity of symptoms of CMD, by age and sex          | DS-1, DS-2, DS-3                        | CIS-R band %: Men/Women/All × 7 age groups. National: Men 12.2%, Women 19.1%.                             | 7,546 (Men 3,058 + Women 4,488) |
|         2 |     2.3 | CMD in past week, by age and sex                     | DS-2                                    | Specific CMD types (GAD, depression, phobias, OCD, panic, NOS) × age/sex.                                 | 7,546                           |
|         2 |     2.4 | CMD in past week across survey years 1993–2014       | DS-4 (trend context)                    | Historical trend. CMD rising in women 16-24: 20.5% (1993) → 28.2% (2014).                                 | Varies by wave                  |
|         2 |     2.7 | CMD in past week, by ethnic group and sex            | DS-4 (dictionary context)               | White British, White Other, Black/Black British, Asian, Mixed.                                            | Subset of 7,546                 |
|         4 |     4.1 | PTSD screen positive by age and sex                  | DS-2 (H04)                              | PTSD screen+: Men 3.7%, Women 5.1%. Highest in women 16-24 (12.6%).                                       | 7,066                           |
|        12 |    12.1 | Suicidal thoughts, attempts and self-harm by age/sex | DS-2 (H03)                              | Lifetime suicidal thoughts: Men 18.7%, Women 22.4%. Self-harm women 16-24: 25.7%.                         | ~7,545                          |
|        14 |    14.1 | Regional stratifier and PSUs selected                | DS-1, DS-4                              | 10 Strategic Health Authority regions. Total 698 PSUs. England total n=22,893,862 delivery points.        | N/A (design table)              |
|        14 |    14.2 | APMS 2014 final response model                       | DS-1 (sample sizes), DS-4 (methodology) | Regional sample n: NE=677, NW=1785, Yorks=1321, EM=1200, WM=1419, EoE=1367, London=1530, SE=756, SW=1088. | 13,319 screened                 |

---

## NHS Data Usage Compliance Checklist
Total checks: 16 | PASS: 13 | ACTION REQUIRED: 3

| ID   | Requirement                                                         | Status          | Evidence                                                                                                         |
|:-----|:--------------------------------------------------------------------|:----------------|:-----------------------------------------------------------------------------------------------------------------|
| A1   | All datasets are publicly available (no patient-identifiable data)  | PASS            | APMS 2014 published tables freely available at digital.nhs.uk. No microdata accessed.                            |
| A2   | No NHS DARS restricted data used                                    | PASS            | Only published aggregate tables used. MHSDS patient-level data not accessed.                                     |
| A3   | No GDPR Article 9 special category data at individual level         | PASS            | All analysis on aggregate published statistics. No individual health records processed.                          |
| A4   | Open Government Licence v3.0 compliance                             | PASS            | APMS 2014 published under OGL v3.0. Source attributed in all outputs.                                            |
| A5   | Copyright attribution to NHS Digital / NatCen                       | PASS            | "Source: APMS 2014, NHS Digital. Copyright © 2016, Health and Social Care Information Centre." cited throughout. |
| B1   | No patient names, NHS numbers, DOBs, or postcodes in any dataset    | PASS            | Published aggregate tables only. No identifiers present.                                                         |
| B2   | Synthetic/derived data clearly labelled                             | PASS            | All reconstructed samples labelled "estimated from published band proportions". CSVs include Notes column.       |
| B3   | Data not shared externally without approval                         | PASS            | Sprint outputs for internal demo only. No external sharing.                                                      |
| B4   | Data stored in approved project environment only                    | PASS            | All data in approved cloud sandbox. No personal device storage.                                                  |
| C1   | All statistical methods documented with reproducibility information | PASS            | numpy.random.seed(42) throughout. All methods documented in notebooks.                                           |
| C2   | Confidence intervals reported alongside point estimates             | PASS            | Wilson 95% CIs in DS-1 for all regional estimates. Method stated.                                                |
| C3   | Approximations clearly distinguished from direct measurements       | PASS            | Mean CIS-R estimates labelled "Est_Mean" and noted as midpoint approximations.                                   |
| C4   | No outputs presented as real patient-level data                     | PASS            | All documentation states "aggregate published statistics" as source.                                             |
| D1   | APMS full microdata — UK Data Service application required          | ACTION REQUIRED | Submit UKDS Data Access Agreement before accessing SN 7702 microdata.                                            |
| D2   | MHSDS patient-level data — NHS Digital DARS application             | ACTION REQUIRED | DARS application + NHS DSP Toolkit completion required.                                                          |
| D3   | Data Processing Agreement if sharing with NHS clients               | ACTION REQUIRED | Legal review required before client demo with any patient-proximate data.                                        |

---

## Key Methodological Notes

1. **CIS-R scoring:** A CIS-R score ≥12 indicates clinically significant CMD. Scores ≥18 indicate severe symptoms almost certainly requiring treatment (APMS 2014, Chapter 2).
2. **Wilson confidence intervals:** Used in preference to normal approximation for proportions, especially for smaller regional samples (n<1000).
3. **Band midpoint estimation:** Mean CIS-R scores estimated using midpoints [2.5, 8.5, 14.5, 22.0]. These are approximations — actual microdata would yield exact scores.
4. **Gender recording:** APMS 2014 recorded binary gender (men/women). Non-binary respondents are not separately reported in the published tables used.
5. **Survey weights:** Published percentages in APMS 2014 tables use survey weights. Unweighted bases are shown separately.

*Source: APMS 2014, NHS Digital / NatCen Social Research. OGL v3.0.*
