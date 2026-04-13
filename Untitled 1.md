### Example 1: Reinsurance Interlocking Clause (Prorated Retention & Decoy CSV Trap)

**1. Metadata**
*   Task Type: Workflow
*   Category / Domain: Reinsurance / Actuarial
*   Workflow: Treaty Adjudication & Quantum Calculation
*   Prompt Type: Actuarial Adjustment Memo
*   Difficulty: Super Hard (Expert)

**2. Prompt**
You are a Reinsurance Claims Actuary. The current date is March 1, 2026.
Your ceding company has submitted a claim under their Property Excess of Loss Reinsurance treaties for a major hurricane.

The hurricane began on December 30, 2024, and ended on January 3, 2025. The ceding company suffered a total gross Property loss of $10,000,000. The independent loss adjuster allocated the losses based on the exact dates the damage occurred:
*   $6,000,000 of the loss occurred in 2024 (60% of the total loss).
*   $4,000,000 of the loss occurred in 2025 (40% of the total loss).

Review the attached `reinsurance_treaties.csv`. The treaties contain a standard "Interlocking Clause" for occurrences spanning multiple treaty years. 

Draft an Actuarial Adjustment Memo. You must:
1. Extract the Retention, Limit, and Interlocking Clause status for the applicable **Property** treaties for 2024 and 2025. Be highly precise to avoid the Casualty treaties.
2. Conduct a comparative analysis of how an excess of loss claim is calculated independently versus how it is calculated when an Interlocking Clause is triggered.
3. Execute the exact mathematical calculation required by the Interlocking Clause to determine the prorated retention for each specific year.
4. Calculate the exact reinsurance recovery amount due for 2024, the recovery for 2025, and formulate an actionable recommendation stating the total combined payout.

**Attached Files (Context):**
`reinsurance_treaties.csv`
```csv
Treaty_ID,LOB,Treaty_Year,Retention,Limit,Interlocking_Clause
XOL-101,Casualty,2024,$3000000,$20000000,No
XOL-102,Property,2024,$5000000,$20000000,Yes
XOL-103,Property,2025,$5000000,$25000000,Yes
XOL-104,Casualty,2025,$4000000,$25000000,No
```
Reference URL: `https://piu.org.pl/public/upload/ibrowser/140610%20Seminarium%20reasekuracyjne/Werner_Bautz_Gen_Re.pdf`

**3. Rubric (Negative Failure Focus)**

| # | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Extracts the 2024 Property data. | Critical | `reinsurance_treaties.csv`, Row 3 | Must identify XOL-102 with a $5,000,000 retention and $20,000,000 limit. | FALSE | **Model Failure (Extraction):** Extracted the Casualty data (XOL-101) by simply reading the first row for 2024. | None |
| 2 | Extracts the 2025 Property data. | Critical | `reinsurance_treaties.csv`, Row 4 | Must identify XOL-103 with a $5,000,000 retention and $25,000,000 limit. | FALSE | **Model Failure (Extraction):** Extracted the Casualty data (XOL-104) due to sequential reading bias. | None |
| 3 | Defines the Interlocking Clause mechanics. | Critical | Gen Re Presentation PDF, Page 41 | Must explain that the clause distributes the loss proportionally across years and reduces the retention (attachment point) accordingly. | FALSE | **Model Failure (Logic):** Ignored the interlocking clause and applied the full $5M retention to each year independently. | 1, 2 |
| 4 | Calculates the prorated 2024 Retention. | Critical | Prompt Text | Must multiply the $5,000,000 retention by the 60% loss ratio for 2024 to arrive at a $3,000,000 retention. | FALSE | **Model Failure (Math):** Failed to prorate the retention, locking in the $5M figure. | 3 |
| 5 | Calculates the prorated 2025 Retention. | Critical | Prompt Text | Must multiply the $5,000,000 retention by the 40% loss ratio for 2025 to arrive at a $2,000,000 retention. | FALSE | **Model Failure (Math):** Failed to prorate the retention. | 3 |
| 6 | Calculates the 2024 Recovery. | Major | Prompt Text | Must subtract the prorated retention ($3M) from the 2024 loss ($6M) to arrive at $3,000,000. | FALSE | Miscalculated the 2024 payout (e.g., $6M - $5M = $1M). | 4 |
| 7 | Calculates the 2025 Recovery. | Major | Prompt Text | Must subtract the prorated retention ($2M) from the 2025 loss ($4M) to arrive at $2,000,000. | FALSE | Miscalculated the 2025 payout (e.g., $4M - $5M = $0). | 5 |
| 8 | Provides the final total payout. | Critical | Prompt Text | Must state the total combined payout is $5,000,000. | FALSE | **Model Failure (Final):** Stated the total payout was $1,000,000 by failing the interlocking rule entirely. | 6, 7 |

