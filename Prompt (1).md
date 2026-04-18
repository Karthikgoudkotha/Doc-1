Scenarios
Scenario 1: Pension Actuarial – AFTAP Asset Smoothing & IRS Segment Rate Liability
1. Metadata
Task Type: Workflow
Category / Domain: Actuarial (Pension / Defined Benefit)
Workflow: Adjusted Funding Target Attainment Percentage (AFTAP) & Liability Valuation
Prompt Type: Actuarial Certification Memo
Difficulty: Singularity (Expected Failure Rate >99%)
2. Prompt
You are an Enrolled Actuary. The current valuation date is April 15, 2026.
You must perform a mid-year funding assessment for a single-employer defined benefit pension plan for the 2026 plan year. This requires calculating the Adjusted Funding Target Attainment Percentage (AFTAP) utilizing statutory asset smoothing rules, followed by a Present Value liability calculation using IRS-published segment rates.
Calculation Directives:
Asset Smoothing: Calculate the Actuarial Value of Assets. The plan utilizes a smoothing method that establishes a Preliminary Smoothed Value. However, under standard IRS regulations, the final Actuarial Value of Assets cannot exceed a 110% corridor (cap) of the actual Market Value of Assets. Apply this cap if the preliminary value breaches the corridor.
AFTAP Calculation: Calculate the AFTAP. The formula for AFTAP is the (Actuarial Value of Assets minus the Carryover Balance) divided by the Funding Target. Carry the percentage out to two decimal places.
Rate Extraction: Access the attached IRS Notice 2025-47. Extract the "Adjusted 24-Month Average First Segment Rate" applicable for Plan Years beginning in 2026 for the specific target month of September 2025. Ignore the unadjusted rates and decoy months.
Liability Valuation: A block of retirees is guaranteed a single lump-sum payout of $2,000,000 due exactly 5 years from the valuation date. Calculate the Present Value (PV) of this liability using standard annual compound interest and the First Segment Rate extracted in Step 3.
Raw Data:
Market Value of Assets (MVA): $10,000,000
Preliminary Smoothed Asset Value: $11,500,000
Carryover Balance: $500,000
Funding Target: $14,000,000
Target Lump Sum Liability: $2,000,000
Time to Payout: 5 Years
Attached Files (Context):
IRS_Notice_2025_47.pdf - Public URL: https://www.irs.gov/pub/irs-drop/n-25-47.pdf
Present your step-by-step mathematical proofs and the final valuations in an Actuarial Certification Memo. Include parenthetical citations referring to the specific pages and tables from the source document for your rate extraction.
3. Rubric
#
Description
Weight
Sources
Justification
Met
Failure Reasoning
Dependent Criteria
1
Action Step A: Calculates the maximum permissible asset corridor limit as $11,000,000.
Critical
Prompt Text
$10,000,000 (MVA) * 1.10 = $11,000,000.
FALSE
Failed to calculate the 110% corridor against the Market Value.
None
2
Rationale Step A: Determines the final Actuarial Value of Assets is capped at $11,000,000.
Critical
Prompt Text
The Preliminary Smoothed Value ($11.5M) exceeds the $11.0M corridor, triggering the statutory cap.
FALSE
Used the raw $11,500,000 smoothed value or the $10,000,000 MVA.
1
3
Calculation Step A: Calculates the AFTAP numerator (adjusted assets) as $10,500,000.
Major
Prompt Text
$11,000,000 (Actuarial Value of Assets) - $500,000 (Carryover Balance).
FALSE
Failed to deduct the carryover balance from the assets.
2
4
Calculation Step B: Calculates the final AFTAP as 75.00%.
Critical
Prompt Text
$10,500,000 / $14,000,000 (Funding Target) = 0.7500.
FALSE
Math error or divided by the wrong denominator.
3
5
Action Step B: Locates the "Adjusted 24-Month Average Segment Rates" table in the IRS Notice.
Minor
IRS_Notice_2025_47.pdf, Page 3
Must distinguish between the Unadjusted and Adjusted tables.
FALSE
Pulled from the Unadjusted table or 25-Year Average table.
None
6
Criterion 1A: Extracts the September 2025 Adjusted First Segment Rate for 2026 Plan Years as 4.81%.
Critical
IRS_Notice_2025_47.pdf, Page 3
The specific rate required for a 5-year liability projection.
FALSE
Extracted the 2025/2024 plan year rate or a decoy month.
5
7
Action Step C: Formulates the Present Value equation utilizing the extracted 4.81% segment rate.
Major
Prompt Text
PV = $2,000,000 / (1 + 0.0481)^5.
FALSE
Used a linear discount or applied the segment rate to the wrong formula.
6
8
Calculation Step C: Evaluates the discount denominator (1.0481^5) as 1.264775 (allow rounding variance).
Minor
Prompt Text
Execution of the exponent math.
FALSE
Mathematical failure on the exponent.
7
9
Calculation Step D: Calculates the final Present Value (PV) of the liability as $1,581,308.85.
Critical
Prompt Text
$2,000,000 / 1.264775 = $1,581,308.85.
FALSE
Final quantum error due to upstream extraction or math failures.
8
10
Action Step D: Formats as an Actuarial Certification Memo and includes the valuation date (April 15, 2026).
Minor
Prompt Text
Required styling and temporal anchor.
FALSE
Omitted the memo formatting or the date.
None

