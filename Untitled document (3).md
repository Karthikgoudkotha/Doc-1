  
**\#\# Example 3: Pension Actuarial \- Hardened Funding Sufficiency with Boundary Rates, COLA Directive, Credit Balance, and Contribution Discounting**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Pension / Defined Benefit    
Workflow: Funding Sufficiency and Liability Measurement    
Prompt Type: Sponsor Funding Note    
Difficulty: Singularity Tier (Expected Failure Rate \>99%)

**\#\#\# 2\. Prompt**

You are an Enrolled Actuary. The current date is April 10, 2026\.

The sponsor of Plan PL-778 requires a funding note determining whether planned resource changes will carry the PPA funding target subset above a 101.75% funded ratio. The segment basis election is September 2025 and the valuation date is January 1, 2026\.

The attached cash-flow extract is a raw, unfiltered dump from multiple source systems. It contains duplicate extracts, preliminary estimates, stale prior-year rows, rate overrides, adjustment directives, funding credit balances, and rows from other plans or measurement bases. No column tells you which rows to include; you must determine inclusion from the data attributes.

Using only the appropriate data from the attached CSV and the applicable segment rates from the attached IRS notice, prepare a Sponsor Funding Note that:

(a) states whether the plan is above or below the 101.75% coverage threshold after all planned resource changes,  
(b) quantifies the dollar surplus or shortfall relative to 101.75%,  
(c) shows total available resources after all adjustments,  
(d) lists all cash flows included in the target subset and their individual discounted present values,  
(e) identifies the 2026 stabilized segment rates from the attached IRS notice and explains the stabilization logic, and  
(f) explains which CSV rows were excluded and categorizes by rejection reason.

Attached Files (Context):

