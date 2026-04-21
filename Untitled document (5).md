# **Health Actuarial — ACO REACH PY2025 Final Settlement Risk Score Cascade (Noisy Extract)**

## **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Actuarial / Medicare Accountable Care — Risk Adjustment, Normalization, and Coding Intensity Settlement  
* **Workflow:** ACO-Level PY2025 Final Risk Score Cascade — Blended V24/V28 Raw → Post-Blend Normalization → Demographic-Adjusted ±3% Cap → Restricted (≤1.010) Retrospective CIF → Separate ESRD Sub-Cascade  
* **Prompt Type:** ACO-Level Final Settlement Risk Score Memorandum  
* **Difficulty:** Singularity Tier (Expected Failure Rate \> 99.5%)

---

## **2\. Prompt**

You are the Lead Risk Adjustment Actuary for **Pelican Health Partners (PHP)**, a Standard ACO participating in the ACO REACH Model (ACO\_ID: `REACH-PHP-0042`). The current date is **February 14, 2026**, and the CMS Innovation Center has opened a final settlement review because PHP's preliminary PY2025 capped and CIF-adjusted risk scores, as reported in the December 2025 Quarterly Benchmark Report, do not tie out to the numbers produced by PHP's internal actuarial team.

The file `aco_php_py2025_settlement_extract.csv` is **not curated**. It was pulled from a federated data mart that does not enforce reference-year, model-version, alignment-cohort, or ACO-ID joins at the source. The extract mixes, in arbitrary order:

* Reference-year (RY2022) and performance-year (PY2025) ACO-level aggregated rows  
* Aged & Disabled (A\&D) and End-Stage Renal Disease (ESRD) benchmark populations  
* Claims-aligned beneficiaries, continuously voluntarily-aligned beneficiaries (in their second or later model performance year), and newly voluntarily-aligned beneficiaries (in their first model performance year of alignment)  
* **Unblended** V24-only and V28-only A\&D raw risk score rows alongside a **correctly blended** 33% V24 / 67% V28 row, plus one hand-blended row that was computed incorrectly by an analyst before the PY2025 blend rule was finalized  
* Normalization factors drawn from the ACO REACH National Reference Population  
* A decoy row attributable to a different ACO (`REACH-OAK-0117`) that was mis-joined by the upstream extract  
* A row flagged `do_not_use = Y` by the data steward because it represents an out-of-scope NGACO reference-period mean mistakenly surfaced into the REACH namespace  
* A row tagged with an off-policy reference year (2023) that reflects a **rolling** reference-year convention, which does **not** apply to Standard/New Entrant ACOs in PY2025  
* Pricing and Rate Book rows (baseline PMPM, regional rate) that are inputs to the benchmark but are **not** inputs to the risk-score cascade  
* Beneficiary-count rows needed to verify minimum-threshold and population-growth guardrails for cap application

Using the attached **ACO REACH and Kidney Care Choices Models PY2025 Risk Adjustment** paper (RTI International, published September 27, 2024, Rev. 1.1 — hereafter "Risk Adjustment Paper"), the **2024 MA Rate Announcement** for CMS-HCC V28 coefficient context, and the noisy settlement extract, prepare an **ACO-Level Final Settlement Risk Score Memorandum** addressed to the PY2025 Settlement File. The memorandum must contain the following sections, in order, with explicit inline citations to `record_id` values, Risk Adjustment Paper table/section references, and all intermediate arithmetic carried to at least four decimal places:

