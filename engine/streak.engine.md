# 🔥 Streak Engine — Working Principle

## Goal
Calculate:
- `currentStreak` = how many consecutive days up to today the user logged progress
- `longestStreak` = maximum consecutive days ever

---

## Input
A list of log dates (strings) like:

["2026-02-15","2026-02-16","2026-02-18","2026-02-19"]


Rules:
- Dates may be unordered
- There can be duplicates
- There may be gaps

---

## Output
Example:

currentStreak = 2
longestStreak = 2


---

## Algorithm (Simple + Correct)

### Step A — Clean the dates
1. Convert list to a **set** (removes duplicates)
2. Sort dates ascending

Now we have a clean timeline.

### Step B — Compute streaks
Maintain:
- `current = 1` (streak currently counting)
- `longest = 1`

For each date from i = 1 to end:
- if `date[i]` is **exactly 1 day** after `date[i-1]`
  - `current++`
  - `longest = max(longest, current)`
- else
  - break the chain
  - set `current = 1`

### Step C — Current streak (ending today)
To compute `currentStreak`, we check from **today backward**:
- if today exists in set, streak starts at 1
- then check yesterday, day before yesterday… until missing

This gives a true “streak till today”.

---

## Why this is efficient
- Using a **hash set** makes checking “does this date exist?” O(1)
- Sorting is O(n log n) for longest streak
- Current streak check is O(k) where k = streak length

---

## Edge Cases
- No logs → current=0, longest=0
- Only one day log → current=1, longest=1
- Logs exist but none today → current=0, longest = computed longest

---

## Example Walkthrough

Logs:

["2026-02-10","2026-02-11","2026-02-13","2026-02-14","2026-02-15"]


Sorted unique:

10, 11, 13, 14, 15


Longest streak:
- 10→11 = 2
- 13→14→15 = 3  ✅ longest = 3

If today = 2026-02-15:
- 15 exists ✅
- 14 exists ✅
- 13 exists ✅
- 12 missing ❌
currentStreak = 3


---
