# 🗄 Career Compass — Data Models

This document defines the core data we store and the exact inputs/outputs used by the logic engine.

---

## 1) User

Represents one student using the platform.

**Fields**
- `id` (string)
- `name` (string)
- `email` (string, optional)
- `createdAt` (date)
- `timezone` (string, default: Asia/Kolkata)
- `track` (enum): `DSA_CORE` | `WEB_DEV` | `DATA_ML` | `CYBER`
- `goal` (object)
  - `targetRole` (string) e.g., "SDE Intern"
  - `targetTier` (enum): `SERVICE` | `MID_PRODUCT` | `STRONG_PRODUCT` | `ELITE`
  - `dailyMinutes` (number) e.g., 90

---

## 2) Roadmap

A roadmap is a structured list of topics/steps.

### RoadmapTopic
**Fields**
- `topicId` (string)
- `title` (string) e.g., "Sliding Window"
- `track` (enum)
- `level` (enum): `BEGINNER` | `INTERMEDIATE` | `ADVANCED`
- `prerequisites` (string[])  // list of topicIds
- `estimatedDays` (number)
- `resources` (array)
  - `label` (string)
  - `url` (string)

### UserRoadmapProgress
Tracks which roadmap topics the user completed.

**Fields**
- `userId`
- `topicId`
- `status` (enum): `NOT_STARTED` | `IN_PROGRESS` | `DONE`
- `startedAt` (date, optional)
- `completedAt` (date, optional)

---

## 3) DailyLog (Progress Entry)

One log per day (or multiple; we can merge later).

**Fields**
- `logId` (string)
- `userId` (string)
- `date` (YYYY-MM-DD string)  // important for streak engine
- `dsa` (object)
  - `problemsSolved` (number)
  - `easy` (number)
  - `medium` (number)
  - `hard` (number)
  - `topicsTouched` (string[])  // ["Arrays","Two Pointers"]
- `coreCS` (object, optional)
  - `osMinutes` (number)
  - `dbmsMinutes` (number)
  - `cnMinutes` (number)
- `projects` (object, optional)
  - `minutes` (number)
  - `tasksDone` (string[]) // ["Built dashboard UI"]
- `timeSpentMinutes` (number)
- `notes` (string, optional)

---

## 4) ScoreSnapshot (Computed Results)

Saved output of the logic engine for fast dashboard loading.

**Fields**
- `userId`
- `date` (YYYY-MM-DD)
- `streak` (object)
  - `current` (number)
  - `longest` (number)
- `momentumScore` (number) // 0–100
- `dsaScore` (number)      // 0–100
- `projectScore` (number)  // 0–100
- `readinessTier` (enum): `SERVICE` | `MID_PRODUCT` | `STRONG_PRODUCT` | `ELITE`

---

## 5) CompanyRequirement (Company Dataset)

Defines what it takes to be "ready" for a company bucket.

**Fields**
- `companyId` (string)
- `companyName` (string)
- `tier` (enum): `SERVICE` | `MID_PRODUCT` | `STRONG_PRODUCT` | `ELITE`
- `requirements` (object)
  - `minProblems` (number)
  - `minTopics` (string[])       // required topics
  - `minProjects` (number)
  - `minMomentumScore` (number)
  - `coreCSRequired` (boolean)
- `notes` (string) // e.g., "Focus on OS + DBMS basics"

---

# 🧠 Logic Engine Inputs & Outputs (Contract)

## Input to Engine
- user profile (`User`)
- last N daily logs (`DailyLog[]`) e.g., last 60 days
- roadmap progress (`UserRoadmapProgress[]`)
- company dataset (`CompanyRequirement[]`)

## Output from Engine
- `ScoreSnapshot`
- `CompanyReachResult`:
  - `readyNow` (companyName[])
  - `closeToReach` (companyName[])
  - `longTerm` (companyName[])
  - `missingRequirements` (map companyName -> missing items)

---

# ✅ Notes

- We keep raw logs separate from computed scores.
- Engine outputs must be explainable (show “why” a company is suggested).
