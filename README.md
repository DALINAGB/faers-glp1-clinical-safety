# FDA FAERS: GLP-1 Receptor Agonists Clinical Safety & Pharmacovigilance Analysis

## 1. GENERAL INFORMATION
- **Project Name:** GLP-1 Receptor Agonists Post-Marketing Safety & Adverse Event Monitoring
- **Specialty:** Data Analytics / Clinical Data Science
- **Data Source:** FDA Adverse Event Reporting System (FAERS) - Quarterly Data Extract (2026 Q2)
- **Source Link:** [FDA FAERS Quarterly Data Files](https://open.fda.gov/data/faers/)
- **Published Project Link:** *Pending deployment*

---

## 2. OBJECTIVE
Analyze post-marketing safety data from the FDA FAERS database to identify reporting patterns, adverse event profiles, and severe clinical outcomes (hospitalization, life-threatening events, death) associated with GLP-1 receptor agonists (e.g., Semaglutide, Tirzepatide). This project provides actionable pharmacovigilance insights to support safety operations, regulatory triage, and clinical decision-making within pharmaceutical sponsors and Contract Research Organizations (CROs).

---

## 3. WORK PLAN
1. **Data Ingestion & Structural Inspection:** Inspect relational ASCII files (`DEMO`, `DRUG`, `REAC`, `OUTC`) and filter out retracted cases listed in `DELETE26Q2.txt`.
2. **Data Cleaning & Standardization:** Standardize patient ages into years (handling days/months conversions), isolate Primary Suspect drugs (`ROLE_COD = 'PS'`), and normalize GLP-1 active ingredient terminology.
3. **Analytical Construction & Signal Screening:** Build optimized SQL/Python queries to compute reporting proportions and contingency matrices for adverse outcomes linked to target therapies.
4. **Evaluation & Clinical Benchmarking:** Cross-reference flagged adverse reactions against known product label safety profiles and evaluate demographic distribution biases.
5. **Insights & Executive Documentation:** Synthesize operational takeaways into visual dashboards and document reproducible pipeline workflows.

---

## 4. KEY QUESTIONS
1. *How do reporting rates for severe outcomes (hospitalizations/deaths) compare between GLP-1 therapies and other post-marketing treatments in the quarterly extract?*
2. *Which specific MedDRA Preferred Terms (PTs) exhibit the highest reporting volume when GLP-1s are designated as the Primary Suspect drug?*
3. *What demographic cohorts (by age bracket and biological sex) concentrate the majority of reported adverse events?*

---

## 5. TECHNICAL DECISIONS & METHODOLOGY
- **Database Relational Integrity:** Joined tables using `primaryid` and cross-referenced cases against deletion logs to prevent double-counting.
- **Exposure Definition:** Filtered exclusively on `ROLE_COD = 'PS'` to minimize confounding noise from concomitant medications.
- **Demographic Standardization:** Standardized multi-unit age entries (`AGE_COD` across years, months, and days) into a normalized continuous numeric feature.
- **Tooling Stack:** SQL (DuckDB/BigQuery) for high-performance relational aggregation; Python (Pandas) for data structuring and pipeline orchestration.

---

## 6. RESULTS
*(In progress: Metrics, top drug-reaction pairs, and outcome distributions to be updated upon pipeline execution).*

---

## 7. CONCLUSIONS & TAKEAWAYS
*(To be detailed upon project completion, including regulatory caveats regarding spontaneous reporting limitations and interview-ready talking points).*

---

## 8. PRE-PUBLICATION CHECKLIST
- [x] Clear README defining business context and clinical rationale
- [ ] Clean folder structure (`/data`, `/sql`, `/scripts`, `/assets`)
- [x] `.gitignore` configured to prevent raw text files (>50MB) from being committed
- [x] Direct link to authoritative FDA data source included
- [ ] Published repository shared with coach Jhon Barrios
