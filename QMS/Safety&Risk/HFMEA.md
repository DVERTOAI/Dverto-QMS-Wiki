

# HFMEA API Documentation

This API allows managing **HFMEA records**, which track **failure modes, causes, controls, and actions** for healthcare processes.
Admin/QMS roles can create, update, and close HFMEA records.

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

## 🗄 Database Table: `hfmeas`

| #  | Column                   | Type            | Null | Default | Extra                              |
| -- | ------------------------ | --------------- | ---- | ------- | ---------------------------------- |
| 1  | id                       | bigint UNSIGNED | No   | None    | AUTO_INCREMENT                     |
| 2  | department_id            | bigint UNSIGNED | No   | None    |                                    |
| 3  | failure_mode             | text            | No   | None    |                                    |
| 4  | potential_causes         | text            | Yes  | NULL    |                                    |
| 5  | severity                 | integer         | Yes  | NULL    |                                    |
| 6  | probability              | integer         | Yes  | NULL    |                                    |
| 7  | haz_score                | integer         | Yes  | NULL    |                                    |
| 8  | single_point_weakness    | varchar(50)     | Yes  | NULL    | (`Yes`/`No`)                       |
| 9  | existing_control_measure | varchar(50)     | Yes  | NULL    | (`Yes`/`No`)                       |
| 10 | detectability            | varchar(50)     | Yes  | NULL    | (`Yes`/`No`)                       |
| 11 | proceed                  | varchar(50)     | Yes  | NULL    |                                    |
| 12 | action_type              | varchar(50)     | Yes  | NULL    | (`Control`/`Eliminate`/`Mitigate`) |
| 13 | actions_or_rationale     | text            | Yes  | NULL    |                                    |
| 14 | outcome_measures         | text            | Yes  | NULL    |                                    |
| 15 | person_responsible       | varchar(100)    | Yes  | NULL    |                                    |
| 16 | status                   | varchar(20)     | Yes  | NULL    | (`open`, `submitted`, `closed`)    |
| 17 | created_at               | timestamp       | Yes  | NULL    |                                    |
| 18 | updated_at               | timestamp       | Yes  | NULL    |                                    |

**Example Records:**

| id | department_id | failure_mode                           | potential_causes          | severity | probability | haz_score | single_point_weakness | existing_control_measure | detectability | proceed | action_type | actions_or_rationale                     | outcome_measures               | person_responsible | status    | created_at          | updated_at          |
| -- | ------------- | -------------------------------------- | ------------------------- | -------- | ----------- | --------- | --------------------- | ------------------------ | ------------- | ------- | ----------- | ---------------------------------------- | ------------------------------ | ------------------ | --------- | ------------------- | ------------------- |
| 1  | 2             | Equipment malfunction during operation | Poor maintenance schedule | 4        | 3           | 12        | Yes                   | No                       | Yes           | No      | Control     | Implement regular preventive maintenance | Reduced downtime and incidents | John Smith         | open      | 2025-10-09 16:43:32 | 2025-10-09 20:03:05 |
| 2  | 2             | Equipment malfunction during operation | Poor maintenance schedule | 4        | 3           | 12        | Yes                   | No                       | Yes           | No      | Control     | Implement regular preventive maintenance | Reduced downtime and incidents | John Smith         | submitted | 2025-10-09 17:04:22 | 2025-10-09 17:07:31 |

---

## 1. List HFMEA Records (Admin/QMS)

**GET** `/hfmea`

**Query Parameters (Optional):**

* `department_id` - Filter by department
* `status` - Filter by HFMEA status (`open`, `submitted`, `closed`)
* `search` - Full text search across failure_mode, potential_causes, actions_or_rationale, person_responsible
* `sort_by` - Column to sort by (default `id`)
* `sort_order` - `asc` or `desc` (default `desc`)

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "HFMEA records fetched successfully.",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "department_id": 2,
        "failure_mode": "Equipment malfunction during operation",
        "potential_causes": "Poor maintenance schedule",
        "severity": 4,
        "probability": 3,
        "haz_score": 12,
        "single_point_weakness": "Yes",
        "existing_control_measure": "No",
        "detectability": "Yes",
        "proceed": "No",
        "action_type": "Control",
        "actions_or_rationale": "Implement regular preventive maintenance",
        "outcome_measures": "Reduced downtime and incidents",
        "person_responsible": "John Smith",
        "status": "open",
        "created_at": "2025-10-09T16:43:32.000000Z",
        "updated_at": "2025-10-09T20:03:05.000000Z",
        "department": {
          "id": 2,
          "name": "ICU",
          "status": "active"
        }
      }
    ],
    "total": 2
  }
}
```

---

## 2. Create HFMEA (Admin/QMS)

**POST** `/hfmea`

**Payload Example:**

```json
{
  "department_id": 2,
  "failure_mode": "Equipment malfunction during operation",
  "potential_causes": "Poor maintenance schedule",
  "severity": 4,
  "probability": 3,
  "haz_score": 12,
  "single_point_weakness": "Yes",
  "existing_control_measure": "No",
  "detectability": "Yes",
  "proceed": "No",
  "action_type": "Control",
  "actions_or_rationale": "Implement regular preventive maintenance",
  "outcome_measures": "Reduced downtime and incidents",
  "person_responsible": "John Smith",
  "status": "open"
}
```

---

## 3. Update HFMEA (Admin/QMS)

**PUT** `/hfmea/{id}`

**Payload Example:**

```json
{
  "department_id": 2,
  "failure_mode": "Equipment malfunction during operation",
  "potential_causes": "Poor maintenance schedule",
  "severity": 4,
  "probability": 3,
  "haz_score": 12,
  "single_point_weakness": "Yes",
  "existing_control_measure": "No",
  "detectability": "Yes",
  "proceed": "No",
  "action_type": "Control",
  "actions_or_rationale": "Implement regular preventive maintenance",
  "outcome_measures": "Reduced downtime and incidents",
  "person_responsible": "John Smith",
  "status": "submitted"
}
```

---

## 4. Close HFMEA (Admin/QMS)

**PATCH** `/hfmea/closed/{id}`

**Payload Example:**

```json
{
  "status": "closed"
}
```

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "HFMEA status updated successfully",
  "data": {
    "id": 2,
    "department_id": 2,
    "failure_mode": "Equipment malfunction during operation",
    "status": "closed",
    "updated_at": "2025-10-09T17:07:31.000000Z"
  }
}
```
