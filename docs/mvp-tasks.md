# ✅ Career Compass — MVP Task Board

This is the build checklist for MVP (Phase 1).  
Rule: Finish tasks in order. Small commits. No feature explosion.

---

## Milestone 0 — Repo + Docs (DONE)
- [x] Professional README
- [x] System design doc
- [x] Data models doc
- [x] Company dataset sample
- [x] Streak engine doc
- [x] Momentum engine doc
- [x] Reach meter engine doc
- [x] UI screens spec

---

## Milestone 1 — Frontend UI (Static with Dummy Data)
### Dashboard
- [ ] Create Dashboard layout (cards + spacing)
- [ ] Show dummy: streak, momentum, next actions, reach preview
- [ ] Add dark mode-friendly styling

### Daily Log
- [ ] Create Daily Log form UI
- [ ] Validate fields (numbers not negative)
- [ ] Show “Saved” toast (UI only)

### Company Reach Meter
- [ ] Create tabs: Ready / Close / Long Term
- [ ] Company cards UI
- [ ] Requirement breakdown UI

### Roadmap
- [ ] Track selector UI
- [ ] Topic checklist UI
- [ ] Locked prerequisites UI (dummy)

---

## Milestone 2 — Backend API (CRUD)
- [ ] Setup Node/Express server
- [ ] Connect MongoDB
- [ ] Create models: User, DailyLog, RoadmapProgress
- [ ] APIs:
  - [ ] POST /logs
  - [ ] GET /logs?range=14d
  - [ ] GET /user
  - [ ] PATCH /roadmap-progress

---

## Milestone 3 — Engine Integration (Real Logic)
- [ ] Implement streak calculation (based on `streak.engine.md`)
- [ ] Implement momentum score (based on `momentum.engine.md`)
- [ ] Implement company reach meter (based on `reach.engine.md`)
- [ ] Store ScoreSnapshot daily

---

## Milestone 4 — Polish (MVP Launch Ready)
- [ ] Add loading states + empty states
- [ ] Add basic error handling UI
- [ ] Add sample demo data seed script
- [ ] Update README with screenshots
- [ ] Deploy frontend (Vercel)
- [ ] Deploy backend (Render/Railway)

---

## Commit Discipline (Rules)
- One feature = one commit
- Commit messages like:
  - "Add dashboard layout"
  - "Add daily log form UI"
  - "Implement momentum score engine"
