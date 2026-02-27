# Minimal Data Model

The system stores minimal but sufficient data to support decision making and auditability.

---

## 1. notifications

Stores every processed notification.

- id (UUID)
- user_id
- event_type
- message
- channel (push/email/SMS/in-app)
- priority_hint
- timestamp
- decision (NOW / LATER / NEVER)
- rule_score
- ai_score
- final_score
- reason
- created_at

---

## 2. user_notification_stats

Stores user-level frequency data.

- user_id
- count_last_hour
- count_last_day
- last_sent_time
- last_updated

Used to reduce alert fatigue.

---

## 3. suppression_records

Stores duplicate tracking records.

- hash_key
- user_id
- created_at
- expires_at

Used for exact and near-duplicate detection.

---

## 4. scheduled_notifications

Stores deferred notifications.

- id
- user_id
- scheduled_time
- status (pending/sent/cancelled)
- original_event_id

---

## 5. audit_logs

Stores decision trace for explainability.

- notification_id
- decision
- explanation
- rule_details
- model_latency
- timestamp

This ensures system transparency and compliance.
