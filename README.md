# Retail Data Quality and Cleaning Pipeline

This project assesses the quality of a retail orders dataset and builds a reproducible cleaning pipeline to prepare the data for analysis.

The main focus is not only to clean missing or invalid values, but also to investigate inconsistencies across related variables, document uncertain decisions, and preserve data quality flags that help analysts understand when specific records require caution.

> **Academic Project**
>
> This project was developed for educational purposes using a simulated retail business case. The analysis was independently expanded and structured as a Data Analytics portfolio project.

---

## 📌 Project Overview

The project evaluates a retail orders dataset with missing values, encoded sentinel values, quantity inconsistencies, temporal anomalies, and extreme numerical observations.

A cleaning pipeline was developed to correct supported issues, preserve uncertain records through quality flags, and produce a prepared dataset that can be used more carefully in downstream analysis.

---

## 🏢 Business Context

In this simulated business case, a retail dataset must be prepared before it can support reliable analysis. Several data quality issues affect key variables such as order dates, customer age, product category, and quantity.

The main challenge is deciding which records can be corrected with available evidence and which should remain unchanged but clearly identified as analytically uncertain.

---

## 🎯 Analysis Objective

### Main Objective

Assess the quality of the retail dataset and build a reproducible cleaning pipeline that prepares the data for analysis while preserving the traceability of uncertain transformations.

### Analysis Questions

- What missing, invalid, or encoded values affect the dataset?
- Can zero quantities be reconstructed using the relationship between `order_value` and `price`?
- Which inconsistencies can be corrected with available evidence, and which should be preserved?
- How do cleaning decisions affect the reliability of quantity-based analysis?
- What types of retail analysis remain reasonable after data preparation?

---

## 📊 Data Sources

The project uses one simulated retail orders dataset containing customer, product, transaction, payment, location, and order information.

The raw dataset contains **5,008 records**. After removing records without usable order dates and applying the cleaning pipeline, the prepared dataset contains **5,000 records**.

The reliable temporal analysis focuses on **2024**. A small group of isolated 2026 dates was preserved and flagged as temporally suspicious.

---

## 🔎 Analysis Process

- Assessed missing values, duplicate records, encoded sentinel values, numerical distributions, and city-state consistency.
- Investigated the relationship between `price`, `quantity`, and `order_value` to evaluate possible quantity reconstruction.
- Defined cleaning decisions before transformation, separating supported corrections from uncertain inconsistencies.
- Built and validated a reproducible cleaning pipeline with record-level quality flags.
- Consolidated the cleaning logic into a reusable Python function and verified it against the manually prepared dataset.
- Evaluated how the prepared data and reconstruction decisions affect downstream retail analysis.

---

## 🔑 Key Findings

### Quantity was the main structural data quality issue

The raw dataset contained **2,971 zero quantities despite positive order values**, making `quantity` the most significant structural issue identified during the quality assessment.

The relationship between `order_value` and `price` provided a possible reconstruction method, but required validation before being used as a cleaning rule.

![Cleaning Pipeline Impact Summary](images/cleaning_pipeline_impact.png)

### The quantity reconstruction rule was useful, but not exact

The `order_value / price` relationship reproduced **88.7% of observed positive quantities**. Based on this evidence, the pipeline reconstructed **2,966 zero-quantity records**.

However, the relationship could not be treated as an exact business rule. **21 severe quantity inconsistencies** were preserved and flagged rather than automatically overwritten because the available data could not determine whether the error originated in `quantity`, `price`, or `order_value`.

### Quantity-based analysis depends heavily on reconstructed values

Reconstructed quantities represent **59.3% of the prepared dataset**.

Recorded quantities have a median of **28 units**, compared with **10 units** for reconstructed quantities. This difference should not be interpreted as an independent retail pattern because reconstructed values are directly derived from `order_value` and `price`.

As a result, quantity-based findings require clear consideration of whether values were originally recorded or reconstructed by the pipeline.

### Quality flags preserve usable records while making uncertainty visible

The pipeline preserves **15 temporal anomalies** and **21 severe quantity inconsistencies** through record-level flags instead of automatically deleting or correcting them.

For temporal analysis, the suspicious 2026 records were excluded from the monthly aggregation while remaining available in the prepared dataset. Monthly order activity across the reliable 2024 period remained broadly stable, ranging from **380 to 461 orders per month**.

![Monthly Order Activity in 2024](images/monthly_order_activity.png)

This approach allows records to remain available when they are useful for other analytical questions without treating every field as equally reliable.

---

## 💡 Recommendations

- **Preserve quality flags in downstream analysis.** Fields such as `quantity_reconstructed`, `quantity_inconsistency`, and `temporal_anomaly` should remain available so analysts can select appropriate records for each analytical question.
- **Investigate the source of zero quantities.** The high number of reconstructed records suggests a possible issue in the original quantity capture or data transfer process.
- **Validate the relationship between `price`, `quantity`, and `order_value` with business rules.** A confirmed calculation rule would allow more confident correction of cross-variable inconsistencies.
- **Separate recorded and reconstructed quantities when quantity is a key metric.** Analyses of units sold or quantity distributions should clearly identify their dependence on reconstructed values.
- **Review the isolated 2026 dates before extending temporal analysis.** Their source should be confirmed before they are included in trend or seasonality analysis.

---

## ⚠️ Analysis Limitations

The quantity reconstruction method is based on an inferred relationship between `order_value` and `price`, not on a confirmed business rule.

For severe cross-variable inconsistencies, the dataset does not provide enough information to identify whether `quantity`, `price`, or `order_value` contains the incorrect value.

Because **59.3% of quantity values were reconstructed**, quantity-based analysis is highly dependent on the cleaning decisions applied in this project.

The reliable temporal coverage is limited to one year of monthly activity, which is not sufficient to establish recurring seasonality.

---

## 🗂️ Repository Structure

```text
analysis_retail_data_quality/
│
├── README.md
├── analysis_retail_data_quality.ipynb
│
├── data/
│   ├── raw/
│   │   └── retail_sales.csv
│   └── processed/
│       └── retail_clean.csv
│
└── images/
    ├── cleaning_pipeline_impact.png
    └── monthly_order_activity.png
```

--- 

## 🛠️ Tools and Technologies

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- Google Colab
- GitHub

---

## 🔄 Reproduction Process

The complete analysis can be executed directly in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rojaspetit/analysis_retail_data_quality/blob/main/analysis_retail_data_quality.ipynb)

1. Open the notebook using the **Open in Colab** button.
2. Select **Runtime → Run all**.
3. The notebook automatically clones the GitHub repository and loads the raw dataset from `data/raw/retail_sales.csv`.
4. The cleaning pipeline, validation checks, exploratory analysis, and output generation run in sequence.

No manual dataset upload or additional configuration is required.

---

## 👤 Author

Edgar Rojas

Data Analytics Portfolio Project
