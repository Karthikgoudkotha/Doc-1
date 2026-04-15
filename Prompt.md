

### **Example 49: Employment Practices Liability (The Eroding Aggregate & Uninsurable Loss Trap)**

#### **1. Metadata**
* **Task Type:** Workflow / Multi-Constraint Reasoning
* **Category / Domain:** Management Liability (EPL)
* **Workflow:** Claims Adjudication, Limit Erosion, Co-Insurance & Exclusions
* **Prompt Type:** Coverage Determination
* **Difficulty:** Tartarus Tier (Expected Failure Rate: >99%)

#### **2. Prompt**
You are a Management Liability Claims Adjuster. The current date is October 24, 2026.

**Task:** Your insured, "Omega Enterprises", has submitted an Immigration Practices Claim under their Employment Practices Liability (EPL) policy. Based strictly on the Policy Excerpts provided below, determine the maximum amount the Insurer will pay for this specific claim, and state the exact dollar amount the insured must pay out-of-pocket (including their retention, co-insurance, uninsured excesses, and excluded amounts). Show your step-by-step math.

**--- CLAIM DATA ---**
* **Total Claim Incurred to Date:** $230,000.
* **Breakdown of Claim:** $180,000 in attorney fees (Defense Costs) + $50,000 civil penalty assessed by the government.
* **Prior Claims:** The insured had one previous claim this policy period (a wrongful termination suit), for which the Insurer has already paid out exactly **$920,000**.

**--- POLICY EXCERPTS ---**
**Section I: Declarations**
* **Aggregate Limit of Liability:** $1,000,000 for all claims first made during the policy period.
* **Immigration Practices Sub-Limit:** $100,000 maximum per policy period. (Note: Sub-limits are part of, and not in addition to, the Aggregate Limit of Liability).
* **Retention:** $25,000 per claim.
* **Co-Insurance:** 20% borne by the Insured. (The Insurer pays 80% of covered loss in excess of the Retention, up to the applicable limit. The Insured pays the remaining 20%).

**Section II: Definitions**
* **Loss:** Means damages, judgments, settlements, and Defense Costs. *Loss shall not include civil or criminal fines, penalties, or taxes.*
* **Defense Costs:** Means reasonable and necessary fees, costs, and expenses consented to by the Insurer resulting solely from the investigation, adjustment, defense, or appeal of a Claim. Defense Costs erode the Limits of Liability.

**Output Requirements:**
Provide a step-by-step mathematical breakdown. Conclude your response by explicitly stating the Insurer's Maximum Payment and the Insured's Total Out-of-Pocket Cost.

---

#### **3. Rubric (Negative Failure Focus)**

| # | Description | Weight | Sources | Justification | Met | Failure Reasoning (Model Failure Example) | Dependent Criteria |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Excludes the $50,000 civil penalty from the covered Loss calculation. | **Critical** | Prompt Text (Sec II) | The definition of "Loss" explicitly excludes civil fines and penalties. | FALSE | **Model Failure:** Included the $50,000 penalty in the gross covered loss, subjecting it to the retention and insurer co-insurance. | 5 |
| 2 | Applies the $25,000 retention to the $180,000 covered defense costs. | **Critical** | Prompt Text (Sec I) | $180,000 - $25,000 = $155,000 remaining covered loss. | FALSE | **Model Failure:** Subtracted the retention from the $100,000 sub-limit instead of the gross loss, or subtracted it from the $230,000 total. | 5 |
| 3 | Calculates the Insurer's 80% share of the remaining covered loss as $124,000. | **Major** | Prompt Text (Sec I) | $155,000 * 0.80 = $124,000. | FALSE | **Model Failure:** Failed the Co-Insurance math, often calculating 20% of the limit instead of the loss. | 4, 5 |
| 4 | Identifies the Aggregate Limit cap of $80,000 overrides the Sub-Limit. | **Critical** | Prompt Text (Sec I) | The $1,000,000 aggregate has already been eroded by $920,000. Only $80,000 remains. Even though the Sub-Limit is $100,000, the Aggregate Limit is the absolute ceiling. | FALSE | **Model Failure:** Capped the insurer's payment at the $100,000 Sub-Limit, completely ignoring the prior claims erosion. | 5 |
| 5 | States the Insured's total out-of-pocket cost is exactly $150,000. | **Critical** | Prompt Text | Total Incurred ($230,000) minus Insurer's Payment ($80,000) = $150,000. | FALSE | **Model Failure:** Failed to add the excluded $50,000 fine back into the insured's out-of-pocket total, or cascaded math errors from the limits. | None |

---

#### **4. Model Analysis**
This prompt is designed to trigger total "Context Collapse" and "Attention Hijacking" in an LLM. 
1. **The Excluded Loss Trap:** The model must read the definition of "Loss", recognize that the $50,000 penalty is excluded, and physically remove it from the mathematical track, while remembering to add it back to the insured's out-of-pocket total at the very end.
2. **The Limit Conflict Trap:** The prompt explicitly displays an "Immigration Practices Sub-Limit: $100,000". Standard models will lock onto this number as the absolute ceiling for the claim. However, the model must simultaneously track the "Aggregate Limit" ($1,000,000) and subtract the "Prior Claims" ($920,000) to realize only $80,000 remains on the entire policy. The remaining aggregate ($80k) supersedes the sub-limit ($100k).
3. **The Algebra Trap:** The model must calculate the theoretical payout ($180k - $25k = $155k; 80% of $155k = $124k) to determine if it breaches the limit. Once $124k breaches the $80k limit, the insurer's payment freezes at $80k, leaving the insured to pay the rest. Models almost universally fail to balance the equation `Total Loss = Insurer Share + Insured Share` when multiple caps and exclusions are applied.

---

#### **5. Golden Response**

**Coverage Determination & Mathematical Breakdown**

**1. Identification of Covered Loss**
* Total Claim Incurred: $230,000
* Excluded Amounts: Section II (Definitions) explicitly states that "Loss" does not include civil penalties. Therefore, the $50,000 civil penalty is strictly uninsured and falls 100% to the Insured.
* **Covered Loss (Defense Costs):** $180,000

**2. Application of Retention & Co-Insurance**
* Covered Loss: $180,000
* Less Retention: -$25,000
* Remaining Covered Loss: $155,000
* **Insurer's Share (80%):** $124,000
* **Insured's Co-Insurance Share (20%):** $31,000

**3. Application of Policy Limits**
We must test the Insurer's calculated share ($124,000) against the available policy limits.
* **Immigration Sub-Limit:** The maximum allowed for this claim type is $100,000.
* **Aggregate Limit Remaining:** The policy has a $1,000,000 aggregate limit. However, $920,000 has already been paid for a prior claim. Therefore, the remaining available limit for the entire policy is **$80,000**.
* **Limit Cap:** Because sub-limits are part of and not in addition to the Aggregate Limit, the absolute maximum the Insurer can pay is the remaining aggregate of $80,000 (which overrides the $100,000 sub-limit). 
* Since the Insurer's calculated share ($124,000) exceeds the remaining limit ($80,000), the Insurer's payment is capped at $80,000.

**4. Final Apportionment**
* **Insurer's Maximum Payment:** **$80,000**
* **Insured's Total Out-of-Pocket Cost:** **$150,000**
*(Calculated as: $230,000 total incurred minus the $80,000 paid by the insurer. This consists of the $25,000 retention, $31,000 co-insurance, $44,000 in uninsured defense costs exceeding the limit, and the $50,000 excluded civil penalty).*