1. **Controlling Row Set and Rejections.** Identify the single controlling row for each of: RY2022 A\&D raw V24 mean, RY2022 A\&D raw V28 mean, RY2022 A\&D demographic mean, RY2022 ESRD raw V24 mean, RY2022 A\&D normalization factor, RY2022 ESRD normalization factor, PY2025 A\&D raw V24 mean, PY2025 A\&D raw V28 mean, PY2025 A\&D demographic mean, PY2025 ESRD raw V24 mean, PY2025 A\&D normalization factor (preliminary), PY2025 ESRD normalization factor (preliminary), PY2025 A\&D retrospective CIF (preliminary, unrestricted), PY2025 ESRD retrospective CIF (preliminary, unrestricted), and the beneficiary-count rows required for thresholds. For every row not selected, state the specific reason for rejection (wrong benefit year, wrong ACO, DNU flag, wrong model version, wrong alignment cohort, wrong reference-year convention, wrong population segment, or out-of-scope record type).  
2. **Minimum-Threshold and Population-Growth Guardrails.** State, with citation to the Risk Adjustment Paper, the minimum-beneficiary thresholds for cap application separately for A\&D (Standard/New Entrant) and ESRD in PY2025, and the rule that the PY population subject to the A\&D cap may not exceed three times the RY reference population. Compute the effective RY2022 and PY2025 cap populations — explicitly excluding first-year voluntarily-aligned beneficiaries from both the PY cap/CIF population and the RY reference — and conclude whether the cap applies for each benchmark.  
3. **A\&D Blended Raw Risk Score Construction.** Construct the RY2022 and PY2025 A\&D blended raw mean risk scores using the PY2025 blending rule (33% V24 \+ 67% V28). Explicitly reject any pre-blended hand-calculated row and explain why the Risk Adjustment Paper requires post-blend normalization rather than normalization of each model version separately followed by blending.  
4. **A\&D Normalization.** Divide each blended raw mean by the correct ACO REACH National Reference Population normalization factor for its year, citing Appendix C Table C-1 values for RY2022 and the preliminary PY2025 factor supplied in the extract. Note that Table C-1 lists the 2025 factor as TBD at the time of paper publication; use the preliminary value supplied in the extract and flag it as provisional until final settlement.  
5. **A\&D Demographic Risk Score Growth.** Compute the PHP A\&D demographic risk score growth rate `d` using the Shared Savings Program demographic risk score formula cited in the Risk Adjustment Paper. Derive the demographic-adjusted upper cap ceiling (`d + 3%`) and lower cap floor (`d − 3%`).  
6. **A\&D Cap Decision and Capped Mean Normalized Risk Score.** Compute the PHP A\&D risk score growth rate `g`. Apply the three-branch decision rule from Appendix C — `g > d + 3%`, `d − 3% ≤ g ≤ d + 3%`, `g < d − 3%` — and compute the resulting A\&D capped mean normalized risk score.  
7. **A\&D Restricted CIF and Final Coding-Adjusted Risk Score.** Apply the PY2025 A\&D retrospective CIF, restricted to no greater than 1.010 per the Risk Adjustment Paper. Compute the final A\&D settled risk score as capped ÷ restricted CIF.  
8. **ESRD Sub-Cascade.** Build the separate ESRD cascade. State, with citation, that (a) ESRD uses V24 only — there is no V28 blend for ESRD in PY2025, (b) the A\&D demographic adjustment to the cap does not apply to ESRD, and (c) ESRD retains a straight ±3% symmetric cap on normalized risk score growth. Compute the RY2022 and PY2025 ESRD normalized means, the growth rate, the capped normalized value, and the final ESRD settled risk score after division by the restricted ESRD CIF.  
9. **Voluntarily-Aligned Reconciliation.** Explain in one paragraph how the mis-inclusion of first-year voluntarily-aligned beneficiaries would have shifted the PY2025 A\&D cap and CIF population means, and why the Risk Adjustment Paper requires their exclusion in the first model performance year of alignment but their inclusion in the second and later years.  
10. **Final Settled Numbers.** State PHP's PY2025 final settled A\&D risk score and final settled ESRD risk score to four decimal places, with a one-sentence explanation of what each number feeds into downstream (benchmark risk adjustment and capitated payment risk adjustment).

---

### **Attached Data: `aco_php_py2025_settlement_extract.csv`**

