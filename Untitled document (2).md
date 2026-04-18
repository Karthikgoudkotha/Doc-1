**\#\# Example 3: Pension Actuarial \- Noisy Funding Sufficiency and Segment Rate Selection**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Pension / Defined Benefit    
Workflow: Funding Sufficiency and Liability Measurement    
Prompt Type: Sponsor Funding Note    
Difficulty: Singularity Tier (Expected Failure Rate \>99%)

**\#\#\# 2\. Prompt**

You are an Enrolled Actuary. The current date is April 10, 2026\.

The sponsor of Plan PL-778 wants an immediate funding note on whether a scheduled contribution is enough to carry a selected subset of projected benefit cash flows above a 102% funded threshold on the PPA funding basis. The plan's segment basis election is September 2025\.

The attached cash-flow extract is intentionally noisy. It contains administrative rows, non-PPA rows, excluded rows, a different plan, and fields that are not relevant to the funding target. Use only the rows and columns that actually belong in the PPA funding measurement for PL-778. Do not assume the file is curated.

Without asking for more information, prepare a Sponsor Funding Note that:

\- identifies the correct 2026 stabilized segment rates from the attached IRS notice,  
\- isolates the benefit rows that belong in the target subset,  
\- discounts those cash flows to the valuation date,  
\- incorporates the audited market value of assets and the already approved contribution,  
\- states whether the sponsor reaches 102% coverage after the contribution, and  
\- quantifies the post-contribution funded ratio and the surplus or shortfall versus 102%.

Attached Files (Context):