**4. Model Analysis**
This prompt is explicitly engineered to force >30% failure on the rubric through a combination of extraction decoys and complex mathematical sequencing. Because LLMs process CSV data sequentially and rely heavily on keyword proximity, an untuned model will instantly anchor to the first 2024 row (XOL-101) and the first 2025 row (XOL-104) without verifying the Line of Business (LOB), causing an immediate failure of Criteria 1 and 2. Furthermore, even if the model extracts the correct $5,000,000 retention, it will likely fail the math trap. Standard logic dictates subtracting the retention from the loss ($6M - $5M = $1M, and $4M - $5M = $0). To achieve the Golden Response, the model must apply the Interlocking Clause, which requires prorating the retention based on the percentage of loss in each year (60% of $5M = $3M for 2024; 40% of $5M = $2M for 2025). This turns a $1,000,000 incorrect payout into the correct $5,000,000 payout.

**5. Golden Response**
ACTUARIAL ADJUSTMENT MEMO
TO: Reinsurance Accounting
FROM: Reinsurance Claims Actuary
DATE: March 1, 2026
SUBJECT: Excess of Loss Treaty – Interlocking Clause Calculation

1. CSV Data Extraction
A careful review of the `reinsurance_treaties.csv` reveals multiple treaties in place for the ceding company. Bypassing the Casualty treaties, the specific data for the Property treaties applicable to this loss are:
*   2024 Property (XOL-102): $5,000,000 Retention, $20,000,000 Limit. Interlocking Clause: Yes.
*   2025 Property (XOL-103): $5,000,000 Retention, $25,000,000 Limit. Interlocking Clause: Yes.

2. Comparative Analysis: Standard Excess of Loss vs. Interlocking Clause
If this loss were calculated under a standard Excess of Loss treaty without an interlocking clause, the $5,000,000 retention would apply fully to each separate treaty year. The 2024 recovery would be $1,000,000 ($6M loss - $5M retention), and the 2025 recovery would be $0 ($4M loss - $5M retention). 
However, because this continuous loss event spans two treaty periods, the Interlocking Clause is triggered. If the different policies provide for dates of loss in different reinsurance years, the loss is distributed to these years proportionally, reducing the limit and attachment point (retention) accordingly. 

3. Mathematical Calculation of Prorated Retentions
To apply the Interlocking Clause, we must prorate the $5,000,000 retention based on the percentage of the total $10,000,000 loss that fell into each respective year:
*   **2024 Retention:** $5,000,000 x 60% = $3,000,000.
*   **2025 Retention:** $5,000,000 x 40% = $2,000,000.

4. Actionable Recommendation and Final Recovery
Applying the prorated retentions to the allocated losses yields the final reinsurance recovery amounts:
*   **2024 Recovery:** $6,000,000 (Loss) - $3,000,000 (Prorated Retention) = $3,000,000.
*   **2025 Recovery:** $4,000,000 (Loss) - $2,000,000 (Prorated Retention) = $2,000,000.

I recommend authorizing a total combined reinsurance payout of $5,000,000 to the ceding company.

***

### Example 2: Title Insurance ALTA 32.2-06 (Direct Payment Trap with Heavy CSV Decoys)

**1. Metadata**
*   Task Type: Workflow
*   Category / Domain: Title Insurance / Escrow / Claims Adjudication
*   Workflow: Coverage Determination
*   Prompt Type: Claims Strategy Memo
*   Difficulty: Super Hard (Expert)