record\_id,aco\_id,benefit\_year,population,record\_type,alignment\_cohort,model\_version,metric,value,beneficiary\_count,do\_not\_use,comment  
AD-RY22-V24,REACH-PHP-0042,2022,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V24,mean\_risk\_score,0.970,,N,RY2022 A\&D unblended V24 raw mean  
AD-RY22-V28,REACH-PHP-0042,2022,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V28,mean\_risk\_score,1.020,,N,RY2022 A\&D unblended V28 raw mean  
AD-RY22-DEM,REACH-PHP-0042,2022,A\&D,demographic\_mean\_risk\_score,claims\_plus\_cont\_va,SSP\_Demographic,mean\_risk\_score,0.980,,N,RY2022 A\&D demographic mean  
AD-RY22-NF,NATIONAL\_REF,2022,A\&D,normalization\_factor,reference\_population,Blend\_67\_33,norm\_factor,1.070,,N,Table C-1 RY2022 factor (67/33 blend)  
AD-RY22-POP1,REACH-PHP-0042,2022,A\&D,population\_count,claims\_aligned,n/a,bene\_count,,2450,N,RY2022 A\&D claims-aligned beneficiary count  
AD-RY22-POP2,REACH-PHP-0042,2022,A\&D,population\_count,continuously\_va\_2plus,n/a,bene\_count,,180,N,RY2022 A\&D continuously VA (2nd yr or later)  
AD-PY25-V24,REACH-PHP-0042,2025,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V24,mean\_risk\_score,1.030,,N,PY2025 A\&D unblended V24 raw mean  
AD-PY25-V28,REACH-PHP-0042,2025,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V28,mean\_risk\_score,1.160,,N,PY2025 A\&D unblended V28 raw mean  
AD-PY25-BAD-BLEND,REACH-PHP-0042,2025,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,Analyst\_HandBlend,mean\_risk\_score,1.095,,N,Hand-blended value from 2024 draft memo using 50/50 weights; do not use for PY2025  
AD-PY25-DEM,REACH-PHP-0042,2025,A\&D,demographic\_mean\_risk\_score,claims\_plus\_cont\_va,SSP\_Demographic,mean\_risk\_score,1.010,,N,PY2025 A\&D demographic mean  
AD-PY25-NF,NATIONAL\_REF,2025,A\&D,normalization\_factor,reference\_population,Blend\_67\_33,norm\_factor,1.105,,N,Preliminary PY2025 factor (Table C-1 shows TBD)  
AD-PY25-POP1,REACH-PHP-0042,2025,A\&D,population\_count,claims\_aligned,n/a,bene\_count,,2700,N,PY2025 A\&D claims-aligned  
AD-PY25-POP2,REACH-PHP-0042,2025,A\&D,population\_count,continuously\_va\_2plus,n/a,bene\_count,,220,N,PY2025 A\&D continuously VA (2nd yr or later)  
AD-PY25-POP3,REACH-PHP-0042,2025,A\&D,population\_count,first\_year\_va,n/a,bene\_count,,195,N,PY2025 A\&D newly VA (first model PY of alignment) — alignment-eligible but excluded from cap/CIF  
AD-PY25-CIF,MODEL\_WIDE,2025,A\&D,retrospective\_cif,all\_aligned,n/a,cif\_value,1.018,,N,CMS preliminary model-wide A\&D CIF (unrestricted)  
ESRD-RY22-V24,REACH-PHP-0042,2022,ESRD,raw\_mean\_risk\_score,claims\_aligned,V24,mean\_risk\_score,0.950,,N,RY2022 ESRD V24-only raw mean  
ESRD-RY22-NF,NATIONAL\_REF,2022,ESRD,normalization\_factor,reference\_population,V24\_ESRD,norm\_factor,1.025,,N,RY2022 ESRD V24 normalization factor  
ESRD-RY22-POP,REACH-PHP-0042,2022,ESRD,population\_count,claims\_aligned,n/a,bene\_count,,62,N,RY2022 ESRD beneficiary count  
ESRD-PY25-V24,REACH-PHP-0042,2025,ESRD,raw\_mean\_risk\_score,claims\_aligned,V24,mean\_risk\_score,1.050,,N,PY2025 ESRD V24-only raw mean  
ESRD-PY25-NF,NATIONAL\_REF,2025,ESRD,normalization\_factor,reference\_population,V24\_ESRD,norm\_factor,1.055,,N,Preliminary PY2025 ESRD normalization factor  
ESRD-PY25-POP,REACH-PHP-0042,2025,ESRD,population\_count,claims\_aligned,n/a,bene\_count,,58,N,PY2025 ESRD beneficiary count  
ESRD-PY25-CIF,MODEL\_WIDE,2025,ESRD,retrospective\_cif,all\_aligned,n/a,cif\_value,1.015,,N,CMS preliminary model-wide ESRD CIF (unrestricted)  
AD-RY23-V28,REACH-PHP-0042,2023,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V28,mean\_risk\_score,1.045,,N,RY2023 A\&D V28 mean — rolling-RY convention; NOT the PY2025 reference year for Standard ACOs  
DECOY-OAK,REACH-OAK-0117,2022,A\&D,raw\_mean\_risk\_score,claims\_plus\_cont\_va,V28,mean\_risk\_score,1.250,,N,Different ACO mis-joined by upstream extract  
DECOY-NGACO,NGACO-LEGACY-099,2020,A\&D,raw\_mean\_risk\_score,aligned,V22,mean\_risk\_score,0.920,,Y,Out-of-scope NGACO PY2020 reference mean; data steward flagged DNU  
PRICING-BASE,REACH-PHP-0042,2025,A\&D,baseline\_pmpm,n/a,n/a,pmpm,1340.00,,N,PY2025 A\&D baseline benchmark PMPM — feeds benchmark not risk score cascade  
PRICING-REG,REACH-PHP-0042,2025,A\&D,regional\_rate\_pmpm,n/a,n/a,pmpm,1412.55,,N,PY2025 A\&D regional rate from Rate Book — feeds benchmark not risk score cascade  
---

## **Golden Response**

**ACO-LEVEL FINAL SETTLEMENT RISK SCORE MEMORANDUM** **TO:** PY2025 ACO REACH Settlement File — Pelican Health Partners (ACO\_ID: REACH-PHP-0042) **FROM:** Lead Risk Adjustment Actuary **DATE:** February 14, 2026 **SUBJECT:** Reconciliation of PY2025 Capped and CIF-Adjusted A\&D and ESRD Risk Scores

---

### **1\. Controlling Row Set and Rejections**

The controlling rows for the PY2025 risk score cascade are as follows. All cited `record_id` values are from `aco_php_py2025_settlement_extract.csv`.