\- IRS\_Notice\_2025\_47.pdf \- Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf  
\- pension\_pl778\_funding\_noise.csv  
\`\`\`csv  
record\_id,plan\_id,source\_extract,row\_type,measurement\_basis,include\_flag,status,years\_from\_valuation,future\_benefit\_amount,asset\_market\_value,committed\_contribution,currency,admin\_code,noise\_ratio,comment  
AST-201,PL-778,treasury\_feed,asset\_market\_value,PPA\_Funding,Y,Audited,,,1677420.18,,USD,A01,0.5001,Audited market value  
CF-104,PL-778,valuation\_run,benefit\_cashflow,GAAP,Y,Projected,8.25,555555.55,,,USD,G01,0.7712,Wrong measurement basis  
CF-102,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Y,Projected,11.40,1249380.80,,,USD,B02,0.8841,Included target subset  
CFG-001,PL-778,ops\_dump,config\_row,NA,N,Ignore,,,,,USD,CFG,0.1019,Non-financial control row  
CNT-301,PL-778,sponsor\_board,committed\_contribution,PPA\_Funding,Y,Approved,,,,239775.05,USD,C01,0.7504,Scheduled for 2026-04-30  
CF-106,PL-904,valuation\_run,benefit\_cashflow,PPA\_Funding,Y,Projected,9.40,913245.10,,,USD,O01,0.9231,Different plan  
CF-101,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Y,Projected,2.75,684215.55,,,USD,B01,1.4432,Included target subset  
CF-105,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,N,Projected,6.60,707070.70,,,USD,X01,1.6622,Flagged exclude  
CF-103,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Y,Projected,23.10,2035520.35,,,USD,B03,1.0027,Included target subset  
CF-107,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Y,Settled,4.10,315000.00,,,USD,S01,1.2001,Already settled  
\`\`\`

**\#\#\# 3\. Model Analysis**

This prompt is harder because it does not tell the model how to stabilize the rates or which rows in the CSV are usable. A weaker model will usually fail in one of five places:

1\. It will pull the unadjusted September 2025 row instead of the 2026 adjusted row from the IRS notice.  
2\. It will keep non-PPA or excluded rows because the largest amounts look economically relevant.  
3\. It will omit either the approved contribution or the asset row because they sit in different row types than the benefit cash flows.  
4\. It will discount every cash flow with the same rate instead of matching each cash flow to the proper segment bucket.  
5\. It will compare assets to 100% of liability and forget that the sponsor asked for a 102% threshold.

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Identifies the correct 2026 stabilized segment rates as 4.81%, 5.25%, and 5.69%. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2; IRS\_Notice\_2025\_47.pdf, PDF Page 1 | Page 2 gives the adjusted September 2025 rates for plan years beginning in 2026\. Page 1 states the 95%-105% corridor and the 5.00% floor rule. | FALSE | Used the unadjusted 5.35% second segment or the 2025 adjusted row. | None |  
| 2 | Keeps only CF-101, CF-102, and CF-103 as liability rows for the PPA funding subset. | Critical | pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103 | These are the only projected PL-778 benefit cash flows with PPA\_Funding basis and include\_flag Y. | FALSE | Included GAAP, excluded, settled, or wrong-plan rows. | None |  
| 3 | Uses AST-201 as assets and CNT-301 as the approved contribution while rejecting non-financial rows. | Major | pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301; CFG-001 | The asset and contribution sit in different row types and must be brought into the same funding note. | FALSE | Omitted the contribution or treated the config row as financial data. | None |  
| 4 | Discounts CF-101 to a present value of 601,292.04 using the first segment rate. | Major | pension\_pl778\_funding\_noise.csv, record\_id CF-101; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 684,215.55 / (1.0481^2.75) \= 601,292.04. | FALSE | Used the wrong rate bucket or wrong exponent. | 1, 2 |  
| 5 | Discounts CF-102 to a present value of 697,208.76 using the second segment rate. | Major | pension\_pl778\_funding\_noise.csv, record\_id CF-102; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 1,249,380.80 / (1.0525^11.40) \= 697,208.76. | FALSE | Used 5.35% or 5.69% instead of 5.25%. | 1, 2 |  
| 6 | Discounts CF-103 to a present value of 566,881.66 using the third segment rate. | Major | pension\_pl778\_funding\_noise.csv, record\_id CF-103; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 2,035,520.35 / (1.0569^23.10) \= 566,881.66. | FALSE | Used a single blended rate or the second segment. | 1, 2 |  
| 7 | Calculates the total target subset present value as 1,865,382.47. | Critical | pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103 | Sum of the three discounted cash flows using unrounded intermediate values. | FALSE | Summed rounded cells incorrectly or included a decoy row. | 4, 5, 6 |  
| 8 | Calculates post-contribution assets as 1,917,195.23 and the post-contribution funded ratio as 102.7776%. | Critical | pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301 | 1,677,420.18 \+ 239,775.05 \= 1,917,195.23; funded ratio \= 1,917,195.23 / 1,865,382.47. | FALSE | Ignored the contribution or divided by the wrong liability base. | 3, 7 |  
| 9 | Calculates the 102% coverage threshold as 1,902,690.12 and the excess above threshold as 14,505.11. | Critical | Prompt Text; pension\_pl778\_funding\_noise.csv; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 1.02 x 1,865,382.47 \= 1,902,690.12; 1,917,195.23 \- 1,902,690.12 \= 14,505.11. | FALSE | Benchmarked against 100% funded instead of 102%. | 7, 8 |  
| 10 | Formats the answer as a Sponsor Funding Note dated April 10, 2026 and explains why the rejected rows were excluded. | Minor | Prompt Text; pension\_pl778\_funding\_noise.csv | The deliverable requires a professional memo plus row-selection reasoning. | FALSE | Wrote a casual answer or did not explain row rejection. | None |

**\#\#\# 5\. Golden Response**

SPONSOR FUNDING NOTE    
TO: Plan Sponsor, PL-778    
FROM: Enrolled Actuary    
DATE: April 10, 2026    
SUBJECT: PPA Funding Sufficiency Review for Selected Cash-Flow Subset

1\. Segment Rate Basis

IRS Notice 2025-47 states that the adjusted September 2025 segment rates for plan years beginning in 2026 are 4.81% for the first segment, 5.25% for the second segment, and 5.69% for the third segment (IRS\_Notice\_2025\_47.pdf, PDF Page 2). The notice also states that the applicable corridor for 2024 through 2026 is 95% to 105% and that any 25-year average segment rate below 5.00% is deemed to be 5.00%; for plan years beginning in 2026 the 25-year averages are 3.22%, 5.00%, and 5.70% (IRS\_Notice\_2025\_47.pdf, PDF Page 1). Accordingly, the correct stabilized rates for this funding note are 4.81%, 5.25%, and 5.69% (IRS\_Notice\_2025\_47.pdf, PDF Pages 1-2).

2\. Row Selection

The liability rows that belong in the requested PPA funding subset are CF-101, CF-102, and CF-103 because each row is for plan PL-778, carries measurement\_basis \= PPA\_Funding, include\_flag \= Y, and status \= Projected (pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103). I excluded CF-104 because it is GAAP rather than PPA, CF-105 because include\_flag \= N, CF-106 because it belongs to PL-904, CF-107 because it is already settled, and CFG-001 because it is a control row rather than a financial exposure (pension\_pl778\_funding\_noise.csv, record\_id CF-104; CF-105; CF-106; CF-107; CFG-001). The relevant funding resources are AST-201 for audited assets and CNT-301 for the approved contribution (pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301).

3\. Present Value of Included Benefit Cash Flows

CF-101 matures in 2.75 years, so it falls in the first segment bucket and is discounted at 4.81% (IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl778\_funding\_noise.csv, record\_id CF-101).

Formula: PV \= Future Cash Flow / (1 \+ r)^t    
PV(CF-101) \= 684,215.55 / (1.0481^2.75) \= 601,292.04 (pension\_pl778\_funding\_noise.csv, record\_id CF-101)

CF-102 matures in 11.40 years, so it falls in the second segment bucket and is discounted at 5.25% (IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl778\_funding\_noise.csv, record\_id CF-102).

PV(CF-102) \= 1,249,380.80 / (1.0525^11.40) \= 697,208.76 (pension\_pl778\_funding\_noise.csv, record\_id CF-102)

CF-103 matures in 23.10 years, so it falls in the third segment bucket and is discounted at 5.69% (IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl778\_funding\_noise.csv, record\_id CF-103).

PV(CF-103) \= 2,035,520.35 / (1.0569^23.10) \= 566,881.66 (pension\_pl778\_funding\_noise.csv, record\_id CF-103)

Using unrounded intermediate values, the total present value of the selected subset is:

Total Funding Target \= 601,292.0438 \+ 697,208.7617 \+ 566,881.6604 \= 1,865,382.47 (pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103)

4\. Assets, Contribution, and Coverage Test

Audited market value of assets \= 1,677,420.18 (pension\_pl778\_funding\_noise.csv, record\_id AST-201).    
Approved contribution \= 239,775.05 (pension\_pl778\_funding\_noise.csv, record\_id CNT-301).

Post-Contribution Assets \= 1,677,420.18 \+ 239,775.05 \= 1,917,195.23 (pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301)

Post-Contribution Funded Ratio \= 1,917,195.23 / 1,865,382.47 \= 1.027776 \= 102.7776% (pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301)

The sponsor asked for a 102% coverage threshold, not merely full funding.

Required Assets at 102% \= 1.02 x 1,865,382.47 \= 1,902,690.12 (Prompt Text; pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103)

Excess Above 102% \= 1,917,195.23 \- 1,902,690.12 \= 14,505.11 (pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301)

5\. Conclusion

Yes. After recognizing the approved contribution, Plan PL-778 exceeds the requested 102% threshold for the selected PPA funding subset. The post-contribution funded ratio is 102.7776%, and the plan stands 14,505.11 above the requested threshold (IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301; CF-101; CF-102; CF-103).

**\---**

**\#\# Example 4: Health Actuarial \- Noisy ACA Risk Adjustment and Transfer Integrity**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Health Pricing and Risk Adjustment    
Workflow: Member-Level RAF and Transfer Integrity Review    
Prompt Type: Member Payment Integrity Memorandum    
Difficulty: Singularity Tier (Expected Failure Rate \>99%)

**\#\#\# 2\. Prompt**

You are a Risk Adjustment Actuary. The current date is January 31, 2026\.

A payment integrity review has been opened for member RA-2047 because the member-level transfer output for the 2025 ACA Silver block does not reconcile to the raw extract. The attached file is not curated. It mixes enrollment, diagnosis, pricing, pharmacy notes, wrong-year records, decoy members, and rows that are marked not to use.

Using the attached CMS V28 model paper and the attached member extract, prepare a Member Payment Integrity Memorandum that determines the member's payable 2025 RAF, the enrollment-weighted RAF, the applicable Silver transfer pricing cell, the member-level transfer PMPM, and the total gross transfer dollars for the member's eligible months. You must also explain which rows are discarded and why.

Attached Files (Context):

\- CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf \- Public URL: https://www.cms.gov/aco-reach-kcc-py2025-risk-adjust-paper  
\- aca\_ra2047\_member\_noise.csv  
\`\`\`csv  
record\_id,member\_id,benefit\_year,record\_type,source\_status,source\_system,diagnosis\_code,mapped\_hcc,eligible\_months,market\_segment,metal\_level,pricing\_cell,base\_transfer\_pmpm,av\_factor,induced\_demand\_factor,do\_not\_use,comment  
PRC-402,RA-2047,2025,pricing\_cell,Accepted,pricing,,,,Community\_NonDual\_Aged,Bronze,BRNZ-OMEGA,511.225,1.0060,1.0190,Y,Wrong metal level  
DX-203,RA-2047,2025,diagnosis,Accepted,claim,I50.9,HCC226,,,,,,,,N,Payable diagnosis  
ENR-101,RA-2047,2025,enrollment,Accepted,enrollment\_feed,,,9,Community\_NonDual\_Aged,Silver,SILV-ALPHA,,,,N,Controlling enrollment row  
DX-205,RA-2047,2025,diagnosis,Denied,claim,J44.9,HCC280,,,,,,,,Y,Denied diagnosis  
PHM-301,RA-2047,2025,pharmacy\_note,Accepted,rx,GLP1,,,,Community\_NonDual\_Aged,Silver,SILV-ALPHA,,,,Y,Non-HCC pharmacy note  
DX-201,RA-2047,2025,diagnosis,Accepted,claim,E11.69,HCC37,,,,,,,,N,Payable diagnosis  
PRC-401,RA-2047,2025,pricing\_cell,Accepted,pricing,,,,Community\_NonDual\_Aged,Silver,SILV-ALPHA,612.845,1.0145,1.0365,N,Correct pricing row  
DX-206,RA-2047,2024,diagnosis,Accepted,claim,E11.69,HCC37,,,,,,,,Y,Wrong benefit year  
DX-202,RA-2047,2025,diagnosis,Accepted,claim,E11.9,HCC38,,,,,,,,N,Subordinate diabetes code  
DX-204,RA-2047,2025,diagnosis,Accepted,claim,I48.91,HCC238,,,,,,,,N,Payable diagnosis  
DX-207,RA-9911,2025,diagnosis,Accepted,claim,I50.9,HCC226,,,,,,,,Y,Different member  
DX-208,RA-2047,2025,diagnosis,Accepted,vendor\_chart,E11.69,HCC37,,,,,,,,Y,Marked do not use  
\`\`\`

**\#\#\# 3\. Model Analysis**

This prompt is difficult because the model has to resolve hierarchy, interactions, month-weighting, and pricing-cell selection without being told the order of operations. Common failure modes are:

1\. Keeping HCC 38 even though HCC 37 is present.  
2\. Using denied, wrong-year, or do\_not\_use rows because they resemble the valid diagnoses.  
3\. Selecting the Bronze pricing row instead of the Silver pricing row that matches the enrollment record.  
4\. Computing the RAF correctly but forgetting to weight it to 9 eligible months.  
5\. Multiplying the weighted RAF by 9 months and then again by 12 months, which double-counts exposure.

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Selects ENR-101 as the controlling enrollment row and extracts 9 eligible months in the Community\_NonDual\_Aged Silver segment. | Critical | aca\_ra2047\_member\_noise.csv, record\_id ENR-101 | The enrollment row defines months, market segment, and metal level. | FALSE | Used the wrong member or the wrong metal segment. | None |  
| 2 | Selects PRC-401 as the valid pricing row and calculates the adjusted Silver pricing cell as 644.4244 PMPM. | Critical | aca\_ra2047\_member\_noise.csv, record\_id PRC-401 | 612.845 x 1.0145 x 1.0365 \= 644.4244. | FALSE | Used the Bronze row or the raw base PMPM without factors. | 1 |  
| 3 | Extracts the demographic coefficient for Female 65-69 as 0.330. | Major | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12 | Table 12 provides the demographic coefficient. | FALSE | Used the wrong age-sex cell. | None |  
| 4 | Applies hierarchy correctly by retaining HCC 37 and dropping HCC 38\. | Critical | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 4; aca\_ra2047\_member\_noise.csv, record\_id DX-201; DX-202 | HCC 37 supersedes the subordinate diabetes code HCC 38\. | FALSE | Counted both diabetes HCCs. | None |  
| 5 | Extracts and keeps HCC 226 \= 0.360 and HCC 238 \= 0.299. | Major | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; aca\_ra2047\_member\_noise.csv, record\_id DX-203; DX-204 | These are the surviving cardiovascular HCCs. | FALSE | Dropped a payable diagnosis or used a wrong coefficient. | None |  
| 6 | Applies the Diabetes \+ CHF interaction factor of 0.112. | Critical | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, interaction table/section | The coexistence of HCC 37 and HCC 226 triggers this additive interaction. | FALSE | Ignored the interaction term. | 4, 5 |  
| 7 | Applies the CHF \+ AFIB interaction factor of 0.077. | Critical | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, interaction table/section | The coexistence of HCC 226 and HCC 238 triggers this additive interaction. | FALSE | Ignored the second interaction term. | 5 |  
| 8 | Calculates the full-year payable RAF as 1.344. | Critical | CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; interaction table/section | 0.330 \+ 0.166 \+ 0.360 \+ 0.299 \+ 0.112 \+ 0.077 \= 1.344. | FALSE | Included HCC 38 or missed one interaction. | 3, 4, 5, 6, 7 |  
| 9 | Calculates the enrollment-weighted RAF as 1.008 and the member transfer PMPM as 866.1065. | Critical | aca\_ra2047\_member\_noise.csv, record\_id ENR-101; PRC-401 | Weighted RAF \= 1.344 x 9/12 \= 1.008; transfer PMPM \= 1.344 x 644.4244 \= 866.1065. | FALSE | Failed to month-weight or multiplied by the wrong pricing cell. | 1, 2, 8 |  
| 10 | Calculates total gross transfer dollars as 7,794.96 and formats a dated memorandum that explains row rejection. | Major | aca\_ra2047\_member\_noise.csv, record\_id ENR-101; PRC-401; DX-205; DX-206; DX-207; DX-208; PHM-301; PRC-402 | 866.1065 x 9 \= 7,794.96. The memorandum must explain why denied, wrong-year, decoy, pharmacy, and Bronze rows are excluded. | FALSE | Used 12 months, retained invalid rows, or omitted the rejection narrative. | 1, 2, 8, 9 |

**\#\#\# 5\. Golden Response**

MEMBER PAYMENT INTEGRITY MEMORANDUM    
TO: 2025 Silver Risk Adjustment Review File    
FROM: Risk Adjustment Actuary    
DATE: January 31, 2026    
SUBJECT: Member RA-2047 RAF and Transfer Reconciliation

1\. Controlling Enrollment and Pricing Rows

The controlling enrollment row for the member is ENR-101. It shows member RA-2047 in benefit year 2025, with 9 eligible months, market segment Community\_NonDual\_Aged, and metal level Silver (aca\_ra2047\_member\_noise.csv, record\_id ENR-101). The matching pricing row is PRC-401 because it is the Silver pricing cell for the same segment and benefit year (aca\_ra2047\_member\_noise.csv, record\_id PRC-401). PRC-402 is rejected because it is a Bronze row and does not match the enrollment metal level (aca\_ra2047\_member\_noise.csv, record\_id PRC-402).

Adjusted Silver Pricing Cell PMPM \= Base Transfer PMPM x AV Factor x Induced Demand Factor    
\= 612.845 x 1.0145 x 1.0365    
\= 644.4244 PMPM, rounded (aca\_ra2047\_member\_noise.csv, record\_id PRC-401)

2\. Payable Diagnosis Set and Row Rejections

The valid payable diagnosis rows are DX-201, DX-202, DX-203, and DX-204 because they belong to RA-2047, benefit year 2025, and carry Accepted status without a do\_not\_use flag (aca\_ra2047\_member\_noise.csv, record\_id DX-201; DX-202; DX-203; DX-204).

I reject DX-205 because it is Denied, DX-206 because it belongs to benefit year 2024, DX-207 because it belongs to another member, DX-208 because it is explicitly marked do\_not\_use \= Y, and PHM-301 because it is a pharmacy note rather than a payable diagnosis row with a mapped HCC (aca\_ra2047\_member\_noise.csv, record\_id DX-205; DX-206; DX-207; DX-208; PHM-301).

3\. Hierarchy, Coefficients, and Interaction Logic

The demographic coefficient for a Female age 65-69 is 0.330 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12). The surviving HCC coefficients are:

\- HCC 37 \= 0.166 from DX-201 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; aca\_ra2047\_member\_noise.csv, record\_id DX-201)  
\- HCC 226 \= 0.360 from DX-203 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; aca\_ra2047\_member\_noise.csv, record\_id DX-203)  
\- HCC 238 \= 0.299 from DX-204 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; aca\_ra2047\_member\_noise.csv, record\_id DX-204)

DX-202 maps to HCC 38, but HCC 37 supersedes HCC 38 under the hierarchy, so HCC 38 is dropped and does not contribute to payment RAF (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 4; aca\_ra2047\_member\_noise.csv, record\_id DX-201; DX-202).

The member also triggers two additive interaction terms:

\- Diabetes \+ CHF interaction \= 0.112 because HCC 37 and HCC 226 are both present (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, interaction table/section)  
\- CHF \+ AFIB interaction \= 0.077 because HCC 226 and HCC 238 are both present (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, interaction table/section)

4\. RAF Calculation

Full-Year Payable RAF \= Demographic \+ HCC 37 \+ HCC 226 \+ HCC 238 \+ DM/CHF Interaction \+ CHF/AFIB Interaction    
\= 0.330 \+ 0.166 \+ 0.360 \+ 0.299 \+ 0.112 \+ 0.077    
\= 1.344 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; Table 4; interaction table/section)

Because the member has 9 eligible months rather than 12, the enrollment-weighted RAF is:

Enrollment-Weighted RAF \= 1.344 x (9 / 12\) \= 1.008 (aca\_ra2047\_member\_noise.csv, record\_id ENR-101)

5\. Transfer PMPM and Gross Transfer Dollars

Transfer PMPM \= Full-Year RAF x Adjusted Silver Pricing Cell PMPM    
\= 1.344 x 644.4244    
\= 866.1065 PMPM, rounded (aca\_ra2047\_member\_noise.csv, record\_id PRC-401)

Gross Transfer Dollars for Eligible Months \= 866.1065 x 9    
\= 7,794.96, rounded (aca\_ra2047\_member\_noise.csv, record\_id ENR-101; PRC-401)

6\. Conclusion

The correct payable RAF for member RA-2047 is 1.344, the enrollment-weighted RAF is 1.008, the applicable adjusted Silver pricing cell is 644.4244 PMPM, the member transfer PMPM is 866.1065, and the total gross transfer amount for the 9 eligible months is 7,794.96 (CMS\_V28\_Risk\_Adjustment\_Guidelines.pdf, Table 12; Table 4; interaction table/section; aca\_ra2047\_member\_noise.csv, record\_id ENR-101; PRC-401; DX-201; DX-203; DX-204).

