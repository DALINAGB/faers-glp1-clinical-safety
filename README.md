# FDA FAERS: GLP-1 Receptor Agonists Clinical Safety & Pharmacovigilance Analysis

## 1. GENERAL INFORMATION
- **Project Name:** FDA FAERS: GLP-1 Receptor Agonists Clinical Safety & Post-Marketing Surveillance
- **Specialty:** Clinical Data Science / Healthcare Business Intelligence (DS/DA)
- **Data Source:** United States Food and Drug Administration (FDA) Adverse Event Reporting System (FAERS), 2026 Q2 Quarterly Extract
- **Source Link:** [FDA FAERS Public Quarterly Data](https://fis.fda.gov/extensions/FPD-QDE/FPD-QDE.html)
- **Interactive Google Colab Notebook:** [01_glp1_faers_analysis.ipynb (Interactive Pipeline)](https://colab.research.google.com/drive/1stjdLcTXTu42OhpjNLzkH6QVfBRW4Cw8?usp=sharing)
- **Repository URL:** [https://github.com/DALINAGB/faers-glp1-clinical-safety](https://github.com/DALINAGB/faers-glp1-clinical-safety)

---

## 2. OBJECTIVE
This project designs an audit-grade clinical pharmacovigilance pipeline to monitor post-marketing adverse drug reactions (ADRs) and severe patient outcomes associated with FDA-approved GLP-1 receptor agonists (Tirzepatide, Semaglutide, Dulaglutide, Liraglutide, Exenatide). By ingesting raw spontaneous reports from FAERS, it resolves confounding multi-ingredient mixtures, differentiates clinical toxicities from autoinjector device failures, and stratifies clinical severity (Death, Hospitalization, Life-Threatening events) across diabetic versus obesity populations to inform regulatory and clinical safety teams.

---

## 3. WORK PLAN
1. **Initial Exploration & Ingestion:** Connect local FDA FAERS text extracts (`DRUG26Q2`, `REAC26Q2`, `OUTC26Q2`, `DELETE26Q2`) directly into DuckDB within Google Colab for low-latency SQL querying on large clinical datasets[span_0](start_span)[span_0](end_span)[span_1](start_span)[span_1](end_span).
2. **Clinical Cohort Harmonization:** Purge officially retracted cases, filter Primary Suspect (`role_cod = 'PS'`) exposures, exclude compounded/off-label peptide mixtures and fixed-ratio insulin combinations, and consolidate delivery device brand nomenclature into clean monotherapy cohorts[span_2](start_span)[span_2](end_span)[span_3](start_span)[span_3](end_span).
3. **Outcome Stratification & Severity Triage:** Execute relational left joins with patient outcomes (`OUTC`) to measure hospitalization and critical seriousness rates per ICH E2D regulatory criteria[span_4](start_span)[span_4](end_span)[span_5](start_span)[span_5](end_span).
4. **Adverse Event Landscape & Taxonomy Layer:** Map MedDRA Preferred Terms (`PT`), expand analytical depth from truncated rankings to complete event libraries, and categorize events into clinical biological reactions versus operational/device friction[span_6](start_span)[span_6](end_span)[span_7](start_span)[span_7](end_span).
5. **Statistical Signal Detection & Visualization (In Progress):** Calculate Proportional Reporting Ratios (PRR) and Reporting Odds Ratios (ROR) for identified safety signals, followed by BI dashboard consolidation in Looker Studio / Tableau.

---

## 4. KEY QUESTIONS
1. *Cohort Integrity:* How do we prevent compounding pharmacies and multi-ingredient off-label mixtures from contaminating brand-specific safety assessments?
2. *Reporting Artifacts:* How much of the spontaneous reporting volume represents pharmacological toxicity versus combination product friction (autoinjector pen misfires, dosing confusion, lack of weight loss efficacy)?
3. *Severity Bias:* Are higher hospitalization rates driven by intrinsic molecular differences between Tirzepatide and Semaglutide, or by baseline cardiovascular and metabolic comorbidities within diabetic cohorts versus obesity cohorts?

---

## 5. METHODOLOGY & TECHNICAL DECISIONS
* **High-Performance Ingestion:** Used `DuckDB` over native Pandas CSV parsing, enabling multi-table joins on millions of records directly from cloud-mounted storage without memory overflows.
* **Clinical Protocol Exclusions:** Stringent regex/exclusion logic filtered out non-FDA-approved compounding combinations (e.g., cyanocobalamin, pyridoxine, glycine additives) and fixed-ratio insulin combinations (*Soliqua*, *Xultophy*)[span_8](start_span)[span_8](end_span).
* **ICH E2D Regulatory Seriousness:** Preserved non-serious baseline reports via relational `LEFT JOIN` on outcome tables and classified critical cases as those documenting Death (`DE`), Life-Threatening experiences (`LT`), or Inpatient Hospitalization (`HO`)[span_9](start_span)[span_9](end_span)[span_10](start_span)[span_10](end_span).
* **Two-Tier Safety Taxonomy:** Engineered a domain-specific categorization separating genuine pathophysiological reactions from administration errors, device defects, and efficacy expectations[span_11](start_span)[span_11](end_span).

---

## 6. INTERIM RESULTS (AS OF OCTOBER 2026)
* **Harmonized Study Universe:** Out of 36,074 raw candidate cases, established a strictly compliant cohort of **35,849 unique monotherapy patient cases**. Modern incretins dominate total volume: *Zepbound* (37.32%), *Ozempic* (22.62%), *Wegovy* (18.15%), and *Mounjaro* (14.40%).
* **Indication vs. Severity Gap:** Evaluated clinical severity demonstrating that Type 2 Diabetes populations suffer substantially higher critical seriousness rates (*Mounjaro*: 17.26%, *Ozempic*: 11.95%) compared to obesity populations (*Zepbound*: 3.35%, *Wegovy*: 6.97%).
* **Operational Reporting Noise:** Analysis of 27,532 reported mentions for *Zepbound* showed that while **80.66%** reflect biological symptoms, **17.90%** stem from medication dosing errors (9.47%) and perceived lack of efficacy/weight rebound (8.43%).
* **Exhaustive Critical Triage for Zepbound:** Unconstrained extraction across 446 serious cases (412 hospitalizations, 36 deaths, 14 life-threatening events) identified 428 distinct MedDRA terms, revealing key emergency drivers:
  * *Acute Dehydration Cascade:* `DEHYDRATION` (50 hospitalizations, 1 death) secondary to intractable vomiting (69 hospitalizations).
  * *Biliary & Pancreatic Burden:* `CHOLELITHIASIS` (18 critical cases, 1 death), `PANCREATITIS` (20 hospitalizations), and `PANCREATITIS ACUTE` (6 hospitalizations).
  * *Severe Intestinal Dysmotility:* `INTESTINAL OBSTRUCTION` (9 hospitalizations) and `IMPAIRED GASTRIC EMPTYING` (10 hospitalizations).

---

## 7. CONCLUSIONS & INTERIM TAKEAWAYS
* **Regulatory & Clinical Insight:** Evaluating solely top-frequency terms induces severe surveillance bias. High-incidence benign events (nausea, mild diarrhea) mask acute, life-threatening complications (pancreatitis, cholelithiasis, severe volume depletion) unless outcome-stratified triage is enforced.
* **Combination Product Burden:** Significant reporting volume stems from user friction with multi-dose autoinjector pens rather than drug cytotoxicity, emphasizing the need for targeted patient administration training.
* **Interview Talking Point:** Demonstrates proficiency in handling real-world evidence (RWE), implementing international pharmacovigilance standards (ICH E2D, MedDRA PTs), and executing scalable SQL architectures for post-marketing surveillance.
* **Next Steps:** Extend unconstrained critical outcome extraction across *Wegovy*, *Ozempic*, and *Mounjaro*, compute disproportionality signal metrics (PRR/ROR), and export aggregated datasets to a Looker Studio executive dashboard.

---

## 8. PRE-PUBLICATION CHECKLIST
- [x] README explains clinical context, architecture, and interim results
- [x] Folder and repository structure organized
- [x] Large raw data files (>50MB) excluded via `.gitignore`
- [x] Authoritative FDA FAERS source links documented and functional
- [x] Interactive Google Colab notebook shared with public view access
- [x] Interim milestone submitted for mentor/coach review
