# Impact-Reporting-of-Returnees-Returned-Displaced-Persons-2001-2024-
This project analyzes global displacement and return trends from **2001 to 2024**, using UNHCR returnee data sourced via the HDX Humanitarian API

# Tracking Homecomings: Analyzing Global Returnee Patterns (2001–2024)

## 📘 Overview

This project investigates global return patterns of forcibly displaced populations over a 23-year period, using data from the **UNHCR Returnees dataset**, accessed via the **Humanitarian Data Exchange (HDX) Humanitarian API (HAPI)**.

The dataset contains standardized return information for two key population groups:

- **Returnees (RET):** Refugees who returned to their countries of origin after a period of asylum.
- **Returned Displaced Persons (RDP):** Internally displaced individuals who returned to their home communities after a crisis.

This project was designed not only as a technical exercise in data wrangling, visualization, and analysis — but also as a tool with **strategic humanitarian value**. By understanding who is returning, where, and when — humanitarian agencies and donors can better target funding, plan reintegration programs, and forecast long-term pressure on post-conflict communities.

---

## 🎯 Project Goals

The key analytical objectives of this work were to explore:

- **Where are displaced people returning to?**
- **Which countries, regions, and continents bear the highest return burdens?**
- **What are the demographic return patterns — by age and gender?**
- **How have these trends evolved year-by-year from 2001 to 2024?**

This project provides a foundational framework for building **data-informed reintegration policies**, **humanitarian dashboards**, and **impact assessments** across global displacement contexts.

---

## 📡 Data Source: HDX HAPI – Affected People: Returnees

This project utilizes data from the **HDX Humanitarian API (HAPI)**, which standardizes humanitarian indicators for use across platforms. The core dataset was retrieved from:

**Dataset:** [HDX – Affected People: Returnees](https://data.humdata.org/)  
**Source Organization:** UNHCR – The UN Refugee Agency  
**Time Period:** January 1, 2001 to December 31, 2024  
**Update Frequency:** Annually  
**Global Scope:** Data spans every reporting country with returnee records.

The HDX HAPI supports automated ingestion and dashboard-ready formats. Data flagged with warnings (e.g., cleaned values or unusual records) were retained. Rows flagged as errors (e.g., missing core fields) were excluded by default, per HDX policy.

---

## 🔍 Variables Used

The cleaned dataset consisted of the following fields:

| Column | Definition |
|--------|------------|
| `population_group` | RET = Returnees, RDP = Returned Displaced Persons |
| `origin_location_code`, `origin_location_name` | Country to which people are returning |
| `asylum_location_code`, `asylum_location_name` | Previous host country (for refugees only) |
| `gender` | m, f, or all |
| `age_range` | Binned age group (e.g., 0-4, 5-11, 60+, or "all") |
| `population` | Number of people in this segment |
| `reference_period_start`, `reference_period_end` | Date range of the return |
| `warning`, `error` | Data quality indicators (rows with `error` were excluded) |

---

## 🧽 Data Cleaning and Transformation

All cleaning was performed in Excel with reproducible logic, suitable for automation in Power Query or Python.

### 1. Demographic Fixes
Age ranges were inconsistently formatted due to Excel’s date-parsing errors (e.g., "11-May" → serial date). These were manually restored and recategorized into:

- **Children:** 0–17
- **Adults:** 18–59
- **Elderly:** 60+

```excel
=IF(OR(G2="0-4",G2="5-11",G2="12-17"),"Children",
   IF(G2="18-59","Adult",
   IF(G2="60+","Elderly","Unknown")))