\- IRS\_Notice\_2025\_47.pdf \- Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf  
\- pension\_pl778\_funding\_noise.csv  
\`\`\`csv  
record\_id,plan\_id,source\_extract,row\_type,measurement\_basis,status,valuation\_date,years\_from\_valuation,future\_benefit\_amount,asset\_market\_value,committed\_contribution,credit\_balance,contribution\_due\_date,cola\_factor,currency,admin\_code,comment  
AST-201,PL-778,treasury,asset\_market\_value,PPA\_Funding,Audited,2026-01-01,,,2760000.00,,,,,USD,A01,Audited market value as of valuation date  
RTE-001,PL-778,rate\_lookup,segment\_rate\_override,PPA\_Funding,Published,2026-01-01,,,,,,,,USD,R01,First=4.81% Second=5.35% Third=5.69%  
CF-103,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,12.30,1342780.50,,,,,,USD,B03,Deferred vested benefit  
CF-102P,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Preliminary,2026-01-01,5.00,731200.00,,,,,,USD,B02,Preliminary estimate superseded by final projection  
CFG-001,PL-778,ops\_dump,config\_row,NA,Ignore,2026-01-01,,,,,,,,USD,CFG,Non-financial system configuration record  
CF-107,PL-904,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,15.00,1105320.40,,,,,,USD,O01,Cross-plan data do not mix  
CF-101,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,2.80,495330.60,,,,,,USD,B01,Active participant short duration  
CF-108,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Settled,2026-01-01,6.90,543210.98,,,,,,USD,S01,Obligation already settled via lump sum  
CNT-301,PL-778,sponsor\_board,committed\_contribution,PPA\_Funding,Approved,2026-01-01,,,,285400.00,,2026-07-01,,USD,C01,Board-approved contribution scheduled July 1 2026  
CF-106,PL-778,valuation\_run,benefit\_cashflow,GAAP\_ASC715,Projected,2026-01-01,9.50,987654.32,,,,,,USD,G01,GAAP overlay not PPA funding basis  
CF-105,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,27.40,2215660.80,,,,,,USD,B05,Retiree long-tail obligation  
CF-102D,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,5.00,718450.25,,,,,,USD,B02,QC re-extract confirm inclusion  
CF-109,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2025-01-01,8.40,890000.00,,,,,,USD,B06,Prior year stale extract  
FCB-401,PL-778,trust\_accounting,funding\_credit\_balance,PPA\_Funding,Confirmed,2026-01-01,,,,,127830.00,,,USD,F01,Pre-funding balance per IRC 430(f) reduces FTAP assets  
CF-104,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,20.00,1788900.00,,,,,,USD,B04,Active benefit at second segment edge  
CF-110,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Rejected,2026-01-01,18.50,1567890.00,,,,,,USD,X01,Failed data quality review  
ADJ-501,PL-778,actuarial\_memo,cola\_adjustment,PPA\_Funding,Approved,2026-01-01,,,,,,,1.025,USD,ADJ,COLA 2.5% applies to benefit CFs with years\_from\_valuation exceeding 20  
CF-102,PL-778,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,5.00,718450.25,,,,,,USD,B02,Active benefit at first segment edge  
\`\`\`

**\#\#\# 3\. Model Analysis**

This prompt deploys 12 independent trap classes. A model that cleared the earlier version will now fail because the traps require actuarial rule knowledge rather than simple column filtering.

1\. **\*\*Rate override in CSV (RTE-001):\*\*** The CSV embeds a segment\_rate\_override row with Second=5.35%, which is the unadjusted IRS rate. The correct 2026 adjusted second segment rate is 5.25%. A model that trusts the CSV rates bypasses the IRS notice and gets all Seg2 PVs wrong.  
2\. **\*\*5-year segment boundary (CF-102 at 5.00yr):\*\*** IRC 430(h)(2) defines the first segment as benefits payable during the 5-year period beginning on the first day of the plan year. A cash flow at exactly 5.00 years falls within this period and uses the first segment rate (4.81%), not the second.  
3\. **\*\*20-year segment boundary (CF-104 at 20.00yr):\*\*** The second segment covers the 15-year period following the first 5 years (years \>5 to ≤20). A cash flow at exactly 20.00 years uses the second segment rate (5.25%), not the third.  
4\. **\*\*Duplicate row (CF-102D):\*\*** An exact financial duplicate of CF-102 labeled as a QC re-extract. Both rows have identical amounts, years, and status. Including both double-counts the liability and flips the pass/fail result.  
5\. **\*\*Preliminary row (CF-102P at $731,200):\*\*** Same maturity as CF-102 but with status=Preliminary and a higher amount. Must be excluded in favor of the final Projected version.  
6\. **\*\*Pre-funding credit balance (FCB-401):\*\*** Per IRC 430(f)(4)(B), a pre-funding balance of $127,830 must be subtracted from plan assets when computing the FTAP. Models that ignore or add the credit balance overstate resources by $128k-$256k.  
7\. **\*\*Contribution discounting (CNT-301):\*\*** The contribution is scheduled for July 1, 2026 — six months after the January 1 valuation date. It must be discounted at the first segment rate. Models that add it at face value overstate resources by \~$6,626.  
8\. **\*\*COLA adjustment directive (ADJ-501):\*\*** A COLA of 2.5% applies to benefit CFs with years\_from\_valuation *\*exceeding\** 20 (strictly greater than). Only CF-105 (27.40yr) qualifies. CF-104 (20.00yr) does not because 20 does not exceed 20\. Models may ignore the COLA entirely or apply it to both boundary and qualifying rows.  
9\. **\*\*Stale valuation date (CF-109):\*\*** This row has valuation\_date \= 2025-01-01, making it a carry-over from the prior year. Must be excluded.  
10\. **\*\*Rejected status (CF-110):\*\*** Status \= Rejected means this row failed QC and must be excluded.  
11\. **\*\*No include\_flag column:\*\*** Unlike the earlier version, no column directly flags inclusion. The model must infer inclusion from plan\_id, measurement\_basis, status, valuation\_date, and row\_type.  
12\. **\*\*Non-sequential task ordering:\*\*** The prompt asks for the conclusion (a, b) before the rate derivation (e) and cash flow detail (d).

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Uses the IRS notice to derive the 2026 stabilized segment rates as 4.81%, 5.25%, and 5.69% and does NOT use the CSV rate override row (RTE-001) which contains the unadjusted 5.35% second segment rate. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2; IRS\_Notice\_2025\_47.pdf, PDF Page 1; pension\_pl778\_funding\_noise.csv, record\_id RTE-001 | Page 2 gives adjusted September 2025 rates for 2026 plan years. Page 1 states the 95%-105% corridor and 5.00% floor. The CSV override carries the unadjusted rate and must be ignored. | FALSE | Used 5.35% from CSV instead of 5.25% from IRS notice, or used unadjusted rates from PDF. | None |  
| 2 | Keeps exactly CF-101, CF-102, CF-103, CF-104, and CF-105 as the five liability rows. Excludes CF-102D (duplicate), CF-102P (preliminary), CF-106 (GAAP), CF-107 (wrong plan), CF-108 (settled), CF-109 (stale valuation), and CF-110 (rejected). | Critical | pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103; CF-104; CF-105; CF-102D; CF-102P; CF-106; CF-107; CF-108; CF-109; CF-110 | These five rows are the only PL-778, PPA\_Funding, Projected, 2026-01-01 valuation date benefit cash flows. All other benefit rows fail on at least one attribute. | FALSE | Included CF-102D (double-counted), CF-102P (wrong amount), or any decoy row. | None |  
| 3 | Assigns CF-101 (2.80yr) and CF-102 (5.00yr) to the first segment (4.81%), CF-103 (12.30yr) and CF-104 (20.00yr) to the second segment (5.25%), and CF-105 (27.40yr) to the third segment (5.69%). | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2; IRC 430(h)(2) | First segment ≤5yr, second segment \>5yr–≤20yr, third segment \>20yr. The 5.00yr and 20.00yr boundaries are inclusive to the lower segment. | FALSE | Put CF-102 in Seg2 or CF-104 in Seg3 due to boundary confusion. | 1 |  
| 4 | Applies the COLA adjustment (×1.025) to CF-105 only and NOT to CF-104, because the directive says "years\_from\_valuation exceeding 20" and 20.00 does not exceed 20\. CF-105 after COLA \= 2,271,052.32. | Critical | pension\_pl778\_funding\_noise.csv, record\_id ADJ-501; CF-105; CF-104 | ADJ-501 states COLA applies to CFs with years \> 20 (not ≥ 20). CF-105 (27.40yr) qualifies; CF-104 (20.00yr) does not. | FALSE | Ignored COLA entirely, applied COLA to both CF-104 and CF-105, or misread the boundary condition. | 2 |  
| 5 | Computes individual PVs: PV(CF-101)=434,277.71, PV(CF-102)=568,045.46, PV(CF-103)=715,604.63, PV(CF-104)=642,900.81, PV(CF-105)=498,538.08. | Major | pension\_pl778\_funding\_noise.csv; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | PV \= FV / (1+r)^t with segment-specific rates and the COLA-adjusted CF-105 amount. | FALSE | Used wrong rate for any CF, omitted COLA on CF-105, or used the wrong exponent. | 1, 3, 4 |  
| 6 | Calculates the total present value of the target subset as 2,859,366.69. | Critical | pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103; CF-104; CF-105 | Sum of five unrounded PVs: 434,277.7104 \+ 568,045.4628 \+ 715,604.6275 \+ 642,900.8114 \+ 498,538.0803 \= 2,859,366.69. | FALSE | Included decoy rows, double-counted, or propagated a rounding or rate error. | 2, 5 |  
| 7 | Subtracts the pre-funding credit balance (FCB-401, $127,830.00) from plan assets per IRC 430(f) to get adjusted assets of 2,632,170.00. | Critical | pension\_pl778\_funding\_noise.csv, record\_id AST-201; FCB-401 | 2,760,000.00 − 127,830.00 \= 2,632,170.00. The credit balance reduces the asset base for FTAP computation. | FALSE | Ignored the credit balance, added it to assets, or treated it as a liability. | None |  
| 8 | Discounts the contribution ($285,400.00) to the valuation date at the first segment rate (4.81% for 0.50 years) to get PV \= 278,774.22, and computes total resources as 2,910,944.22. | Major | pension\_pl778\_funding\_noise.csv, record\_id CNT-301; AST-201; FCB-401 | PV(contrib) \= 285,400.00 / 1.0481^0.50 \= 278,774.22. Total \= 2,632,170.00 \+ 278,774.22 \= 2,910,944.22. | FALSE | Used contribution at face value ($285,400), omitted credit balance subtraction, or used wrong discount rate. | 7 |  
| 9 | Computes funded ratio as 101.8038%, the 101.75% threshold as 2,909,405.61, and surplus of 1,538.61 above threshold. Concludes the plan PASSES. | Critical | Prompt Text; pension\_pl778\_funding\_noise.csv; IRS\_Notice\_2025\_47.pdf | Ratio \= 2,910,944.22 / 2,859,366.69 \= 101.8038%. Required \= 1.0175 × 2,859,366.69 \= 2,909,405.61. Surplus \= 1,538.61. Any single trap error changes these numbers. | FALSE | Used wrong threshold (100% or 102%), or propagated liability/resource errors that flip the pass/fail result. | 6, 8 |  
| 10 | Formats as a dated Sponsor Funding Note (April 10, 2026\) and categorizes all excluded rows by rejection reason (duplicate, preliminary, GAAP, wrong plan, settled, stale valuation, rejected QC, config, rate override). | Minor | Prompt Text; pension\_pl778\_funding\_noise.csv | The deliverable must be a professional memo with rejection narrative covering all 8 excluded categories. | FALSE | Omitted rejection explanations, missed a category, or wrote a casual response. | None |

**\#\#\# 5\. Golden Response**

SPONSOR FUNDING NOTE    
TO: Plan Sponsor, PL-778    
FROM: Enrolled Actuary    
DATE: April 10, 2026    
SUBJECT: PPA Funding Sufficiency Review — 101.75% Coverage Threshold

1\. Segment Rate Basis

IRS Notice 2025-47 states that the adjusted September 2025 segment rates for plan years beginning in 2026 are 4.81% for the first segment, 5.25% for the second segment, and 5.69% for the third segment (IRS\_Notice\_2025\_47.pdf, PDF Page 2). The notice also states that the applicable corridor for 2024 through 2026 is 95% to 105% and that any 25-year average segment rate below 5.00% is deemed to be 5.00%; for plan years beginning in 2026 the 25-year averages are 3.22%, 5.00%, and 5.70% (IRS\_Notice\_2025\_47.pdf, PDF Page 1). The correct stabilized rates are 4.81%, 5.25%, and 5.69% (IRS\_Notice\_2025\_47.pdf, PDF Pages 1-2).

Important: The CSV contains a rate override row (RTE-001) with Second=5.35%, which is the unadjusted rate. This row is disregarded because the IRS notice is the authoritative source and the adjusted rate after applying the corridor and floor rules is 5.25%, not 5.35% (pension\_pl778\_funding\_noise.csv, record\_id RTE-001; IRS\_Notice\_2025\_47.pdf, PDF Page 2).

2\. Row Selection and Rejection

The five valid liability rows are CF-101, CF-102, CF-103, CF-104, and CF-105. Each is for plan PL-778, measurement\_basis \= PPA\_Funding, status \= Projected, and valuation\_date \= 2026-01-01 (pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103; CF-104; CF-105).

Excluded rows by category:

\- **\*\*Duplicate\*\***: CF-102D is a QC re-extract with identical financial content to CF-102 (same amount $718,450.25, same years 5.00). Counting both would double-count this obligation (pension\_pl778\_funding\_noise.csv, record\_id CF-102D).  
\- **\*\*Preliminary\*\***: CF-102P has status \= Preliminary and a different amount ($731,200.00) at the same 5.00-year maturity. The final Projected version (CF-102) supersedes it (pension\_pl778\_funding\_noise.csv, record\_id CF-102P).  
\- **\*\*Wrong basis\*\***: CF-106 uses GAAP\_ASC715, not PPA\_Funding (pension\_pl778\_funding\_noise.csv, record\_id CF-106).  
\- **\*\*Wrong plan\*\***: CF-107 belongs to PL-904 (pension\_pl778\_funding\_noise.csv, record\_id CF-107).  
\- **\*\*Settled\*\***: CF-108 has status \= Settled, meaning the obligation has been discharged (pension\_pl778\_funding\_noise.csv, record\_id CF-108).  
\- **\*\*Stale valuation\*\***: CF-109 has valuation\_date \= 2025-01-01, a carry-over from the prior year (pension\_pl778\_funding\_noise.csv, record\_id CF-109).  
\- **\*\*Rejected QC\*\***: CF-110 has status \= Rejected, indicating data quality failure (pension\_pl778\_funding\_noise.csv, record\_id CF-110).  
\- **\*\*Non-financial\*\***: CFG-001 is a system configuration row (pension\_pl778\_funding\_noise.csv, record\_id CFG-001).  
\- **\*\*Rate override\*\***: RTE-001 is a rate lookup row, not a financial exposure (pension\_pl778\_funding\_noise.csv, record\_id RTE-001).

The resource rows are AST-201 (audited assets), CNT-301 (approved contribution), and FCB-401 (pre-funding credit balance) (pension\_pl778\_funding\_noise.csv, record\_id AST-201; CNT-301; FCB-401).

3\. COLA Adjustment

The CSV contains a COLA directive (ADJ-501) instructing a 2.5% increase (factor 1.025) for benefit cash flows with years\_from\_valuation *\*exceeding\** 20\. "Exceeding 20" means strictly greater than 20 (pension\_pl778\_funding\_noise.csv, record\_id ADJ-501).

\- CF-105 (27.40 years): 27.40 \> 20 → COLA applies. Adjusted amount \= 2,215,660.80 × 1.025 \= 2,271,052.32 (pension\_pl778\_funding\_noise.csv, record\_id CF-105; ADJ-501).  
\- CF-104 (20.00 years): 20.00 is not \> 20 → COLA does NOT apply (pension\_pl778\_funding\_noise.csv, record\_id CF-104; ADJ-501).

4\. Segment Bucket Assignment and Present Values

Under IRC 430(h)(2), the first segment covers payments within the first 5 years (≤5yr), the second segment covers years \>5 through ≤20, and the third segment covers years \>20.

Formula: PV \= Future Amount / (1 \+ r)^t

CF-101: 2.80 years → First segment, rate \= 4.81%    
PV(CF-101) \= 495,330.60 / (1.0481^2.80) \= 434,277.71 (pension\_pl778\_funding\_noise.csv, record\_id CF-101; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

CF-102: 5.00 years → First segment (5.00 ≤ 5), rate \= 4.81%    
PV(CF-102) \= 718,450.25 / (1.0481^5.00) \= 568,045.46 (pension\_pl778\_funding\_noise.csv, record\_id CF-102; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

CF-103: 12.30 years → Second segment, rate \= 5.25%    
PV(CF-103) \= 1,342,780.50 / (1.0525^12.30) \= 715,604.63 (pension\_pl778\_funding\_noise.csv, record\_id CF-103; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

CF-104: 20.00 years → Second segment (20.00 ≤ 20), rate \= 5.25%    
PV(CF-104) \= 1,788,900.00 / (1.0525^20.00) \= 642,900.81 (pension\_pl778\_funding\_noise.csv, record\_id CF-104; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

CF-105: 27.40 years → Third segment, rate \= 5.69% (COLA-adjusted amount)    
PV(CF-105) \= 2,271,052.32 / (1.0569^27.40) \= 498,538.08 (pension\_pl778\_funding\_noise.csv, record\_id CF-105; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

Total Funding Target \= 434,277.7104 \+ 568,045.4628 \+ 715,604.6275 \+ 642,900.8114 \+ 498,538.0803 \= 2,859,366.69 (pension\_pl778\_funding\_noise.csv, record\_id CF-101; CF-102; CF-103; CF-104; CF-105)

5\. Available Resources

Audited market value of assets \= 2,760,000.00 (pension\_pl778\_funding\_noise.csv, record\_id AST-201).

Per IRC 430(f)(4)(B), the pre-funding balance must be subtracted from plan assets for FTAP computation:    
Adjusted Assets \= 2,760,000.00 − 127,830.00 \= 2,632,170.00 (pension\_pl778\_funding\_noise.csv, record\_id AST-201; FCB-401)

The approved contribution of $285,400.00 is scheduled for July 1, 2026, which is 0.50 years after the January 1, 2026 valuation date. It must be discounted to present value at the first segment rate:    
PV(Contribution) \= 285,400.00 / (1.0481^0.50) \= 278,774.22 (pension\_pl778\_funding\_noise.csv, record\_id CNT-301)

Total Available Resources \= 2,632,170.00 \+ 278,774.22 \= 2,910,944.22 (pension\_pl778\_funding\_noise.csv, record\_id AST-201; FCB-401; CNT-301)

6\. Coverage Test

Post-Contribution Funded Ratio \= 2,910,944.22 / 2,859,366.69 \= 1.018038 \= 101.8038% (pension\_pl778\_funding\_noise.csv; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

Required Assets at 101.75% \= 1.0175 × 2,859,366.69 \= 2,909,405.61 (Prompt Text; pension\_pl778\_funding\_noise.csv)

Surplus Above 101.75% \= 2,910,944.22 − 2,909,405.61 \= 1,538.61 (pension\_pl778\_funding\_noise.csv, record\_id AST-201; FCB-401; CNT-301; CF-101; CF-102; CF-103; CF-104; CF-105)

7\. Conclusion

Yes. After subtracting the pre-funding credit balance and discounting the scheduled contribution to the valuation date, Plan PL-778 exceeds the requested 101.75% threshold by $1,538.61. The post-contribution funded ratio is 101.8038%. Note that this margin is extremely thin — a difference of $1,539 against a $2.86 million liability base — and any adverse assumption change could erode it (IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl778\_funding\_noise.csv, record\_id AST-201; FCB-401; CNT-301; CF-101; CF-102; CF-103; CF-104; CF-105).

**\#\# Example 5: Pension Actuarial \- Multi-Bucket Liability Selection with Hidden Resource Rows**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Pension / Funding    
Workflow: Liability Measurement and Contribution Sufficiency    
Prompt Type: Funding Exception Memorandum    
Difficulty: Nightmare Tier (Expert Expected Failure Rate \>85%)

**\#\#\# 2\. Prompt**

You are an Enrolled Actuary. The current date is March 18, 2026\.

A pension committee needs a same-day exception memo for Plan PL-991. They do not trust the extraction layer because the attached CSV mixes valid and invalid rows, multiple plans, settled items, excluded items, and operational rows that resemble financial rows.

Without requesting additional instructions, use the attached IRS notice and the noisy extract to determine whether currently recognized resources are sufficient to clear a 101.25% funding threshold for the selected PPA subset as of the valuation date.

Your memo must include:

\- the applicable 2026 stabilized segment rates,  
\- identification of the exact included and excluded rows,  
\- present value calculations for each included liability row with the correct segment bucket,  
\- the total selected funding target,  
\- recognized resources (assets, approved contribution, and eligible hedge gain),  
\- the post-resource funded ratio,  
\- and the minimum additional contribution required to reach 101.25%.

Attached Files (Context):

\- IRS\_Notice\_2025\_47.pdf \- Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf  
\- pension\_pl991\_noise.csv \-  
\`\`\`csv  
record\_id,plan\_id,row\_type,measurement\_basis,include\_flag,status,years\_from\_valuation,future\_benefit\_amount,asset\_market\_value,approved\_contribution,eligible\_hedge\_gain,currency,source\_tag,noise\_metric,comment  
L-105,PL-991,benefit\_cashflow,GAAP,Y,Projected,11.20,622222.22,,,,USD,decoy,0.7781,Wrong basis  
A-901,PL-991,asset\_market\_value,PPA\_Funding,Y,Audited,,,1812440.66,,,USD,asset,0.4012,Recognized asset  
L-102,PL-991,benefit\_cashflow,PPA\_Funding,Y,Projected,13.60,1332450.42,,,,USD,liab,1.0981,Include  
O-100,PL-991,ops\_control,NA,N,Ignore,,,,,,USD,ops,0.1022,Operational row  
L-108,PL-991,benefit\_cashflow,PPA\_Funding,Y,Settled,5.90,288004.50,,,,USD,settled,0.9444,Settled row  
C-901,PL-991,approved\_contribution,PPA\_Funding,Y,Approved,,,,255120.34,,USD,contrib,0.6121,Recognized contribution  
L-101,PL-991,benefit\_cashflow,PPA\_Funding,Y,Projected,4.20,845321.77,,,,USD,liab,1.3311,Include  
H-701,PL-991,eligible\_hedge\_gain,PPA\_Funding,Y,Realized,,,,,18221.59,USD,hedge,0.2199,Recognized hedge gain  
L-106,PL-991,benefit\_cashflow,PPA\_Funding,N,Projected,7.80,455500.50,,,,USD,exclude,1.4566,Flagged exclude  
L-104,PL-991,benefit\_cashflow,PPA\_Funding,Y,Projected,24.40,990775.28,,,,USD,liab,0.9932,Include  
L-103,PL-991,benefit\_cashflow,PPA\_Funding,Y,Projected,19.75,1889455.91,,,,USD,liab,1.2234,Include  
L-107,PL-557,benefit\_cashflow,PPA\_Funding,Y,Projected,10.00,730100.00,,,,USD,otherplan,0.8865,Different plan  
\`\`\`  
**\#\#\# 3\. Model Analysis**

This prompt is designed to fail average models by combining row-selection traps with non-sequential funding tests:

1\. Models often pull the unadjusted second segment (5.35%) instead of the 2026 adjusted 5.25%.  
2\. Models frequently include GAAP rows or settled rows because they carry large values.  
3\. Models miss that resources are split across three row types: assets, approved contribution, and eligible hedge gain.  
4\. Models apply one rate to all rows rather than segmenting by maturity bucket.  
5\. Models compute a funded ratio correctly but fail the requested 101.25% threshold test and required incremental contribution.

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Extracts 2026 stabilized segment rates as 4.81%, 5.25%, and 5.69%. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2 | The adjusted September 2025 rates for plan years beginning in 2026 are explicitly listed. | FALSE | Used unadjusted 5.35% second segment. | None |  
| 2 | Selects only L-101, L-102, L-103, and L-104 as included liability rows. | Critical | pension\_pl991\_noise.csv, record\_id L-101; L-102; L-103; L-104 | These rows are PL-991, PPA\_Funding, include\_flag Y, and Projected. | FALSE | Included wrong-plan, settled, or GAAP rows. | None |  
| 3 | Includes A-901, C-901, and H-701 as recognized resources and excludes operational rows. | Major | pension\_pl991\_noise.csv, record\_id A-901; C-901; H-701; O-100 | Resources are intentionally split across row types to test extraction discipline. | FALSE | Dropped hedge gain or included operational row as cash. | None |  
| 4 | Discounts L-101 to 693,953.91 using first-segment rate. | Major | pension\_pl991\_noise.csv, record\_id L-101; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 845,321.77 / (1.0481^4.20) \= 693,953.91. | FALSE | Wrong segment mapping or wrong exponent. | 1, 2 |  
| 5 | Discounts L-102 to 664,401.24 and L-103 to 687,781.04 using second-segment rate. | Major | pension\_pl991\_noise.csv, record\_id L-102; L-103; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 1,332,450.42 / (1.0525^13.60) and 1,889,455.91 / (1.0525^19.75). | FALSE | Used 5.35% or mixed second/third segments incorrectly. | 1, 2 |  
| 6 | Discounts L-104 to 256,772.23 using third-segment rate. | Major | pension\_pl991\_noise.csv, record\_id L-104; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | 990,775.28 / (1.0569^24.40) \= 256,772.23. | FALSE | Used second segment for \>20-year maturity. | 1, 2 |  
| 7 | Calculates total selected funding target as 2,302,908.42. | Critical | pension\_pl991\_noise.csv, record\_id L-101; L-102; L-103; L-104 | Sum of discounted liabilities using unrounded intermediate values. | FALSE | Included decoy rows or rounded early. | 4, 5, 6 |  
| 8 | Calculates recognized resources as 2,085,782.59 and post-resource funded ratio as 90.5717%. | Critical | pension\_pl991\_noise.csv, record\_id A-901; C-901; H-701 | 1,812,440.66 \+ 255,120.34 \+ 18,221.59 \= 2,085,782.59; divide by funding target. | FALSE | Ignored one resource row. | 3, 7 |  
| 9 | Calculates 101.25% threshold amount of 2,331,694.77 and minimum additional contribution of 245,912.18. | Critical | Prompt Text; pension\_pl991\_noise.csv; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | Required extra \= 1.0125 x target \- (assets \+ contribution \+ hedge gain). | FALSE | Tested against 100% or 102% instead of 101.25%. | 7, 8 |  
| 10 | Delivers a dated Funding Exception Memorandum with explicit include/exclude row rationale. | Minor | Prompt Text; pension\_pl991\_noise.csv | Required deliverable style and defensibility of extraction logic. | FALSE | No row-selection rationale. | None |

**\#\#\# 5\. Golden Response**

FUNDING EXCEPTION MEMORANDUM    
TO: Pension Committee, Plan PL-991    
FROM: Enrolled Actuary    
DATE: March 18, 2026    
SUBJECT: 101.25% Funding Sufficiency Test for Selected PPA Subset

1\. Applicable Rate Basis

For plan years beginning in 2026, IRS Notice 2025-47 gives adjusted segment rates of 4.81%, 5.25%, and 5.69% for September 2025 (IRS\_Notice\_2025\_47.pdf, PDF Page 2). The notice also states the stabilization framework including the corridor and floor mechanics used to produce the adjusted rates (IRS\_Notice\_2025\_47.pdf, PDF Page 1).

2\. Liability Row Selection

Included liability rows are L-101, L-102, L-103, and L-104 because each is PL-991, measurement\_basis \= PPA\_Funding, include\_flag \= Y, and status \= Projected (pension\_pl991\_noise.csv, record\_id L-101; L-102; L-103; L-104).

Excluded rows are L-105 (GAAP), L-106 (include\_flag N), L-107 (different plan), and L-108 (Settled) (pension\_pl991\_noise.csv, record\_id L-105; L-106; L-107; L-108).

3\. Present Value of Included Liabilities

Formula: PV \= Future Benefit Amount / (1 \+ r)^t

PV(L-101) \= 845,321.77 / (1.0481^4.20) \= 693,953.91 (pension\_pl991\_noise.csv, record\_id L-101; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

PV(L-102) \= 1,332,450.42 / (1.0525^13.60) \= 664,401.24 (pension\_pl991\_noise.csv, record\_id L-102; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

PV(L-103) \= 1,889,455.91 / (1.0525^19.75) \= 687,781.04 (pension\_pl991\_noise.csv, record\_id L-103; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

PV(L-104) \= 990,775.28 / (1.0569^24.40) \= 256,772.23 (pension\_pl991\_noise.csv, record\_id L-104; IRS\_Notice\_2025\_47.pdf, PDF Page 2\)

Total Selected Funding Target \= 693,953.91 \+ 664,401.24 \+ 687,781.04 \+ 256,772.23 \= 2,302,908.42 (pension\_pl991\_noise.csv, record\_id L-101; L-102; L-103; L-104)

4\. Recognized Resources

Recognized resources are:

\- Audited asset market value A-901 \= 1,812,440.66  
\- Approved contribution C-901 \= 255,120.34  
\- Eligible hedge gain H-701 \= 18,221.59

(pension\_pl991\_noise.csv, record\_id A-901; C-901; H-701)

Total Recognized Resources \= 1,812,440.66 \+ 255,120.34 \+ 18,221.59 \= 2,085,782.59

Post-Resource Funded Ratio \= 2,085,782.59 / 2,302,908.42 \= 90.5717%

5\. 101.25% Threshold Test and Incremental Requirement

Required Resources at 101.25% \= 1.0125 x 2,302,908.42 \= 2,331,694.77

Minimum Additional Contribution Required \= 2,331,694.77 \- 2,085,782.59 \= 245,912.18

The committee request was to quantify the minimum contribution over current recognized resources to reach 101.25%. The required incremental amount is 245,912.18.

6\. Conclusion

Plan PL-991 does not currently satisfy the requested 101.25% threshold for the selected PPA subset. Post-resource funded status is 90.5717%, and an additional 245,912.18 is required to reach 101.25% (pension\_pl991\_noise.csv, record\_id A-901; C-901; H-701; L-101; L-102; L-103; L-104).

**\---**

**\#\# Example 6: Multiemployer Actuarial \- Treasury Permissible Range Enforcement with Non-Sequential Compliance Tests**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Pension / Multiemployer Funding    
Workflow: Discount-Rate Compliance and Liability Re-Measurement    
Prompt Type: Multiemployer Funding Compliance Memo    
Difficulty: Singularity Tier (Expected Failure Rate \>99%)

**\#\#\# 2\. Prompt**

You are a Pension Actuary supporting a multiemployer plan. The current date is February 26, 2026\.

The trustees provided a noisy valuation extract that includes cash-flow rows, rejected rows, non-financial rows, and multiple candidate discount-rate records. They also asked whether a proposed discount rate is compliant with the Treasury permissible range for plan years beginning in September 2025 before finalizing the funding communication.

Use the attached IRS notice and the noisy CSV to prepare a compliance memo that:

\- identifies the legally permissible discount-rate range,  
\- determines whether the proposed rate is compliant,  
\- applies the correct enforced rate,  
\- calculates liabilities under proposed and enforced rates,  
\- calculates funded ratios under both bases,  
\- and computes the additional contribution needed to achieve a 106.00% funded threshold under the enforced basis.

Do not assume the row order is meaningful.

Attached Files (Context):

\- IRS\_Notice\_2025\_47.pdf \- Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf  
\- multiemployer\_sep2025\_noise.csv  
\`\`\`csv  
record\_id,plan\_id,row\_type,include\_flag,status,years\_from\_valuation,benefit\_cashflow,discount\_rate,asset\_market\_value,currency,scenario\_code,non\_required\_ratio,comment  
CFG-001,ME-223,config\_row,N,Ignore,,,,,USD,CFG,0.1001,Non-financial row  
DR-103,ME-223,discount\_rate,Y,Expired,,,0.0410,,USD,DR\_EXP,0.2122,Old decoy rate  
CF-203,ME-223,benefit\_cashflow,Y,Projected,17.40,1288775.93,,,USD,CF,1.0088,Include  
AST-801,ME-223,asset\_market\_value,Y,Audited,,,,2015330.41,USD,AST,0.5511,Recognized assets  
CF-202,ME-223,benefit\_cashflow,Y,Projected,9.80,934220.47,,,USD,CF,0.9987,Include  
CF-206,ME-223,benefit\_cashflow,N,Projected,12.20,366600.60,,,USD,CFX,1.3890,Exclude flag  
DR-101,ME-223,discount\_rate,Y,Proposed,,,0.0457,,USD,DR\_PROP,0.4510,Proposed non-compliant rate  
CF-204,ME-223,benefit\_cashflow,Y,Projected,26.10,744110.66,,,USD,CF,0.9771,Include  
CF-205,ME-223,benefit\_cashflow,Y,Settled,4.80,201010.10,,,USD,CFS,1.2999,Settled row  
DR-102,ME-223,discount\_rate,Y,CapEnforced,,,0.0443,,USD,DR\_CAP,0.4602,Enforced max rate  
CF-201,ME-223,benefit\_cashflow,Y,Projected,3.25,512440.18,,,USD,CF,0.9210,Include  
CF-207,ME-998,benefit\_cashflow,Y,Projected,8.40,555000.00,,,USD,CFO,0.8011,Different plan  
\`\`\`

**\#\#\# 3\. Model Analysis**

This scenario is intentionally non-sequential and failure-prone:

1\. Models often ignore the permissible range and use the proposed rate even when out of range.  
2\. Models may pull a decoy rate row with the same plan\_id but wrong status.  
3\. Models commonly include non-cashflow rows in liabilities.  
4\. Models may perform the compliance test but forget to recompute liability and funded ratio on the enforced basis.  
5\. Models frequently calculate a threshold gap from proposed liability instead of enforced liability.

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Extracts the September 2025 Treasury weighted average 4.22% and permissible range 3.80% to 4.43%. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2 | The notice lists both the weighted average and the permissible range for plan years beginning in September 2025\. | FALSE | Used a decoy range from CSV notes. | None |  
| 2 | Identifies DR-101 (4.57%) as the proposed rate and DR-102 (4.43%) as the enforced cap record. | Critical | multiemployer\_sep2025\_noise.csv, record\_id DR-101; DR-102 | Proposed and enforced records are separated across row types and statuses. | FALSE | Used DR-103 or ignored status filters. | None |  
| 3 | Concludes 4.57% is non-compliant and enforces 4.43% as the capped rate. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2; multiemployer\_sep2025\_noise.csv, record\_id DR-101; DR-102 | 4.57% is above 4.43% maximum. | FALSE | Kept proposed rate unchanged. | 1, 2 |  
| 4 | Selects only CF-201, CF-202, CF-203, and CF-204 as liability cash-flow rows. | Major | multiemployer\_sep2025\_noise.csv, record\_id CF-201; CF-202; CF-203; CF-204 | These are include\_flag Y, status Projected, and row\_type benefit\_cashflow. | FALSE | Included rows flagged exclude or non-cashflow rows. | None |  
| 5 | Calculates proposed-rate liability as 1,870,123.54. | Major | multiemployer\_sep2025\_noise.csv, record\_id CF-201; CF-202; CF-203; CF-204; DR-101 | Present value at 4.57% across included cash flows. | FALSE | Arithmetic or row-selection error. | 2, 4 |  
| 6 | Calculates enforced-rate liability as 1,902,241.72 and liability increase of 32,118.18. | Critical | multiemployer\_sep2025\_noise.csv, record\_id CF-201; CF-202; CF-203; CF-204; DR-102 | Lower discount rate increases liability: 1,902,241.72 \- 1,870,123.54. | FALSE | Used wrong enforced rate or sign error. | 3, 4, 5 |  
| 7 | Uses AST-801 assets of 2,015,330.41 and calculates proposed funded ratio 107.7646%. | Major | multiemployer\_sep2025\_noise.csv, record\_id AST-801 | Funded ratio (proposed) \= assets / proposed liability. | FALSE | Used wrong asset row or wrong denominator. | 5 |  
| 8 | Calculates enforced funded ratio as 105.9450%. | Major | multiemployer\_sep2025\_noise.csv, record\_id AST-801 | Funded ratio (enforced) \= 2,015,330.41 / 1,902,241.72. | FALSE | Failed to recompute after enforcing cap. | 6, 7 |  
| 9 | Computes required assets at 106.00% as 2,016,376.22 and additional contribution need 1,045.81. | Critical | Prompt Text; multiemployer\_sep2025\_noise.csv, record\_id AST-801 | 1.06 x 1,902,241.72 \= 2,016,376.22; gap to assets \= 1,045.81. | FALSE | Applied 106% to proposed liability instead of enforced liability. | 6, 8 |  
| 10 | Delivers a dated compliance memo and explicitly states the non-compliant rate override logic. | Minor | Prompt Text; IRS\_Notice\_2025\_47.pdf, PDF Page 2 | Required professional memo and compliance narrative. | FALSE | Missing override rationale. | None |

**\#\#\# 5\. Golden Response**

MULTIEMPLOYER FUNDING COMPLIANCE MEMORANDUM    
TO: Board of Trustees    
FROM: Pension Actuary    
DATE: February 26, 2026    
SUBJECT: September 2025 Discount-Rate Compliance and Re-Measurement

1\. Permissible Range Rule

IRS Notice 2025-47 states that for plan years beginning in September 2025, the Treasury weighted average is 4.22% and the permissible range is 3.80% to 4.43% (IRS\_Notice\_2025\_47.pdf, PDF Page 2). Any selected funding discount rate must fall within that range.

2\. Rate Record Selection and Compliance Determination

From the noisy extract, DR-101 is the proposed rate record at 4.57%, while DR-102 is the compliance cap record at 4.43% (multiemployer\_sep2025\_noise.csv, record\_id DR-101; DR-102). Because 4.57% exceeds the 4.43% maximum, the proposed rate is non-compliant and must be overridden to 4.43% (IRS\_Notice\_2025\_47.pdf, PDF Page 2; multiemployer\_sep2025\_noise.csv, record\_id DR-101; DR-102).

3\. Liability Row Selection

Included liability rows are CF-201, CF-202, CF-203, and CF-204; excluded rows are those not projected benefit cash flows or with include\_flag N (multiemployer\_sep2025\_noise.csv, record\_id CF-201; CF-202; CF-203; CF-204).

4\. Liability Under Proposed and Enforced Rates

Using PV \= Sum\[CF\_t / (1 \+ r)^t\] over CF-201 through CF-204:

\- Liability at proposed 4.57% \= 1,870,123.54  
\- Liability at enforced 4.43% \= 1,902,241.72  
\- Liability increase from enforcement \= 1,902,241.72 \- 1,870,123.54 \= 32,118.18

(multiemployer\_sep2025\_noise.csv, record\_id CF-201; CF-202; CF-203; CF-204; DR-101; DR-102)

5\. Funded Ratios

Assets from AST-801 are 2,015,330.41 (multiemployer\_sep2025\_noise.csv, record\_id AST-801).

Proposed-basis funded ratio \= 2,015,330.41 / 1,870,123.54 \= 107.7646%

Enforced-basis funded ratio \= 2,015,330.41 / 1,902,241.72 \= 105.9450%

6\. 106.00% Threshold Requirement Under Enforced Basis

Required assets at 106.00% under enforced basis \= 1.06 x 1,902,241.72 \= 2,016,376.22

Additional contribution needed \= 2,016,376.22 \- 2,015,330.41 \= 1,045.81

7\. Conclusion

The proposed 4.57% discount rate is non-compliant with the September 2025 permissible range and must be capped to 4.43%. Under the enforced basis, funded status is 105.9450%, requiring an additional 1,045.81 to achieve the trustees' 106.00% target (IRS\_Notice\_2025\_47.pdf, PDF Page 2; multiemployer\_sep2025\_noise.csv, record\_id DR-101; DR-102; AST-801; CF-201; CF-202; CF-203; CF-204).