4. Golden Response
ACTUARIAL CERTIFICATION MEMORANDUM
DATE: April 15, 2026
SUBJECT: 2026 AFTAP Determination and Segment Rate Liability Valuation
5. Actuarial Value of Assets and AFTAP Calculation
Under statutory pension funding rules, the smoothed Actuarial Value of Assets cannot exceed a 110% corridor of the actual Market Value of Assets (MVA).
MVA: $10,000,000
Maximum Asset Corridor: $10,000,000 \times 1.10 = $11,000,000.
Because the Preliminary Smoothed Asset Value ($11,500,000) exceeds the $11,000,000 corridor limit, the final Actuarial Value of Assets must be capped at $11,000,000.
The AFTAP is calculated by subtracting the Carryover Balance from the Actuarial Value of Assets, and dividing the result by the Funding Target.
Adjusted Assets (Numerator): $11,000,000 - $500,000 (Carryover Balance) = $10,500,000.
AFTAP Calculation: $10,500,000 / $14,000,000 (Funding Target) = 0.7500.
Final AFTAP: 75.00%
6. Segment Rate Extraction and Liability Valuation
To value the 5-year lump sum liability, we must utilize the First Segment Rate (which covers cash flows for years 0 to 5) from the "Adjusted 24-Month Average Segment Rates" table for Plan Years beginning in 2026, using the target month of September 2025.
Adjusted First Segment Rate: 4.81% (IRS_Notice_2025_47.pdf, Page 3, Adjusted 24-Month Average Segment Rates Table).
Using standard annual compound interest, the Present Value (PV) of the $2,000,000 liability due in 5 years is calculated as follows:
Formula: 



