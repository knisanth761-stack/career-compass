# 🧠 Career Compass — System Design Overview

## 🎯 Objective

Career Compass is designed to help students move from random learning to structured, data-driven career preparation.

The system tracks user progress, analyzes performance patterns, and provides realistic career guidance based on actual skill development.

---

## 🏗 High-Level Architecture

User → Frontend UI → Backend API → Database → Logic Engine → Recommendation Output

---

## 🧩 Core System Components

### 1️⃣ Frontend Layer

Responsible for:

- Displaying roadmap and progress dashboard
- Accepting user input (daily progress logs)
- Showing streaks, scores, and recommendations

Technologies:
- Next.js
- Tailwind CSS
- TypeScript

---

### 2️⃣ Backend Layer

Responsible for:

- Handling API requests
- Managing user data
- Storing progress records
- Communicating with the logic engine

Technologies:
- Node.js
- Express.js
- MongoDB

---

### 3️⃣ Logic Engine (Core Intelligence)

This is the heart of the system.

It performs:

- Streak calculation
- Momentum scoring
- Company eligibility analysis
- Recommendation generation

---

## ⚙️ Key Algorithms

### 🔥 Streak Engine

Calculates consecutive days of activity.

Input:
- List of dates when user logged progress

Output:
- Current streak
- Longest streak

---

### 📈 Momentum Score Engine

Measures consistency and effort.

Factors:
- Daily activity
- Problem difficulty
- Time spent learning
- Topic coverage

Output:
- Score between 0–100

---

### 🎯 Company Eligibility Engine

Determines which companies a user can realistically target.

Inputs:
- Problems solved
- Topics completed
- Project experience
- Consistency score

Outputs:
- Companies categorized into:
  - Ready now
  - Close to reach
  - Long-term goals

---

## 🗄 Data Flow

1. User logs daily progress
2. Data stored in database
3. Logic engine processes metrics
4. Scores and recommendations generated
5. Results displayed on dashboard

---

## 🚀 Future Scalability Considerations

- Support thousands of concurrent users
- Add AI-based recommendation system
- Integrate external learning platforms
- Provide predictive career timelines

---

## 🧠 Engineering Philosophy

The system focuses on:

- Realistic motivation over generic encouragement
- Data-driven decision making
- Transparency in recommendations
- Continuous improvement tracking

---

## 📌 Summary

Career Compass is not just a tracker — it is a decision-support system that transforms career preparation into a measurable and structured journey.