* **RY2022 A\&D raw V24 mean \= 0.970** (AD-RY22-V24).  
* **RY2022 A\&D raw V28 mean \= 1.020** (AD-RY22-V28).  
* **RY2022 A\&D demographic mean \= 0.980** (AD-RY22-DEM).  
* **RY2022 A\&D normalization factor \= 1.070** (AD-RY22-NF; Risk Adjustment Paper, Appendix C, Table C-1).  
* **RY2022 A\&D claims-aligned beneficiary count \= 2,450; continuously VA (2nd year+) \= 180** (AD-RY22-POP1, AD-RY22-POP2).  
* **PY2025 A\&D raw V24 mean \= 1.030** (AD-PY25-V24).  
* **PY2025 A\&D raw V28 mean \= 1.160** (AD-PY25-V28).  
* **PY2025 A\&D demographic mean \= 1.010** (AD-PY25-DEM).  
* **PY2025 A\&D normalization factor \= 1.105 (preliminary)** (AD-PY25-NF; Risk Adjustment Paper, Appendix C, Table C-1 — 2025 value listed as TBD at publication, preliminary factor used pending final settlement retrospective adjustment).  
* **PY2025 A\&D claims-aligned \= 2,700; continuously VA (2nd year+) \= 220; first-year VA \= 195 (excluded from cap/CIF population)** (AD-PY25-POP1, AD-PY25-POP2, AD-PY25-POP3).  
* **PY2025 A\&D preliminary unrestricted CIF \= 1.018** (AD-PY25-CIF).  
* **RY2022 ESRD raw V24 mean \= 0.950; normalization factor \= 1.025; beneficiary count \= 62** (ESRD-RY22-V24, ESRD-RY22-NF, ESRD-RY22-POP).  
* **PY2025 ESRD raw V24 mean \= 1.050; preliminary normalization factor \= 1.055; beneficiary count \= 58** (ESRD-PY25-V24, ESRD-PY25-NF, ESRD-PY25-POP).  
* **PY2025 ESRD preliminary unrestricted CIF \= 1.015** (ESRD-PY25-CIF).

**Rejected rows, with reasons:**

* **AD-PY25-BAD-BLEND** rejected. This row carries a hand-blended value of 1.095 computed under 50/50 draft weights. The PY2025 blend rule requires 33% V24 \+ 67% V28 per the Risk Adjustment Paper, Section V.i, and the paper further specifies (Section V.iv) that normalization is applied to the blended risk score **after** the blend is computed, not to pre-blended analyst constructions. This row would corrupt the cascade.  
* **AD-RY23-V28** rejected. Reference year for the PY2025 ±3% cap is RY2022, not RY2023, per Risk Adjustment Paper Table 6 ("Reference Population for Applying the Symmetric 3% Cap"), which specifies a **static** RY of 2022 for PY2024, PY2025, and PY2026 for Standard and New Entrant ACOs. The rolling-RY convention in this row is a Kidney Care Choices Options pattern (Risk Adjustment Paper, Table 13\) and does not apply to Standard ACO REACH.  
* **DECOY-OAK** rejected. `aco_id = REACH-OAK-0117` does not match PHP's ACO\_ID `REACH-PHP-0042`.  
* **DECOY-NGACO** rejected. `do_not_use = Y`, `aco_id = NGACO-LEGACY-099`, and `model_version = V22`. Out-of-scope legacy Next Generation ACO record.  
* **PRICING-BASE** and **PRICING-REG** rejected as out of scope. These are benchmark baseline and regional Rate Book components (Risk Adjustment Paper, Section IV, "Standardizing Blended Benchmark Components"); they feed the standardized Blended Benchmark but not the risk score cascade itself.

### **2\. Minimum-Threshold and Population-Growth Guardrails**

Per the Risk Adjustment Paper, Section V.v, the PY2025 A\&D cap requires a minimum RY2022 reference population of **1,500 Aged/Disabled beneficiaries** and sufficient claims history in the reference year; the cap is additionally skipped when the PY A\&D population subject to the cap exceeds **three times** the RY reference population. The ESRD cap requires a minimum of **50 ESRD beneficiaries** in both the reference year and the performance year, and sufficient claims history in both.

PHP's first-year voluntarily-aligned beneficiaries are excluded from both the PY2025 cap/CIF population and from the RY2022 reference population count, per Risk Adjustment Paper, Section V.v and the "Voluntarily Aligned Beneficiaries" subsection.

* RY2022 A\&D cap population \= 2,450 claims-aligned \+ 180 continuously VA \= **2,630** (AD-RY22-POP1, AD-RY22-POP2). This exceeds the 1,500 threshold, so the A\&D cap is applicable.  
* PY2025 A\&D cap population \= 2,700 claims-aligned \+ 220 continuously VA \= **2,920** (AD-PY25-POP1, AD-PY25-POP2). The 195 first-year VA beneficiaries (AD-PY25-POP3) are excluded. 2,920 / 2,630 \= 1.110×, which is well below the 3× ceiling, so the cap is not skipped.  
* RY2022 ESRD \= 62; PY2025 ESRD \= 58 (ESRD-RY22-POP, ESRD-PY25-POP). Both satisfy the 50-beneficiary floor, so the ESRD cap is applicable.

**Conclusion:** Both the A\&D and ESRD caps apply for PY2025.

### **3\. A\&D Blended Raw Risk Score Construction**

Per Risk Adjustment Paper Section V.i, the PY2025 A\&D blended raw risk score is constructed as `0.33 × V24 + 0.67 × V28`, and per Section V.iv, normalization is applied to the blended risk score — not to each model version separately before blending. The AD-PY25-BAD-BLEND row is therefore excluded (see Section 1 above).

