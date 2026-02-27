# Decision Logic Design

## Overview

The Notification Prioritization Engine follows a multi-stage pipeline to classify each notification into:

- NOW (Send Immediately)
- LATER (Defer / Schedule)
- NEVER (Suppress)

The goal is to balance urgency, user experience, and system reliability.

---

## Stage 1: Immediate Rejection (NEVER)

A notification is immediately suppressed if:

- `expires_at` < current time
- Exact duplicate is detected
- Near duplicate detected within 5 minutes
- User exceeded daily hard cap
- Promotional notification during quiet hours
- Message is identified as spam or invalid

If any condition matches:
Decision = NEVER  
Reason is logged for auditability.

---

## Stage 2: Priority Scoring

If not rejected, a priority score (0–100) is calculated.

### Rule-Based Scoring

| Condition | Score Impact |
|------------|-------------|
| priority_hint = high | +40 |
| System alert | +30 |
| Transactional event | +25 |
| User active in app | +10 |
| Promotional | -20 |
| 5+ notifications in last hour | -30 |
| Outside active hours | -15 |

---

## AI Score Adjustment

An AI model adjusts the score based on:

- Message importance
- Historical engagement rate
- Context relevance
- User behavior patterns

Final Score = Rule Score + AI Adjustment

---

## Stage 3: Final Classification

If score ≥ 70 → NOW  
If score between 40–69 → LATER  
If score < 40 → NEVER  

Time-sensitive system alerts override suppression.

Important notifications are never silently dropped.

---

## Explainability

Each decision stores:

- Rule score
- AI score
- Final score
- Decision reason
- Timestamp

This ensures auditability and transparency.
