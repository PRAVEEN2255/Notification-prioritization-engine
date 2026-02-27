# API Design

The Notification Prioritization Engine exposes REST APIs for evaluation, rule management, and audit access.

---

## 1. POST /notifications/evaluate

Evaluates an incoming notification.

### Request Body

{
  "user_id": "123",
  "event_type": "system_alert",
  "message": "Your payment failed",
  "priority_hint": "high",
  "timestamp": "2025-02-20T10:00:00Z",
  "channel": "push",
  "dedupe_key": "payment_fail_123"
}

### Response

{
  "decision": "NOW",
  "reason": "High priority system alert",
  "final_score": 85
}

---

## 2. GET /users/{id}/history

Returns recent notification stats.

### Response

{
  "user_id": "123",
  "count_last_hour": 3,
  "count_last_day": 12,
  "last_sent_time": "2025-02-20T09:45:00Z"
}

---

## 3. POST /rules/update

Allows admin to update configurable rules.

### Example Request

{
  "max_per_hour": 5,
  "max_per_day": 20,
  "quiet_hours": "22:00-07:00",
  "promotion_limit": 2
}

Response:
{
  "status": "Rules updated successfully"
}

---

## 4. GET /audit/{notification_id}

Returns full decision explanation.

### Response

{
  "notification_id": "abc123",
  "decision": "LATER",
  "rule_score": 45,
  "ai_score": 5,
  "final_score": 50,
  "reason": "Moderate priority during high traffic period"
}