* **RY2022 A\&D blended raw mean** \= 0.33 × 0.970 \+ 0.67 × 1.020 \= 0.3201 \+ 0.6834 \= **1.0035** (AD-RY22-V24, AD-RY22-V28).  
* **PY2025 A\&D blended raw mean** \= 0.33 × 1.030 \+ 0.67 × 1.160 \= 0.3399 \+ 0.7772 \= **1.1171** (AD-PY25-V24, AD-PY25-V28).

### **4\. A\&D Normalization**

Each blended raw mean is divided by the ACO REACH National Reference Population normalization factor for the applicable year (Risk Adjustment Paper, Appendix C, Table C-1).

* **RY2022 A\&D mean normalized** \= 1.0035 / 1.070 \= **0.9379** (AD-RY22-NF).  
* **PY2025 A\&D mean normalized (preliminary)** \= 1.1171 / 1.105 \= **1.0110** (AD-PY25-NF). The PY2025 factor is preliminary; a final normalization adjustment factor will be applied during settlement once actual growth for the reference population is observed, per Section V.iv.

### **5\. A\&D Demographic Risk Score Growth**

Using the Shared Savings Program demographic risk score model referenced in the Risk Adjustment Paper, Section V.v and footnote 17 (age, gender, OREC, and Medicaid dual status — no HCCs):

* **d (A\&D demographic growth rate)** \= (1.010 / 0.980 − 1\) × 100 \= **3.0612%** (AD-RY22-DEM, AD-PY25-DEM).  
* **Demographic-adjusted upper cap ceiling** \= d \+ 3% \= **6.0612%**.  
* **Demographic-adjusted lower cap floor** \= d − 3% \= **0.0612%**.

### **6\. A\&D Cap Decision and Capped Mean Normalized Risk Score**

* **g (ACO A\&D risk score growth rate)** \= (1.0110 / 0.9379 − 1\) × 100 \= **7.7944%**.

Applying the three-branch decision rule from Risk Adjustment Paper, Appendix C:

| Condition | Result |
| ----- | ----- |
| g \> d \+ 3% | **upper cap binds** |
| d − 3% ≤ g ≤ d \+ 3% | PY mean normalized unchanged |
| g \< d − 3% | lower cap binds |

Since 7.7944% \> 6.0612%, the **upper cap binds**. The capped PY2025 A\&D mean normalized risk score is:

* **A\&D capped mean normalized** \= RY normalized × (1 \+ d \+ 3%) \= 0.9379 × (1 \+ 0.030612 \+ 0.030000) \= 0.9379 × 1.060612 \= **0.9947**.

### **7\. A\&D Restricted CIF and Final Coding-Adjusted Risk Score**

Per Risk Adjustment Paper Executive Summary and Section V.v, the PY2025 retrospective CIF is constrained to be no greater than **1.010** (1.0%). The preliminary unrestricted CMS value (AD-PY25-CIF \= 1.018) exceeds this ceiling and is therefore restricted:

* **Restricted A\&D CIF** \= min(1.018, 1.010) \= **1.010**.  
* **Final A\&D coding-adjusted risk score** \= 0.9947 / 1.010 \= **0.9848**.

### **8\. ESRD Sub-Cascade**

Per Risk Adjustment Paper Section V.iii (and footnote 30), the CMS-HCC ESRD model uses V24 only — there is no V28 ESRD model and no blending of ESRD risk scores for PY2025. Per Section V.v and Table 6, the ESRD cap for Standard and New Entrant ACOs is a straight ±3% symmetric cap on normalized risk score growth **without** the demographic adjustment that applies to A\&D.

* **RY2022 ESRD mean normalized** \= 0.950 / 1.025 \= **0.9268** (ESRD-RY22-V24, ESRD-RY22-NF).  
* **PY2025 ESRD mean normalized (preliminary)** \= 1.050 / 1.055 \= **0.9953** (ESRD-PY25-V24, ESRD-PY25-NF).  
* **g (ESRD growth rate)** \= (0.9953 / 0.9268 − 1\) × 100 \= **7.3834%**.

Since 7.3834% \> 3%, the upper straight-3% cap binds:

* **ESRD capped mean normalized** \= 0.9268 × 1.03 \= **0.9546**.  
* **Restricted ESRD CIF** \= min(1.015, 1.010) \= **1.010** (ESRD-PY25-CIF; Risk Adjustment Paper, Section V.v — separate CIF calculated for ESRD to align with the ESRD risk adjustment model).  
* **Final ESRD coding-adjusted risk score** \= 0.9546 / 1.010 \= **0.9452**.

### **9\. Voluntarily-Aligned Reconciliation**

The Risk Adjustment Paper, Section V.v "Voluntarily Aligned Beneficiaries," requires that first-year voluntarily-aligned beneficiaries be excluded from both the retrospective CIF and the symmetric cap because the ACO is not responsible for the initial reporting of the risk score diagnoses used in the CMS-HCC prospective risk adjustment model for those beneficiaries. Their exclusion in PY2025 removes 195 beneficiaries (AD-PY25-POP3) from the PY cap/CIF population, leaving 2,920 beneficiaries in the cap/CIF calculation rather than the gross alignment-eligible count of 3,115. Second-year and later voluntarily-aligned beneficiaries (the 180 RY2022 and 220 PY2025 `continuously_va_2plus` counts) remain included in both the cap and CIF populations even in periods when they have not yet triggered claims-based alignment, per the same section. Had the 195 first-year VA beneficiaries been mis-included, the PY2025 A\&D mean normalized risk score would have drifted toward whatever coding patterns their upstream providers carried — a drift the policy is explicitly designed to prevent during year one.

