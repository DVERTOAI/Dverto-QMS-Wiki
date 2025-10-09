# HIRA (Hazard Identification and Risk Assessment) API Documentation

This API allows managing **HIRA records** for departments.
Admin/QMS roles can create, update, and close HIRA records.
Public users can view assigned HIRA tabs.

---

## Base URL

```
{{site_url}}/api/safety-risk
```

## Headers (Admin/QMS)

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## 🗄 Database Table: `hiras`

| #  | Column                        | Type            | Null | Default | Extra                           |
| -- | ----------------------------- | --------------- | ---- | ------- | ------------------------------- |
| 1  | id                            | bigint UNSIGNED | No   | None    | AUTO_INCREMENT                  |
| 2  | department_id                 | bigint UNSIGNED | No   | None    |                                 |
| 3  | activity                      | text            | No   | None    |                                 |
| 4  | hazards                       | text            | Yes  | NULL    |                                 |
| 5  | type_of_activity              | varchar(50)     | Yes  | NULL    |                                 |
| 6  | impact                        | text            | Yes  | NULL    |                                 |
| 7  | likelihood_occurrence         | integer         | Yes  | NULL    |                                 |
| 8  | severity                      | integer         | Yes  | NULL    |                                 |
| 9  | total_risk                    | integer         | Yes  | NULL    |                                 |
| 10 | risk_status                   | text            | Yes  | NULL    |                                 |
| 11 | controls_interventions        | text            | Yes  | NULL    |                                 |
| 12 | post_intervention_probability | integer         | Yes  | NULL    |                                 |
| 13 | post_intervention_severity    | integer         | Yes  | NULL    |                                 |
| 14 | post_intervention_total_risk  | integer         | Yes  | NULL    |                                 |
| 15 | post_risk_status              | text            | Yes  | NULL    |                                 |
| 16 | status                        | varchar(20)     | Yes  | NULL    | (`open`, `submitted`, `closed`) |
| 17 | created_at                    | timestamp       | Yes  | NULL    |                                 |
| 18 | updated_at                    | timestamp       | Yes  | NULL    |                                 |

**Example Records:**

| id | department_id | activity                 | hazards                     | type_of_activity | impact                 | likelihood_occurrence | severity | total_risk | risk_status           | controls_interventions | post_intervention_probability | post_intervention_severity | post_intervention_total_risk | post_risk_status          | status    | created_at          | updated_at          |
| -- | ------------- | ------------------------ | --------------------------- | ---------------- | ---------------------- | --------------------- | -------- | ---------- | --------------------- | ---------------------- | ----------------------------- | -------------------------- | ---------------------------- | ------------------------- | --------- | ------------------- | ------------------- |
| 2  | 2             | Chemical handling in lab | Exposure to toxic chemicals | Routine          | Health and safety risk | 5                     | 7        | 35         | Score of more than 10 | Use PPE, follow SOPs   | 2                             | 3                          | 6                            | Criteria for Unacceptable | submitted | 2025-10-09 10:29:48 | 2025-10-09 10:45:21 |
| 4  | 1             | Chemical handling in lab | Exposure to toxic chemicals | Routine          | Health and safety risk | 5                     | 7        | 35         | Score of more than 10 | Use PPE, follow SOPs   | 2                             | 3                          | 6                            | Criteria for Unacceptable | closed    | 2025-10-09 12:00:43 | 2025-10-09 12:14:05 |

---

## 1. List HIRA Records (Admin/QMS)

**GET** `/safety-risk/hira`

**Query Parameters (Optional):**

* `department_id` - Filter by department
* `impact` - Filter by impact
* `status` - Filter by HIRA status (`open`, `submitted`, `closed`)
* `search` - Full text search across activity, hazards, etc.
* `sort_by` - Column to sort by (default `id`)
* `sort_order` - `asc` or `desc` (default `desc`)

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "HIRA records fetched successfully.",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 4,
        "department_id": 1,
        "activity": "Chemical handling in lab",
        "hazards": "Exposure to toxic chemicals",
        "type_of_activity": "Routine",
        "impact": "Health and safety risk",
        "likelihood_occurrence": 5,
        "severity": 7,
        "total_risk": 35,
        "risk_status": "Score of more than 10",
        "controls_interventions": "Use PPE, follow SOPs",
        "post_intervention_probability": 2,
        "post_intervention_severity": 3,
        "post_intervention_total_risk": 6,
        "post_risk_status": "Criteria for Unacceptable",
        "status": "closed",
        "created_at": "2025-10-09T06:30:43.000000Z",
        "updated_at": "2025-10-09T06:44:05.000000Z",
        "department": {
          "id": 1,
          "name": "Floor",
          "status": "active"
        }
      }
    ],
    "total": 2
  }
}
```

---

## 2. Create HIRA (Admin/QMS)

**POST** `/safety-risk/hira`

**Payload Example:**

```json
{
  "department_id": 1,
  "activity": "Chemical handling in lab",
  "hazards": "Exposure to toxic chemicals",
  "type_of_activity": "Routine",
  "impact": "Health and safety risk",
  "likelihood_occurrence": 5,
  "severity": 7,
  "total_risk": 35,
  "risk_status": "Score of more than 10",
  "controls_interventions": "Use PPE, follow SOPs",
  "status": "open"
}
```

---

## 3. Update HIRA (Admin/QMS)

**PUT** `/safety-risk/hira/{id}`

**Payload Example:**

```json
{
  "department_id": 1,
  "activity": "Chemical handling in lab",
  "hazards": "Exposure to toxic chemicals",
  "type_of_activity": "Routine",
  "impact": "Health and safety risk",
  "likelihood_occurrence": 5,
  "severity": 7,
  "total_risk": 35,
  "risk_status": "Score of more than 10",
  "controls_interventions": "Use PPE, follow SOPs",
  "post_intervention_probability": 2,
  "post_intervention_severity": 3,
  "post_intervention_total_risk": 6,
  "post_risk_status": "Criteria for Unacceptable",
  "status": "submitted"
}
```

---

## 4. Close HIRA (Admin/QMS)

**PATCH** `/safety-risk/hira/closed/{id}`

**Payload Example:**

```json
{
  "status": "closed"
}
```
