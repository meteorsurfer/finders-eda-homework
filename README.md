# 📊 Data Analyst Take-Home Challenge Report

**Author:** Alan Perry C. Daen
**Project:** Finder Awards Satisfaction Survey ETL Pipeline

---

## 1. Introduction

This project demonstrates my ability to design and execute a full **ETL (Extract, Transform, Load)** pipeline using Python (Jupyter Notebook) and Google BigQuery. The dataset comes from a hypothetical **Finder Awards satisfaction survey**, which collected responses from nominees, Finder crew, and other participants (250 respondents in total).

The goal of the ETL process was to:

* Clean and standardize the raw survey data.
* Transform it into a scalable analytical format.
* Load it into BigQuery for further analysis and dashboarding.

This reflects the Finder Data Analyst role requirements: **working with large datasets, transforming messy survey data, and deploying it into modern data warehousing solutions (BigQuery)**.

---

## 2. ETL Process Overview

### **Extract**

* Imported the raw survey dataset into a Jupyter environment.
* Inspected schema and column structure for consistency.
* Identified key metadata: respondent type, event categories, and satisfaction ratings.

---

### **Transform**

1. **Reshaping Survey Data**

   * Used `pandas.melt()` to convert the dataset from wide format to **long/tidy format**, enabling scalability and flexible aggregation across multiple ratings and categories.

   ```python
   finder_long = finder_df.melt(
       id_vars=["Event Type", "Category"],
       var_name="Rating",
       value_name="Count"
   )
   ```

2. **Mapping Likert Ratings to Numeric Scores**

   * Mapped satisfaction levels (e.g., *Very Dissatisfied → 1*, *Very Satisfied → 5*).
   * Ensured correct data types (`int` for counts, `float` for scores).

   ```python
   finder_long["Score"] = finder_long["Rating"].map(likert_map)
   finder_long["Score"] = pd.to_numeric(finder_long["Score"], errors="coerce")
   finder_long["Count"] = pd.to_numeric(finder_long["Count"], errors="coerce")
   ```

3. **Data Quality Assurance**

   * Checked for nulls and coerced invalid entries to maintain schema consistency.
   * Normalized text columns (trimmed spaces, standardized capitalization).

---

### **Load**

1. **Schema Definition in BigQuery**

   * Designed schema with fields:

     * `id` (INTEGER)
     * `event_type` (STRING)
     * `category` (STRING)
     * `rating` (STRING)
     * `count` (INTEGER)
     * `score` (FLOAT)

2. **Table Creation**

   * Created a target table in **Google BigQuery**:

   ```python
   dataset_id = "finders-au.finder_award"
   table_id = f"{dataset_id}.take_home_assignment"
   ```

   * Successfully created table:

     ```
     Created table finders-au.finder_award.take_home_assignment
     ```

3. **Data Load**

   * Uploaded the transformed `finder_long` DataFrame into BigQuery.
   * Verified row counts and schema consistency.

---

## 3. Results & Insights

✅ The dataset is now structured in a **fact table format**, optimized for:

* **Chi-Square Testing** to evaluate whether satisfaction scores differ significantly across event types, categories, or respondent groups.
* **Dashboarding** in Looker Studio or Data Studio.
* **Forecasting & planning** (e.g., satisfaction trends by respondent type).

✅ By scoring Likert responses, we enabled **quantitative comparisons** across categories.

✅ The BigQuery load ensures scalability and alignment with Finder’s **modern data stack**.

---

## 4. Alignment with Finder Job Role

This ETL project highlights:

* **Experience with BigQuery:** Designed schemas, created tables, and loaded cleaned data.
* **Data wrangling in Python (pandas):** Melted wide survey data into long analytical format.
* **Applied statistics foundations:** Converted qualitative survey results into numerical scores.
* **Collaboration-ready outputs:** Dataset is structured for dashboarding and stakeholder reporting.
* **Scalable infrastructure mindset:** Pipeline can be automated for recurring surveys.

---

## 5. Conclusion

This take-home demonstrates my ability to:

* Extract raw survey data.
* Transform it into a structured analytical format.
* Load it into BigQuery for business-ready insights.
* Connected load BigQuery table as source to [Looker Dashboard](https://lookerstudio.google.com/reporting/284ed259-394d-4ea8-acf1-cca13626a1fb)

It showcases the same skills Finder is seeking: **data cleaning, scalable pipelines, applied analytics, and warehouse integration**—all while keeping results transparent and actionable.

---
## Detailed Recommendations Based on the Analysis
See this [document](https://docs.google.com/document/d/18CHc4_01vak49MUgSxTF8eH5StxwQ83DIWHF6Ar1pTE/edit?usp=sharing)
---
