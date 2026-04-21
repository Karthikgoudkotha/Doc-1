**\#\# Example 9: Pension Actuarial \- Source-Linked Dual-Scenario Coverage with Extreme Noise**

**\#\#\# 1\. Metadata**

Task Type: Workflow    
Category / Domain: Actuarial / Pension / Defined Benefit    
Workflow: Dual-Scenario Funding Sufficiency and Incremental Contribution Need    
Prompt Type: Sponsor Funding Note    
Difficulty: Singularity Tier (Expected Failure Rate \>99%)

**\#\#\# 2\. Prompt**

You are an Enrolled Actuary. The current date is April 18, 2026\.

Prepare a Sponsor Funding Note for Plan PL-1302 using only valid rows from the attached extract and rules/rates extracted from the linked source document.

Important prompt constraints:

\- extract the discount-rate basis from the source link, not from embedded CSV override rows,  
\- derive row inclusion/exclusion logic from row attributes (the file is not curated),  
\- process all tasks below even if intermediate outputs appear inconsistent,  
\- present final outputs in the same sequence listed below.

Required outputs (in this exact order):

1\. stress-scenario funded ratio after all liability and resource adjustments,  
2\. base-scenario funded ratio after all liability and resource adjustments,  
3\. stress-scenario required asset amount at 105.00%,  
4\. base-scenario required asset amount at 103.25%,  
5\. final incremental contribution required to satisfy both thresholds simultaneously,  
6\. list of included liability row IDs,  
7\. categorized rejected rows by reason.

Attached Files (Context):