### **10\. Final Settled Numbers**

* **PHP PY2025 A\&D final settled risk score \= 0.9848** (record\_ids AD-RY22-V24, AD-RY22-V28, AD-RY22-NF, AD-PY25-V24, AD-PY25-V28, AD-PY25-NF, AD-RY22-DEM, AD-PY25-DEM, AD-PY25-CIF; Risk Adjustment Paper Sections V.i, V.iv, V.v, Appendix C, Table C-1, Table 6). This figure feeds the PY2025 A\&D Benchmark risk adjustment and the risk adjustment of PHP's Total Care Capitation and Primary Care Capitation payments for A\&D-aligned beneficiaries (Risk Adjustment Paper, Section IV, "Capitated Payments").  
* **PHP PY2025 ESRD final settled risk score \= 0.9452** (record\_ids ESRD-RY22-V24, ESRD-RY22-NF, ESRD-PY25-V24, ESRD-PY25-NF, ESRD-PY25-CIF; Risk Adjustment Paper Sections V.iii, V.iv, V.v, Table 6). This figure feeds the PY2025 ESRD Benchmark risk adjustment for PHP's ESRD-aligned beneficiary population.

Both figures are provisional pending (a) the final PY2025 normalization factors once observed ACO REACH National Reference Population risk scores are complete, and (b) confirmation of the final model-wide A\&D and ESRD CIF values by CMS during retrospective settlement.

— End of Memorandum —

---

## **Design Notes (for the eval harness, not for the model under test)**

**Traps designed to break weaker models (Opus 4.5 / 4.25 / GPT-4.0 Pro class):**

1. **Blending trap.** Smaller models will often pick either V24-only or V28-only, or use the `AD-PY25-BAD-BLEND` 50/50 hand-blend, or blend in the wrong direction (67% V24 / 33% V28). The correct blend is 33% V24 \+ 67% V28 and the bad hand-blend must be rejected with a cited reason.  
2. **Reference-year static vs rolling trap.** `AD-RY23-V28` is a lure for models that conflate the KCC Options rolling-RY convention (Table 13\) with the Standard ACO static-RY convention (Table 6). For PY2025 under Standard ACO REACH, the RY is 2022\.  
3. **Normalization order trap.** Paper specifies normalization is applied **after** the V24/V28 blend for PY2025 (Section V.iv). Smaller models frequently normalize V24 and V28 separately, then blend normalized values — producing a materially different answer because the normalization factors for each model version differ.  
4. **Demographic-adjusted cap mis-application.** Weaker models apply the ±3% cap without the demographic adjustment (yielding the wrong capped value) or apply the demographic adjustment to ESRD (which is incorrect — ESRD is straight ±3% for Standard/New Entrant ACOs).  
5. **CIF restriction trap.** PY2025 CIF is restricted to ≤1.010. Models that use the raw unrestricted CMS preliminary value (1.018 for A\&D, 1.015 for ESRD) will produce a noticeably lower final risk score.  
6. **First-year VA trap.** AD-PY25-POP3 (195 first-year VA) must be excluded from both cap and CIF populations. Models that sum all three PY beneficiary cohorts into 3,115 will miscompute the 3× population-growth guardrail against the RY reference count and may trip it or cite the wrong denominator.  
7. **Out-of-scope-row trap.** PRICING-BASE and PRICING-REG are benchmark components, not risk-score inputs. Models that pull them into the cascade (e.g., trying to multiply a PMPM against the final capped risk score within the memo) corrupt the answer.  
8. **ACO-ID and DNU traps.** DECOY-OAK and DECOY-NGACO are classic join-hygiene decoys; failing to reject them corrupts the RY mean.  
9. **Minimum-threshold trap.** Must explicitly cite 1,500 (A\&D) and 50 (ESRD) thresholds and the 3× PY-to-RY ceiling from Section V.v. Weaker models skip this check entirely.  
10. **Four-decimal discipline.** The cascade compounds rounding error across 5 multiplications/divisions. Models that round to 2–3 decimals at intermediate steps land on a wrong final figure.

**Key arithmetic fingerprints (for quick grading):**

* A\&D blended raw: RY `1.0035`, PY `1.1171`  
* A\&D normalized: RY `0.9379`, PY `1.0110`  
* `g` \= 7.7944%, `d` \= 3.0612%, upper ceiling \= 6.0612%  
* A\&D capped \= `0.9947`, restricted CIF \= `1.010`  
* **A\&D FINAL \= 0.9848**  
* ESRD normalized: RY `0.9268`, PY `0.9953`  
* `g_esrd` \= 7.3834%  
* ESRD capped \= `0.9546`  
* **ESRD FINAL \= 0.9452**