**2. Prompt**
You are a Title Insurance Claims Counsel. The current date is March 15, 2026.
A commercial mortgage lender is financing a $15 million construction project. The lender received an ALTA Loan Policy from your company. Attached to the policy is the ALTA 32.2-06 (Construction Loan - Loss of Priority - Insured's Direct Payment) endorsement.

During the project, the lender approved a draw request. The lender has filed a claim under their ALTA 32.2-06 endorsement for a $400,000 loss of priority, because a **Subcontractor** named **"GlassWorks Inc."** filed a valid mechanic's lien after not receiving payment. 

The lender argues that because the $400,000 was meticulously "designated for payment" to GlassWorks Inc. in the approved draw documents, and because the lender (the Insured) was the entity that released the funds, the loss of priority is fully insured.

Review the attached `disbursement_audit_log.csv` and the ALTA Endorsement Guidelines. 
Draft a Claims Strategy Memo. You must:
1. Extract the exact `Payer_Entity` and `Routing_Destination` for the specific transaction where the *Subcontractor* "GlassWorks Inc." filed a lien. Be highly precise, as there are similar records in the log.
2. Conduct a comparative analysis of the mechanic's lien coverage triggers provided under the standard ALTA 32-06 ("designated for payment") versus the ALTA 32.2-06 ("Insured's direct payment") endorsement.
3. Evaluate the scenario to determine if the lender's argument establishes liability under the ALTA 32.2-06 endorsement.
4. Assess the `Routing_Destination` data from the CSV to determine if the disbursement method complied with the policy requirements.
5. Formulate an actionable recommendation on whether to accept or deny the claim for the $400,000 loss of priority.

**Attached Files (Context):**
`disbursement_audit_log.csv`
```csv
Draw_Ref,Date,Entity_Type,Claimant_Name,Designated_Amount,Payer_Entity,Routing_Destination,Lien_Status
DR-04A,2025-10-01,Material Supplier,GlassWorks Inc,$400000,Title Company,Direct Wire to Supplier,Cleared
DR-04B,2025-10-15,Subcontractor,GlassWorks Inc,$400000,Insured Lender,Wire to GC Operating Acct,Lien Filed
DR-04C,2025-11-01,Subcontractor,GlassWorks LLC,$400000,Insured Lender,Direct Wire to Subcontractor,Lien Filed
```
ALTA Endorsements Guide: `https://www.virtualunderwriter.com/guidelines/2012/9/gl134869089900000006`

**3. Rubric (Negative Failure Focus)**

| # | Description | Weight | Sources (with exact PDF pages) | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Extracts the correct target row from the CSV. | Critical | `disbursement_audit_log.csv`, Row 3 | Must bypass the decoy entities and select DR-04B, identifying the Payer as "Insured Lender" and Routing as "Wire to GC Operating Acct". | FALSE | **Model Failure (Extraction):** Extracted DR-04A (which was a supplier) or DR-04C (which was an LLC), failing the strict fact-matching requirements. | None |
| 2 | Compares ALTA 32-06 and 32.2-06. | Critical | ALTA Guide URL, Section "Explanation" | Must explain that 32-06 covers funds "designated for payment" in draw documents, while 32.2-06 strictly requires "direct payment" by the Insured. | FALSE | **Model Failure (Logic):** Failed to differentiate the specific triggers of the two endorsement variations, treating all 32-series endorsements identically. | None |
| 3 | Evaluates the lender's argument. | Critical | Prompt Text & ALTA Guide | Must reject the lender's argument because "designated for payment" is the trigger for the 32-06, which the lender does not possess. | FALSE | **Model Failure (Logic):** Approved the claim because the funds were "designated for payment," hallucinating the coverage of the 32-06 onto the 32.2-06 endorsement. | 2 |
| 4 | Assesses the CSV routing data against the policy. | Critical | `disbursement_audit_log.csv`, Row 3 | Must note that routing funds to the "GC Operating Acct" violates the "direct payment" requirement of the ALTA 32.2-06. | FALSE | **Model Failure (Synthesis):** Extracted DR-04C's routing data ("Direct Wire") and falsely concluded the direct payment rule was satisfied. | 1 |
| 5 | Provides an actionable denial recommendation. | Major | Prompt Text | Must recommend denying the claim for the $400,000 loss of priority. | FALSE | **Model Failure (Final):** Recommended paying the claim or defending the lender's priority. | 3, 4 |

**4. Model Analysis**
This prompt is designed to trigger a massive data extraction failure using highly deceptive "lookalike" CSV records. The prompt asks for the *Subcontractor* named *GlassWorks Inc.* that *filed a lien*. DR-04A is a Material Supplier. DR-04C is GlassWorks LLC (wrong suffix). If the LLM lacks rigorous attention to detail, it will extract the wrong row. If it extracts DR-04C, it will see the routing was a "Direct Wire to Subcontractor" and falsely approve the claim. 
Even if it extracts the correct row (DR-04B), the prompt uses keyword association traps. Because the prompt heavily emphasizes that the draw documents "meticulously designated" the funds, an untuned LLM will naturally associate this with standard mechanic's lien coverage and approve the claim. The model must recognize that the ALTA 32.2-06 endorsement requires the *Insured* to make a *direct payment* to the specific lien claimant. Because the money was funneled through the General Contractor's operating account (as proven by the CSV), the direct payment requirement was breached. 

**5. Golden Response**
COVERAGE STRATEGY MEMO
TO: Title Claims Department
FROM: Claims Counsel
DATE: March 15, 2026
SUBJECT: Mechanic's Lien Claim – ALTA 32.2-06 Endorsement Application

1. CSV Data Extraction
To evaluate this claim, we must isolate the exact transaction from the `disbursement_audit_log.csv`. The lender is claiming a loss regarding the Subcontractor "GlassWorks Inc." who filed a lien. 
*   DR-04A is incorrect (Material Supplier, Cleared).
*   DR-04C is incorrect (GlassWorks LLC).
*   **DR-04B is the correct target record.** For DR-04B, the `Payer_Entity` was the "Insured Lender" and the `Routing_Destination` was "Wire to GC Operating Acct."

2. Comparative Analysis: ALTA 32-06 vs. ALTA 32.2-06
The ALTA 32 series endorsements provide limited mechanic's lien coverage, but the coverage triggers vary drastically:
*   **ALTA 32-06 (Loss of Priority):** Provides coverage to the extent that the charges for the services or materials were "designated for payment" in the documents supporting a construction loan advance.
*   **ALTA 32.2-06 (Insured's Direct Payment):** This endorsement is far more restrictive. It provides coverage *only* to the extent that direct payment to the Mechanic's Lien claimant has been made by the Insured or on the Insured's behalf on or before the Date of Coverage.

3. Scenario Evaluation: The Lender's Argument
The lender argues that because the $400,000 was meticulously "designated for payment" in the draw documents, the loss of priority should be covered. If the lender possessed the standard ALTA 32-06 endorsement, this argument would be valid. However, the lender's policy contains the ALTA 32.2-06. The phrase "designated for payment" has no bearing on coverage under the 32.2-06, making the lender's argument legally irrelevant to the contract they actually purchased.

4. Assessment of CSV Disbursement Data
To trigger coverage under the ALTA 32.2-06, a direct payment must be made to the lien claimant. A review of the CSV reveals that while the Insured Lender was the payer, the `Routing_Destination` was a "Wire to GC Operating Acct." Because the Insured relinquished control of the funds to the General Contractor rather than paying the subcontractor directly, the mandatory "direct payment" condition of the ALTA 32.2-06 was breached. 

5. Actionable Recommendation
I recommend that we issue a full, formal denial of the lender's claim regarding the framing subcontractor's mechanic's lien. The ALTA 32.2-06 endorsement explicitly excludes loss of priority if the mechanic's lien claimant was not directly paid by the Insured or on the Insured's behalf. The General Contractor's failure to pass the funds down falls entirely outside the scope of this specific endorsement's protection.
```handwritten-ink
{
	"versionAtEmbed": "0.2.2",
	"filepath": "Ink/Writing/2026.3.13 - 20.37pm.writing"
}
```
