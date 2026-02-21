# 🎯 Company Reach Meter Engine — Working Principle

## Goal
Classify companies into 3 buckets for a user:
- ✅ **Ready Now** (user meets requirements)
- ⏳ **Close to Reach** (user is near; missing only a few things)
- 🚀 **Long Term** (user is far from requirements)

Also generate an **explainable reason**:
- what requirements are satisfied
- what is missing
- the next actionable steps

---

## Inputs

1) User metrics (from logs + engines):
- `totalProblems` (lifetime)
- `topicsCompleted` (set)
- `projectCount`
- `momentumScore` (0–100)
- `coreCSCovered` (boolean; or % later)

2) Company dataset (`CompanyRequirement[]`)
Each company has:
- `minProblems`
- `minTopics[]`
- `minProjects`
- `minMomentumScore`
- `coreCSRequired`

---

## Output

A `CompanyReachResult`:
- `readyNow`: string[]
- `closeToReach`: string[]
- `longTerm`: string[]
- `missingRequirements`: map(companyName -> string[])
- `matchedRequirements`: map(companyName -> string[])

---

## Core Idea: Requirement Checks

For each company:

### Requirement 1 — Problems Solved
Pass if:
- `totalProblems >= minProblems`

### Requirement 2 — Topics Coverage
Pass if:
- user has completed **ALL** topics in `minTopics`
(Use set membership checks)

### Requirement 3 — Projects
Pass if:
- `projectCount >= minProjects`

### Requirement 4 — Momentum
Pass if:
- `momentumScore >= minMomentumScore`

### Requirement 5 — Core CS (if required)
Pass if:
- if `coreCSRequired == false` → auto pass
- else user must have `coreCSCovered == true` (or above threshold later)

---

## Eligibility Score (How “Close” They Are)

We compute a simple completeness score:

Let `totalReq = 5`  
Let `passedReq = number of passed requirements`

`completeness = passedReq / totalReq`

---

## Bucket Rule

- If `completeness == 1.0` → ✅ Ready Now
- Else if `completeness >= 0.6` → ⏳ Close to Reach
- Else → 🚀 Long Term

Why 0.6?
It means they satisfy at least 3 out of 5 requirements.

(Tunable constant)

---

## Explainability (Most Important Part)

For each company:
- `matchedRequirements`: list all passed items
- `missingRequirements`: list all missing items in plain English

Example missing list:
- "Solve 80 more problems (need 200)"
- "Complete topics: Sliding Window, Binary Search"
- "Build 1 more project"
- "Increase momentum score to 60 (current 45)"
- "Cover Core CS basics (OS/DBMS/CN)"

---

## Next Steps Generator (Simple Version)

From the missing requirements, generate 1–3 next actions:

Priority order:
1) Consistency / Momentum
2) Topics missing
3) Problems gap
4) Projects gap
5) Core CS gap

Return actionable suggestions:
- "Do 5 active days this week"
- "Finish Sliding Window + Binary Search"
- "Solve 30 medium problems"
- "Build a project feature and push weekly"
- "Revise DBMS normalization"

---

## Efficiency
- Topics check uses a set → fast membership
- Each company evaluation is O(T) where T = topics required
- Total complexity: O(C * T), where C = number of companies in dataset

---

## Example (Walkthrough)

User:
- totalProblems = 180
- topicsCompleted = {Arrays, Two Pointers, Sliding Window}
- projectCount = 1
- momentumScore = 55
- coreCSCovered = false

Company "Zoho" needs:
- minProblems = 200 ❌ (missing 20)
- minTopics = Arrays, Two Pointers, Sliding Window, Binary Search ❌ (missing Binary Search)
- minProjects = 2 ❌
- minMomentumScore = 60 ❌
- coreCSRequired = false ✅

Passed = 1/5 → completeness = 0.2 → 🚀 Long Term

But after improvements, user will move buckets.

---

## UI Usage
For each company card show:
- Badge: Ready / Close / Long Term
- ✅ Matched list
- ❌ Missing list
- One “Next 14-day plan” button (later)