\- IRS\_Notice\_2025\_47.pdf \- Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf  
\- pension\_pl1302\_extreme\_noise.csv  
\`\`\`csv  
record\_id,plan\_id,source\_extract,row\_type,measurement\_basis,status,valuation\_date,years\_from\_valuation,future\_benefit\_amount,asset\_market\_value,committed\_contribution,credit\_balance,contribution\_due\_date,loc\_amount,recognition\_factor,rate\_shock\_bps,expense\_load\_pct,mortality\_load\_factor,pending\_payment\_amount,currency,admin\_code,comment  
AST-501,PL-1302,treasury\_feed,asset\_market\_value,PPA\_Funding,Audited,2026-01-01,,,3562300.00,,,,,,,,,,USD,A11,Audited market value at valuation date  
CF-401,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,3.25,622450.70,,,,,,,,,,,USD,B11,Projected short duration benefit  
CF-402,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,7.75,980120.40,,,,,,,,,,,USD,B12,Projected mid duration benefit  
CF-403,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,15.00,1415050.55,,,,,,,,,,,USD,B13,Projected boundary benefit second segment start edge  
CF-404,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,20.00,1888420.10,,,,,,,,,,,USD,B14,Projected boundary benefit second segment end edge  
CF-405,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,21.40,2057755.80,,,,,,,,,,,USD,B15,Projected long duration benefit  
CF-406,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,29.80,2644400.95,,,,,,,,,,,USD,B16,Projected ultra long duration benefit  
CF-402D,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,7.75,980120.40,,,,,,,,,,,USD,B12,QC duplicate extraction row  
CF-403P,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Preliminary,2026-01-01,15.00,1477000.00,,,,,,,,,,,USD,B13,Preliminary estimate superseded by final projection  
CF-407,PL-8820,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2026-01-01,11.20,910000.00,,,,,,,,,,,USD,O11,Different plan do not include  
CF-408,PL-1302,valuation\_run,benefit\_cashflow,GAAP\_ASC715,Projected,2026-01-01,9.40,1000550.00,,,,,,,,,,,USD,G11,Wrong measurement basis  
CF-409,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Settled,2026-01-01,6.10,744000.00,,,,,,,,,,,USD,S11,Settled obligation  
CF-410,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Rejected,2026-01-01,18.50,1620050.00,,,,,,,,,,,USD,X11,Rejected by data quality  
CF-411,PL-1302,valuation\_run,benefit\_cashflow,PPA\_Funding,Projected,2025-01-01,8.30,860000.00,,,,,,,,,,,USD,B17,Stale prior-year valuation row  
CNT-501,PL-1302,sponsor\_board,committed\_contribution,PPA\_Funding,Approved,2026-01-01,,,,420000.00,,2026-10-01,,,,,,USD,C11,Approved contribution tranche 1  
CNT-502,PL-1302,sponsor\_board,committed\_contribution,PPA\_Funding,Approved,2026-01-01,,,,180000.00,,2027-01-01,,,,,,USD,C12,Approved contribution tranche 2  
FCB-501,PL-1302,trust\_accounting,funding\_credit\_balance,PPA\_Funding,Confirmed,2026-01-01,,,,,210450.00,,,,,,,,USD,F11,Pre-funding balance reduces FTAP assets  
LOC-501,PL-1302,treasury\_feed,letter\_of\_credit,PPA\_Funding,Active,2026-01-01,,,,,,,350000.00,,,,,,,USD,L11,Letter of credit resource component  
REC-501,PL-1302,treasury\_feed,recognition\_factor,PPA\_Funding,Active,2026-01-01,,,,,,,,0.88,,,,,,USD,R11,Recognition factor applies to LOC amount  
PAY-501,PL-1302,operations,pending\_benefit\_payment,PPA\_Funding,Approved,2026-01-01,,,,,,,,,,,,95600.00,USD,P11,Pending payment reduces net resources  
ADJ-701,PL-1302,actuarial\_memo,mortality\_load,PPA\_Funding,Approved,2026-01-01,,,,,,,,,,,1.018,,USD,M11,Mortality load applies only to projected benefit rows with years\_from\_valuation strictly greater than 20  
ADJ-702,PL-1302,actuarial\_memo,expense\_load,PPA\_Funding,Approved,2026-01-01,,,,,,,,,,0.0135,,,,USD,E11,Expense load applies to discounted liability subtotal after mortality load  
SHK-501,PL-1302,actuarial\_memo,rate\_shock,PPA\_Funding,Approved,2026-01-01,,,,,,,,,-35,,,,USD,K11,Stress scenario uses uniform parallel shock in bps to segment discount rates  
RTE-501,PL-1302,rate\_lookup,segment\_rate\_override,PPA\_Funding,Published,2026-01-01,,,,,,,,,,,,,,USD,R12,Embedded row says second segment 5.35 unadjusted do not use for 2026 adjusted basis  
CFG-501,PL-1302,ops\_dump,config\_row,NA,Ignore,2026-01-01,,,,,,,,,,,,,,USD,CFG,Non-financial configuration row  
\`\`\`

**\#\#\# 3\. Model Analysis**

This design is intended to force extraction discipline from source links and multi-phase arithmetic consistency. Typical failure modes are:

1\. Using the embedded CSV override rate row (RTE-501) instead of the 2026 adjusted IRS segment rates.  
2\. Keeping CF-402D and double counting a valid liability.  
3\. Keeping CF-403P (preliminary) instead of CF-403 (projected final).  
4\. Mishandling segment boundaries at exactly 15.00 and 20.00 years.  
5\. Applying mortality load to CF-404 (20.00) even though the directive is strictly greater than 20\.  
6\. Applying expense load before discounting instead of after discounted-liability subtotal.  
7\. Treating contribution tranches at face value instead of valuation-date present values.  
8\. Ignoring the funding credit balance reduction.  
9\. Taking full LOC amount without recognition factor.  
10\. Computing incremental contribution against only one threshold instead of both thresholds jointly.

**\#\#\# 4\. Rubric**

| \# | Description | Weight | Sources | Justification | Met | Failure Reasoning | Dependent Criteria |  
| \--- | \--- | \--- | \--- | \--- | \--- | \--- | \--- |  
| 1 | Uses IRS Notice 2025-47 adjusted 2026 segment rates (4.81%, 5.25%, 5.69%) and ignores RTE-501 override row with unadjusted second segment 5.35%. | Critical | IRS\_Notice\_2025\_47.pdf, PDF Page 2; pension\_pl1302\_extreme\_noise.csv, record\_id RTE-501 | Source-link extraction is mandatory; CSV override is a decoy. | FALSE | Pulled rates from CSV override or unadjusted notice row. | None |  
| 2 | Keeps exactly CF-401, CF-402, CF-403, CF-404, CF-405, CF-406 as liability rows. | Critical | pension\_pl1302\_extreme\_noise.csv, record\_id CF-401; CF-402; CF-403; CF-404; CF-405; CF-406 | These are PL-1302, PPA\_Funding, Projected, valuation\_date 2026-01-01 and non-duplicate rows. | FALSE | Included duplicate/preliminary/wrong-basis/stale/settled/rejected/other-plan rows. | None |  
| 3 | Applies mortality load factor 1.018 only to CF-405 and CF-406 because ADJ-701 says years\_from\_valuation strictly greater than 20\. | Critical | pension\_pl1302\_extreme\_noise.csv, record\_id ADJ-701; CF-404; CF-405; CF-406 | CF-404 at 20.00 does not qualify; CF-405 and CF-406 do. | FALSE | Applied mortality load to CF-404 or skipped load on CF-405/CF-406. | 2 |  
| 4 | Computes discounted base liability subtotal (before expense load) as 3,687,437.34. | Major | pension\_pl1302\_extreme\_noise.csv, record\_id CF-401; CF-402; CF-403; CF-404; CF-405; CF-406; ADJ-701 | Sum of six discounted CFs using proper buckets and mortality-adjusted long-tail rows. | FALSE | Bucket or adjustment mistakes altered subtotal. | 1, 2, 3 |  
| 5 | Applies expense load 1.35% after discounted subtotal to get base liability 3,737,217.74. | Major | pension\_pl1302\_extreme\_noise.csv, record\_id ADJ-702 | Base liability \= 3,687,437.34 x 1.0135 \= 3,737,217.74. | FALSE | Applied expense load in wrong stage or wrong direction. | 4 |  
| 6 | Applies stress shock of \-35 bps from SHK-501 to segment rates and computes stress liability (after expense load) as 3,944,373.80. | Critical | pension\_pl1302\_extreme\_noise.csv, record\_id SHK-501; ADJ-702 | Stress rates are parallel shift: Seg1=4.46%, Seg2=4.90%, Seg3=5.34%; then expense load applies. | FALSE | Applied absolute 35% shock, reversed sign, or ignored expense load in stress run. | 1, 2, 3 |  
| 7 | Computes net resources as 4,141,448.62 using asset, credit balance, discounted contributions, recognized LOC, and pending payment. | Critical | pension\_pl1302\_extreme\_noise.csv, record\_id AST-501; FCB-501; CNT-501; CNT-502; LOC-501; REC-501; PAY-501 | Resources \= (3,562,300.00 \- 210,450.00) \+ PV(420,000 at 0.75y) \+ PV(180,000 at 1.00y) \+ (350,000 x 0.88) \- 95,600 \= 4,141,448.62. | FALSE | Omitted discounting, ignored credit balance, or used full LOC. | 1 |  
| 8 | Computes stress funded ratio \= 104.9964% and base funded ratio \= 110.8164%. | Critical | pension\_pl1302\_extreme\_noise.csv; IRS\_Notice\_2025\_47.pdf | Stress ratio \= 4,141,448.62 / 3,944,373.80; base ratio \= 4,141,448.62 / 3,737,217.74. | FALSE | Swapped numerators/denominators or used inconsistent liabilities/resources. | 5, 6, 7 |  
| 9 | Computes required assets at 105.00% stress \= 4,141,592.49 and at 103.25% base \= 3,858,677.32; final incremental contribution need \= 143.87 (max of two deficits). | Critical | Prompt Text; pension\_pl1302\_extreme\_noise.csv | Additional requirement must satisfy both thresholds simultaneously, so the binding deficit is stress-side 143.87. | FALSE | Checked only one threshold or used min instead of max deficit logic. | 5, 6, 7, 8 |  
| 10 | Presents outputs in the requested non-sequential order and includes row-rejection categories. | Minor | Prompt Text; pension\_pl1302\_extreme\_noise.csv | Prompt explicitly enforces output order and rejection taxonomy. | FALSE | Reordered outputs or omitted rejection categories. | None |

**\#\#\# 5\. Golden Response**

SPONSOR FUNDING NOTE    
TO: Plan Sponsor, PL-1302    
FROM: Enrolled Actuary    
DATE: April 18, 2026    
SUBJECT: Dual-Scenario Funding Sufficiency and Incremental Contribution Requirement

1\. Stress-Scenario Funded Ratio

Stress funded ratio \= 4,141,448.62 / 3,944,373.80 \= 1.049964 \= 104.9964%.

2\. Base-Scenario Funded Ratio

Base funded ratio \= 4,141,448.62 / 3,737,217.74 \= 1.108164 \= 110.8164%.

3\. Stress Required Asset Amount at 105.00%

Required stress assets \= 1.05 x 3,944,373.80 \= 4,141,592.49.

4\. Base Required Asset Amount at 103.25%

Required base assets \= 1.0325 x 3,737,217.74 \= 3,858,677.32.

5\. Final Incremental Contribution Required to Satisfy Both Thresholds

Base deficit \= 3,858,677.32 \- 4,141,448.62 \= \-282,771.31 (no deficit).    
Stress deficit \= 4,141,592.49 \- 4,141,448.62 \= 143.87 (binding deficit).

Final incremental contribution required \= max(base deficit, stress deficit, 0\) \= 143.87.

6\. Included Liability Row IDs

CF-401, CF-402, CF-403, CF-404, CF-405, CF-406.

Calculation support:

\- Source-extracted 2026 adjusted rates from IRS Notice 2025-47: Seg1 4.81%, Seg2 5.25%, Seg3 5.69% (PDF Page 2).  
\- Mortality load ADJ-701 applies only where years\_from\_valuation \> 20, so CF-405 and CF-406 are multiplied by 1.018; CF-404 at 20.00 is not loaded.  
\- Base discounted-liability subtotal (post mortality-load, pre expense-load) \= 3,687,437.34.  
\- Expense load ADJ-702 applied after discounting: 3,687,437.34 x 1.0135 \= 3,737,217.74 base liability.  
\- Stress scenario applies SHK-501 parallel \-35 bps to segment rates and then ADJ-702 expense load, producing 3,944,373.80 stress liability.  
\- Resources computed as:  
    \- adjusted assets: 3,562,300.00 \- 210,450.00 \= 3,351,850.00,  
    \- PV contribution tranche 1: 420,000.00 / (1.0481^0.75) \= 405,459.28,  
    \- PV contribution tranche 2: 180,000.00 / (1.0481^1.00) \= 171,739.34,  
    \- recognized LOC: 350,000.00 x 0.88 \= 308,000.00,  
    \- less pending payment: \-95,600.00,  
    \- total resources: 4,141,448.62.

7\. Rejected Rows by Reason

\- Duplicate: CF-402D.  
\- Preliminary superseded row: CF-403P.  
\- Different plan: CF-407.  
\- Wrong measurement basis: CF-408.  
\- Settled: CF-409.  
\- Rejected by quality control: CF-410.  
\- Stale valuation date (2025 carryover): CF-411.  
\- Non-liability operational/config rows excluded from liability set but used by role where applicable: RTE-501, CFG-501, ADJ-701, ADJ-702, SHK-501, AST-501, CNT-501, CNT-502, FCB-501, LOC-501, REC-501, PAY-501.