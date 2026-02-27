# Notification-prioritization-engine
# 📌 Notification Prioritization Engine  
### Cyepro AI-Native Solution Crafting Test  
**Candidate:** Praveen Kumar

---

# 1️⃣ Problem Summary

Modern applications generate notifications from multiple services such as:

- Messages
- Reminders
- System alerts
- Updates
- Promotions
- Transactional events

Users often experience:

- Too many notifications
- Duplicate alerts
- Poor timing
- Important notifications missed
- Alert fatigue

The goal is to design a **Notification Prioritization Engine** that intelligently decides for each incoming notification:

- ✅ **NOW** → Send immediately  
- ⏳ **LATER** → Defer / Schedule  
- ❌ **NEVER** → Suppress  

The system must be:

- Scalable
- Low latency
- Explainable
- Configurable
- Reliable
- Fail-safe

---

# 2️⃣ High-Level Architecture
Incoming Event

↓

API Gateway

↓

Notification Processing Service

↓

┌─────────────────────────────────────┐

│ 1. Deduplication Engine │

│ 2. Rule Engine (Configurable) │

│ 3. AI Priority Scorer │

│ 4. Fatigue Controller │

└─────────────────────────────────────┘

↓
Decision Engine (NOW / LATER / NEVER)
↓
Send Now | Schedule | Suppress
↓
Audit & Logging Service


### Core Components

- **API Gateway** – Receives notification events
- **Deduplication Engine** – Prevents exact & near duplicates
- **Rule Engine** – Applies configurable business logic
- **AI Priority Scorer** – Adjusts importance dynamically
- **Fatigue Controller** – Prevents over-notification
- **Scheduler** – Handles deferred notifications
- **Audit Logger** – Stores decision explanations

---

# 3️⃣ Decision Logic Overview

The engine follows a 3-stage pipeline:

---

## Stage 1: Immediate Suppression (NEVER)

Suppress notification if:

- Expired
- Exact duplicate
- Near duplicate
- Daily/hourly limit exceeded
- Promotional during quiet hours

---

## Stage 2: Priority Scoring

Each notification gets a score (0–100).

### Rule-Based Scoring

| Condition | Score |
|------------|--------|
| priority_hint = high | +40 |
| System alert | +30 |
| Transactional | +25 |
| Promotional | -20 |
| 5+ notifications in last hour | -30 |
| Outside active hours | -15 |

### AI Score Adjustment

AI considers:

- Message importance
- User engagement history
- Context relevance

Final Score = Rule Score + AI Adjustment

---

## Stage 3: Classification

- Score ≥ 70 → NOW  
- Score 40–69 → LATER  
- Score < 40 → NEVER  

Critical alerts override suppression rules.

All decisions are logged with explanation.

---

# 4️⃣ Duplicate Prevention Strategy

## Exact Duplicate

- Use `dedupe_key`
- OR generate hash of (user_id + message + event_type)
- Store in Redis for 10 minutes

## Near Duplicate

- Fuzzy string matching
- Embedding similarity (cosine similarity > 90%)
- Suppress if similar within short time window

---

# 5️⃣ Alert Fatigue Strategy

To reduce overload:

- Max 5 notifications per hour
- Max 20 per day
- Max 2 promotions per day
- Quiet hours: 10 PM – 7 AM
- Batch low-priority notifications into digest

---

# 6️⃣ Human Configurable Rules

Rules stored in configuration service:

Example:
{
"max_per_hour": 5,
"max_per_day": 20,
"quiet_hours": "22:00-07:00",
"promotion_limit": 2
}


Rules can be updated without code deployment.

---

# 7️⃣ Fallback Strategy (Fail-Safe Design)

If AI service is slow or unavailable:

- Switch to rule-only mode
- Always allow critical system alerts
- Never silently drop important notifications
- Queue uncertain events for retry

System principle:

> Fail safe, not fail silent.

---

# 8️⃣ Data Model Summary

Core tables:

- notifications
- user_notification_stats
- suppression_records
- scheduled_notifications
- audit_logs

Each decision is explainable and traceable.

---

# 9️⃣ API Overview

- POST /notifications/evaluate
- GET /users/{id}/history
- POST /rules/update
- GET /audit/{notification_id}

APIs return decision, score, and reason.

---

# 🔟 Metrics & Monitoring

### System Metrics

- Decision distribution (NOW/LATER/NEVER)
- AI latency
- Duplicate suppression rate
- Throughput (events per minute)

### Product Metrics

- Notification open rate
- Engagement rate
- Critical alert miss rate
- User churn impact

Alerts triggered if:

- AI latency spikes
- Suppression rate abnormal
- Critical alerts drop unexpectedly

---

# 1️⃣1️⃣ Design Tradeoffs

- Hybrid Rules + AI approach
- Simplicity over over-engineering
- Explainability prioritized over black-box models
- Redis for low-latency deduplication
- Important alerts never silently lost

---

# 1️⃣2️⃣ Scalability Considerations

- Stateless processing services
- Redis for fast duplicate lookup
- Horizontal scaling of API layer
- Asynchronous scheduling for deferred notifications
- Monitoring and observability built-in

---

# 1️⃣3️⃣ Tools Used

- ChatGPT (for structuring ideas)
- Draw.io (for architecture diagram)
- GitHub (version control)

All architectural decisions were manually reviewed and structured.

---

# 🎥 Walkthrough Video

The walkthrough explains:

- Problem understanding
- Architecture
- Decision logic
- Duplicate handling
- Fatigue strategy
- Fallback design
- Metrics

Duration: ~7 minutes

---

# ✅ Conclusion

This solution balances:

- User experience
- System reliability
- Business flexibility
- AI-enhanced intelligence
- Explainability and auditability

The system is scalable, configurable, and production-ready with incremental improvements.

---
