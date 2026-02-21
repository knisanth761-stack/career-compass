# 📈 Momentum Score Engine (0–100) — Working Principle

## Goal
Compute a single score `momentumScore` (0–100) that represents how strong the user’s preparation is **recently**.

“Recently” matters because a person who studied 3 months ago but stopped should not have a high score today.

---

## Inputs
From the last **14 days** of `DailyLog[]`:
- `daysActive` = number of days with a log (0–14)
- `totalMinutes` = sum of `timeSpentMinutes`
- `easy, medium, hard` solved counts in last 14 days
- `topicsTouched` = unique topics practiced in last 14 days

From streak engine:
- `currentStreak`

---

## Output
- `momentumScore` (integer 0–100)
- Explainable breakdown (for UI):
  - consistencyPoints
  - effortPoints
  - difficultyPoints
  - coveragePoints
  - streakBonus

---

## Scoring Breakdown (Total = 100)

### 1) Consistency Points (0–35)
Based on how many days the user was active in the last 14 days.

Formula:
- `consistencyRatio = daysActive / 14`
- `consistencyPoints = round(35 * consistencyRatio)`

Example:
- 7 active days → 7/14 = 0.5 → 35*0.5 = 17.5 → 18 points

Why:
Consistency is the #1 predictor of progress.

---

### 2) Effort Points (0–25)
Based on study time in the last 14 days.

We cap it so people don’t “game” the score by studying 12 hours in one day.

Set:
- `targetMinutes = 14 * user.goal.dailyMinutes`  (e.g., 14*90 = 1260)
- `effortRatio = min(totalMinutes / targetMinutes, 1.0)`
- `effortPoints = round(25 * effortRatio)`

Example:
- user target = 1260 mins
- total = 900 mins
- effortRatio = 900/1260 = 0.714
- effortPoints ≈ 18

---

### 3) Difficulty Points (0–25)
Harder problems should add more score.

Weighted solved count:
- `weightedSolved = 1*easy + 2*medium + 4*hard`

Set a reasonable 14-day target:
- `difficultyTarget = 120`  (tunable constant)

Compute:
- `difficultyRatio = min(weightedSolved / difficultyTarget, 1.0)`
- `difficultyPoints = round(25 * difficultyRatio)`

Why:
Someone solving mediums/hards should be rewarded more than only easy.

---

### 4) Coverage Points (0–10)
Rewards exploring multiple topics (prevents being stuck in only one topic forever).

Let:
- `uniqueTopics = count(topicsTouched)`
- `coverageTarget = 6` (in 14 days)

Compute:
- `coverageRatio = min(uniqueTopics / coverageTarget, 1.0)`
- `coveragePoints = round(10 * coverageRatio)`

---

### 5) Streak Bonus (0–5)
A small bonus to encourage continuing the chain.

Compute:
- if `currentStreak == 0` → bonus = 0
- else `streakBonus = min(currentStreak, 5)`  (max 5)

---

## Final Score
`momentumScore = consistencyPoints + effortPoints + difficultyPoints + coveragePoints + streakBonus`

Clamp:
- if > 100, set to 100
- if < 0, set to 0

---

## Example (Walkthrough)

Assume in last 14 days:
- daysActive = 9
- totalMinutes = 840
- easy=30, medium=18, hard=2
- uniqueTopics = 5
- currentStreak = 4
- dailyMinutes goal = 90 → targetMinutes = 1260

Compute:
- consistencyPoints = round(35 * 9/14) = round(22.5) = 23
- effortPoints = round(25 * 840/1260) = round(16.7) = 17
- weightedSolved = 30 + 2*18 + 4*2 = 30 + 36 + 8 = 74
- difficultyPoints = round(25 * 74/120) = round(15.4) = 15
- coveragePoints = round(10 * 5/6) = round(8.3) = 8
- streakBonus = 4

Final:
momentumScore = 23 + 17 + 15 + 8 + 4 = 67

---

## UI Usage (Motivation Loop)
Show the breakdown:
- “Consistency: 23/35”
- “Effort: 17/25”
- “Difficulty: 15/25”
- “Coverage: 8/10”
- “Streak Bonus: 4/5”

Then show a single suggestion:
- If consistencyPoints is low → “Aim 5 days this week”
- If difficultyPoints is low → “Add 3 medium problems”
- If coveragePoints is low → “Try 1 new topic”

---

## Notes
- All targets (`difficultyTarget`, `coverageTarget`) are tunable constants.
- Keep it explainable. The UI should always show “why your score is X”.