Final Present Value Liability: $1,581,308.85
Scenario 2: Reinsurance Actuarial – Interlocking Clause & Franchise Indexation
1. Metadata
Task Type: Workflow
Category / Domain: Actuarial (Reinsurance)
Workflow: Complex Commutation & Limit Apportionment
Prompt Type: Actuarial Commutation Memo
Difficulty: Singularity (Expected Failure Rate >99%)
2. Prompt
You are a Reinsurance Actuary. The current valuation date is April 15, 2026.
You must calculate the final recovery owed to a Cedent under an Excess of Loss (XoL) property treaty for a massive industrial fire. The loss spans multiple treaty years, triggering both a "London Market Indexation Clause (Franchise)" and an "Interlocking Clause."
Calculation Directives:
Indexation Calculation: The treaty utilizes a Franchise Inflation Clause with a 10.0% threshold. Divide the Settlement Index by the Base Index to find the inflation factor. If the inflation breaches the 10.0% threshold, apply the full inflation multiplier to BOTH the treaty Retention and the treaty Limit (per Guy Carpenter rules).
Loss Apportionment: The industrial fire burned continuously across two treaty years. Calculate the percentage of the Total Ground Up (FGU) Loss that falls into Year 1.
Interlocking Clause Application: The treaty utilizes BRMA 22A (Interlocking Clause). Because the occurrence involves more than one reinsurance period, the Retention and Limit for Year 1 must be reduced. Multiply the Indexed Retention and Indexed Limit from Step 1 by the Year 1 loss percentage from Step 2 to determine the "Apportioned Retention" and "Apportioned Limit" for Year 1.
Final Recovery: Calculate the Year 1 Loss to the Layer (Year 1 Loss minus Year 1 Apportioned Retention). Cap this recovery at the Year 1 Apportioned Limit to determine the final payout.
Raw Data:
Treaty Base Retention: $5,000,000
Treaty Base Limit: $10,000,000 (i.e., $10M xs $5M)
Base Date Index: 200.0
Settlement Date Index: 230.0
Total FGU Loss (Both Years): $20,000,000
Year 1 FGU Loss: $15,000,000
Year 2 FGU Loss: $5,000,000
Attached Files (Context):
guy_carpenter_indexation_clauses.html - Public URL: https://www.guycarp.com/insights/2008/09/indexation-clauses-in-liability-reinsurance-treaties-a-comparison-across-europe.html
BRMA_Interlocking_Clause.doc - Public URL:(https://www.brma.org/docs/Interlocking_Clause_BRMA__22A.doc)
Present your step-by-step mathematical proofs in a formal Actuarial Commutation Memo. Include parenthetical citations for the source logic.
3. Rubric
#
Description
Weight
Sources
Justification
Met
Failure Reasoning
Dependent Criteria
1
Calculation Step A: Calculates the inflation factor as 1.15 (or 15.0%).
Minor
Prompt Text
230.0 / 200.0 = 1.15.
FALSE
Math error.
None
2
Action Step A: Identifies that the 15.0% inflation breaches the 10.0% Franchise threshold.
Critical
guy_carpenter_indexation.html
Recognizes the conditional logic trigger for Franchise clauses.
FALSE
Failed to check the threshold or concluded it was not breached.
1
3
Rationale Step A: Applies the full 1.15 multiplier to establish an Indexed Retention of $5,750,000 and an Indexed Limit of $11,500,000.
Critical
guy_carpenter_indexation.html
A Franchise clause applies the full indexation value once breached, unlike a Severe Inflation Clause.
FALSE
Applied only the 5% excess to the limits, or failed to index the Limit.
1, 2
4
Calculation Step B: Calculates the Year 1 loss apportionment factor as 75.0%.
Minor
Prompt Text
$15,000,000 (Year 1) / $20,000,000 (Total) = 0.75.
FALSE
Divided by the wrong base.
None
5
Action Step B: Identifies that under BRMA 22A, the retention and limit must be scaled by the 75.0% proportion of loss.
Critical
BRMA_Interlocking_Clause.doc
Applies the Interlocking Clause rule to prevent the Cedent from absorbing multiple full retentions.
FALSE
Ignored the Interlocking Clause.
4
6
Rationale Step B: Calculates the Year 1 Apportioned Retention as $4,312,500.
Critical
Prompt Text
$5,750,000 (Indexed Retention) \times 0.75 = $4,312,500.
FALSE
Scaled the original $5M retention instead of the Indexed retention.
3, 5
7
Rationale Step C: Calculates the Year 1 Apportioned Limit as $8,625,000.
Critical
Prompt Text
$11,500,000 (Indexed Limit) \times 0.75 = $8,625,000.
FALSE
Scaled the original $10M limit instead of the Indexed limit.
3, 5
8
Calculation Step C: Calculates the Year 1 Loss to the Layer as $10,687,500.
Major
Prompt Text
$15,000,000 (Year 1 Gross Loss) - $4,312,500 (Apportioned Retention) = $10,687,500.
FALSE
Subtracted the retention from the $20M total loss instead of the Year 1 loss.
6
9
Calculation Step D: Determines the Final Year 1 Recovery is capped at $8,625,000.
Critical
Prompt Text
Because the $10,687,500 Loss to Layer exceeds the $8,625,000 Apportioned Limit, the payout is capped.
FALSE
Paid the full $10.68M or capped it at the original $10M limit.
7, 8
10
Action Step C: Formats output as an Actuarial Commutation Memo and includes the date April 15, 2026.
Minor
Prompt Text
Required styling and temporal anchor.
FALSE
Formatted casually or omitted the date.
None

4. Golden Response
ACTUARIAL COMMUTATION MEMORANDUM
DATE: April 15, 2026
SUBJECT: Excess of Loss Claim Settlement – Indexation and Interlocking Clause Application
5. Franchise Indexation Calculation
To account for inflation over the claim period, we calculate the inflation factor by dividing the Settlement Index by the Base Index.
Inflation Factor = 230.0 / 200.0 = 1.15 (15.0% inflation).
The treaty stipulates a Franchise Inflation Clause with a 10.0% threshold. Because 15.0% exceeds the 10.0% margin, the Franchise trigger is breached. According to standard reinsurance principles, a Franchise clause dictates that the full value of the index increase must be applied once the margin is breached, not just the excess amount (guy_carpenter_indexation_clauses.html).
Both the Retention and Limit must be indexed by the 1.15 multiplier:
Indexed Retention = $5,000,000 \times 1.15 = $5,750,000.
Indexed Limit = $10,000,000 \times 1.15 = $11,500,000.
6. Loss Apportionment and Interlocking Clause (BRMA 22A)
Because the single occurrence spans two treaty years, the BRMA 22A Interlocking Clause applies. This clause dictates that the retention and limit for a specific contract period must be scaled by the percentage that the covered claim bears to the total loss occurrence (BRMA_Interlocking_Clause.doc).
Year 1 Apportionment Factor = $15,000,000 (Year 1 Loss) / $20,000,000 (Total Loss) = 0.75 (75.0%).
We apply this 75.0% factor to the Indexed treaty parameters to find the Apportioned parameters for Year 1:
Year 1 Apportioned Retention = $5,750,000 \times 0.75 = $4,312,500.
Year 1 Apportioned Limit = $11,500,000 \times 0.75 = $8,625,000.
7. Final Recovery Calculation
The Cedent absorbs the loss from ground up to the Apportioned Retention. The remainder falls to the reinsurance layer.
Year 1 Loss to Layer = $15,000,000 (Year 1 FGU Loss) - $4,312,500 (Year 1 Apportioned Retention) = $10,687,500.
The $10,687,500 Loss to the Layer must be capped at the Year 1 Apportioned Limit of $8,625,000.
Final Year 1 Recovery: $8,625,000.