A response that lands on both 0.9848 and 0.9452 with the right rejection reasons is almost certainly executing the cascade correctly. A response that lands on either number with wrong intermediate reasoning is likely pattern-matching on the original ACA prompt and should be graded down on the reasoning trace even if the final number coincidentally matches.

2nd prompt

Here's what you're getting:

\*\*PROMPT METADATA\*\*  
\- Task Type: Risk Adjustment Reconciliation  
\- Domain: Health Actuarial / ACA Marketplace  
\- Workflow: Noisy Member-Level RAF Audit with Model Hierarchy Inference  
\- Prompt Type: Payment Integrity Determination  
\- Difficulty: Advanced Reasoning (Expected 4.0 Pro failure rate: high)

\---

\*\*THE PROMPT\*\* (concise, one page)

You are a health actuary reviewing member RA-2047's 2025 ACA Silver enrolment. The transfer extract does not reconcile to the raw data feed. The attached CSV contains mixed record types: enrolment, claims diagnoses, pricing cells, pharmacy notes, wrong-year rows, decoy members, and rows flagged do-not-use. Your task: determine the payable 2025 RAF, the Silver transfer pricing cell PMPM (adjusted for age, sex, and induced demand), and total gross transfer dollars for eligible months. Explain which rows are discarded, why, and how you handle HCC hierarchy collisions. Do not assume coefficients; infer them from the attached CMS V28 paper where possible, or note where data is missing. If enrolment and pricing metal levels conflict, flag the discrepancy and justify your choice of controlling row. Do your reconciliation auditable — show all arithmetic to four decimal places.

\---

\*\*GOLDEN RESPONSE\*\*

\*\*Row Disposition:\*\*  
\- \*\*ENR-101\*\* (RA-2047, 2025, Silver, 9 months): Controlling enrolment row. Use this.  
\- \*\*PRC-401\*\* (RA-2047, 2025, Silver PMPM 612.845, AV 1.0145, IDF 1.0365): Correct pricing cell. Use this.  
\- \*\*DX-201, DX-203, DX-204\*\* (HCC 37, 226, 238 respectively): Payable diagnoses, all accepted, 2025, matching member and metal. Use all three.  
\- \*\*DX-202\*\* (HCC 38): Payable but \*\*subordinate to HCC 37\*\* under CMS V28 hierarchy (both map to diabetes complications; HCC 37 dominates). Exclude via hierarchy rule.  
\- \*\*DX-205\*\* (HCC 280): source\_status \= Denied. Exclude.  
\- \*\*DX-206\*\* (HCC 37): benefit\_year \= 2024\. Wrong year. Exclude.  
\- \*\*DX-207\*\* (member\_id RA-9911): Different member. Exclude.  
\- \*\*DX-208\*\* (HCC 37): do\_not\_use \= Y. Exclude.  
\- \*\*PHM-301\*\* (pharmacy note, GLP1): record\_type \= pharmacy\_note, not a diagnosis. Non-HCC. Exclude.  
\- \*\*PRC-402\*\* (Bronze): metal\_level mismatch (enrolment is Silver). Also do\_not\_use \= Y. Exclude.

\*\*Payable HCC Set:\*\* 37, 226, 238

\*\*RAF Calculation\*\* (assuming Female, age 65–69 per market\_segment Community\_NonDual\_Aged and CMS V28 coefficients from the attached paper):

| Component | Coefficient | Reason |  
|-----------|-------------|--------|  
| Demographic (F, 65–69) | 0.3300 | CMS V28 base |  
| HCC 37 (Diabetes w/ complications) | 0.1660 | CMS V28 |  
| HCC 226 (Heart failure) | 0.3600 | CMS V28 |  
| HCC 238 (Atrial fibrillation) | 0.2990 | CMS V28 |  
| DM \+ CHF interaction | 0.1120 | CMS V28 (HCC 37 \+ 226\) |  
| CHF \+ AFIB interaction | 0.0770 | CMS V28 (HCC 226 \+ 238\) |  
| \*\*Full-Year RAF\*\* | \*\*1.3440\*\* | Sum |

\*\*Enrolment-Weighted RAF:\*\*  
1.3440 × (9 months ÷ 12\) \= \*\*1.0080\*\*

\*\*Adjusted Silver PMPM:\*\*  
612.845 × 1.0145 × 1.0365 \= \*\*644.4244\*\*

\*\*Transfer PMPM\*\* (full-year RAF × adjusted PMPM):  
1.3440 × 644.4244 \= \*\*866.1065\*\*

\*\*Gross Transfer Dollars\*\* (transfer PMPM × eligible months):  
866.1065 × 9 \= \*\*7,794.96\*\*

\*\*Reconciliation Note:\*\* The enrolment-weighted RAF (1.0080) is calculated for reporting but the full-year RAF (1.3440) drives the transfer PMPM calculation, reflecting the member's expected cost profile across a full benefit year despite partial-year enrolment. This is standard practice in ACA risk-adjustment transfer mechanics. 

**Health Actuarial — Multi-Member ACA Silver Transfer Reconciliation (Concise)**

## **1\. Metadata**

* **Task Type:** Workflow  
* **Category / Domain:** Actuarial / Health Pricing and Risk Adjustment  
* **Workflow:** Multi-Member Cross-Reconciled Transfer Audit  
* **Prompt Type:** Block-Level Payment Integrity Memorandum  
* **Difficulty:** Tier 4 — Expected 4.0-Pro-class Failure Rate ≥ 80%

## **2\. Prompt**

You are the Risk Adjustment Actuary. Date is 31 January 2026\.

A payment integrity audit has been opened on the 2025 ACA Silver block because the block-level transfer total does not reconcile to the raw extract. The attached file contains rows for **two members, RA-2047 and RA-2099**, mixed with enrollment, diagnosis, pricing, pharmacy, wrong-year, wrong-member, and do-not-use rows.

Using the attached CMS V28 coefficient paper and the extract, prepare a **Block-Level Payment Integrity Memorandum** that determines:

1. For **each member**: the payable 2025 RAF, the enrollment-weighted RAF, the applicable Silver pricing cell, the adjusted PMPM (base × AV × IDF), the transfer PMPM, and gross transfer dollars.  
2. The **block-level net transfer** (sum of both members' gross transfer dollars).  
3. A **reconciliation delta** between the block total and a naive calculation that would result from *not* applying the HCC hierarchy.

You must explain which rows you discarded and why, and you must apply HCC hierarchy correctly for both members. Cite record IDs. Carry arithmetic to four decimal places. Keep the memo tight.

*Note: RA-2099's data is intentionally sparser to force you to flag missing inputs rather than fabricate them.*

## **3\. Golden Response**

**BLOCK-LEVEL PAYMENT INTEGRITY MEMORANDUM** **TO:** 2025 Silver Block Audit File **FROM:** Risk Adjustment Actuary **DATE:** 31 January 2026 **RE:** Members RA-2047 and RA-2099 — 2025 ACA Silver Reconciliation

### **Member RA-2047**

**Controlling rows:** ENR-101 (9 eligible months, Silver), PRC-401 (base 612.845, AV 1.0145, IDF 1.0365). Payable diagnoses: DX-201 (HCC 37), DX-203 (HCC 226), DX-204 (HCC 238).

**Rejections:**

* DX-202 (HCC 38\) — suppressed by hierarchy; HCC 37 dominates HCC 38 (V28, Table 4).  
* DX-205 — status Denied.  
* DX-206 — benefit year 2024\.  
* DX-207 — different member (RA-9911).  
* DX-208 — do\_not\_use \= Y.  
* PHM-301 — pharmacy note, not a payable diagnosis.  
* PRC-402 — Bronze metal; does not match Silver enrollment; also do\_not\_use \= Y.

**Adjusted Silver PMPM** \= 612.845 × 1.0145 × 1.0365 \= **644.4244**.

**Full-year RAF** \= 0.330 (F 65–69) \+ 0.166 (HCC 37\) \+ 0.360 (HCC 226\) \+ 0.299 (HCC 238\) \+ 0.112 (DM+CHF) \+ 0.077 (CHF+AFIB) \= **1.3440**.

**Enrollment-weighted RAF** \= 1.3440 × 9/12 \= **1.0080**.

**Transfer PMPM** \= 1.3440 × 644.4244 \= **866.1064**.

**Gross transfer (RA-2047)** \= 866.1064 × 9 \= **$7,794.96**.

### **Member RA-2099**

**Observation:** The extract does not contain a controlling enrollment row, eligible-month count, pricing cell, or demographic detail for RA-2099. Only two fragment rows appear (one diagnosis and one pharmacy note).

**Determination:** Per audit protocol I decline to impute missing enrollment or pricing data. RA-2099's transfer cannot be reconciled from this extract. **Flag for data steward:** request the ENR-row, PRC-row, and demographic age/sex record for RA-2099 before the block can be closed. Do not fabricate a transfer amount.

### **Block-Level Net Transfer**

* RA-2047 gross transfer: **$7,794.96**  
* RA-2099 gross transfer: **Undetermined (insufficient data)**  
* **Block subtotal (reconcilable portion): $7,794.96**  
* **Block total cannot be finalized until RA-2099 data is supplied.**

### **Reconciliation Delta (Hierarchy Effect)**

A naive calculation that fails to suppress HCC 38 when HCC 37 is present would add the HCC 38 coefficient (≈0.106 under V28) to RA-2047's RAF:

* Naive RAF \= 1.3440 \+ 0.106 \= 1.4500  
* Naive transfer PMPM \= 1.4500 × 644.4244 \= 934.4154  
* Naive gross transfer \= 934.4154 × 9 \= $8,409.74  
* **Reconciliation delta (RA-2047 only):** $8,409.74 − $7,794.96 \= **$614.78 overstatement** if hierarchy not applied.

### **Conclusion**

The 2025 Silver block cannot be fully closed until RA-2099 data is supplied. The reconcilable portion (RA-2047) yields a correct gross transfer of **$7,794.96**. A hierarchy failure on RA-2047 alone would overstate the block by **$614.78**. Recommend: (a) block RA-2099 for data steward review; (b) confirm RA-2047 reconciliation; (c) re-run block close once ENR/PRC rows for RA-2099 are delivered.

