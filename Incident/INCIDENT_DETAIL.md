

# 🛠️ Incident Detail API Documentation

This API manages **Incident Actions and Workflow Details**, including assignment, RCA submission, postponement, reassignment, and closure.

All endpoints are protected via **Laravel Sanctum**.

---

## 🗃️ Database Table: `incident_details`

| #  | Name                  | Type                | Attributes        | Null | Default | Extra           |
| -- | --------------------- | ------------------- | ----------------- | ---- | ------- | --------------- |
| 1  | id                    | bigint(20) unsigned | Primary Key       | No   | None    | AUTO\_INCREMENT |
| 2  | incident\_id          | bigint(20) unsigned | Foreign Key       | No   | None    |                 |
| 3  | assigned\_to          | bigint(20) unsigned | User ID           | Yes  | NULL    |                 |
| 4  | assigned\_department  | bigint(20) unsigned | Department ID     | Yes  | NULL    |                 |
| 5  | due\_at               | timestamp           | Due Date          | Yes  | NULL    |                 |
| 6  | is\_set\_assignee     | tinyint(1)          | 0 = Not Set       | No   | 0       |                 |
| 7  | category              | enum(...)           | Incident Category | No   | None    |                 |
| 8  | comment               | text                | General Comment   | Yes  | NULL    |                 |
| 9  | root\_cause\_analysis | text                | RCA Text          | Yes  | NULL    |                 |
| 10 | corrective\_action    | text                | Corrective Action | Yes  | NULL    |                 |
| 11 | preventive\_action    | text                | Preventive Action | Yes  | NULL    |                 |
| 12 | postponed\_to         | varchar(255)        | Postpone Date     | Yes  | NULL    |                 |
| 13 | postponed\_remark     | varchar(255)        | Postpone Reason   | Yes  | NULL    |                 |
| 14 | deleted\_at           | timestamp           | Soft Delete       | Yes  | NULL    |                 |
| 15 | created\_at           | timestamp           | Created At        | Yes  | NULL    |                 |
| 16 | updated\_at           | timestamp           | Updated At        | Yes  | NULL    |                 |

---

## 🌐 Base URL

```
https://your-api-domain.com/api/incident
```

---

## 🔐 Authentication

All endpoints require **Bearer Token (Laravel Sanctum)**.

### Headers:

```
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## 🔄 Incident Workflow Endpoints

### 1. Accept Incident

* **Endpoint:** `POST /api/incident/{id}/accept`
* **Validation:** *None*
* **Description:** Accept the incident to initiate follow-up actions.

---

### 2. Decline Incident

* **Endpoint:** `POST /api/incident/{id}/decline`
* **Validation:** *None*
* **Description:** Decline the incident for valid reasons.

---

### 3. Assign Incident

* **Endpoint:** `POST /api/incident/{id}/assign`
* **Description:** Assign the incident to a department and user.

#### Request Body:

| Field                | Type   | Required | Description                                                     |
| -------------------- | ------ | -------- | --------------------------------------------------------------- |
| assigned\_to         | int    | Yes      | User ID to assign the incident                                  |
| assigned\_department | int    | Yes      | Department ID                                                   |
| category             | enum   | Yes      | One of: `incident`, `accident`, `near_miss`, etc. (from config) |
| comment              | string | No       | Assignment comment (max 1000 chars)                             |
| assignment\_file     | file   | No       | PDF/Image file (max 10MB)                                       |

#### Validation:

* `category`: `required|in:[values from config('constant.incident_category')]`
* `assigned_to`: `required|exists:tenant.users,id`
* `assigned_department`: `required|exists:tenant.departments,id`
* `assignment_file`: `nullable|file|mimes:pdf,jpg,jpeg,png,gif|max:10240`

---

### 4. Submit RCA (Root Cause Analysis)

* **Endpoint:** `POST /api/incident/{id}/submit-rca`
* **Description:** Submit RCA or postpone the incident.

#### Request Body:

| Field                 | Type   | Required                        | Description                  |
| --------------------- | ------ | ------------------------------- | ---------------------------- |
| root\_cause\_analysis | string | Required unless postponed       | Analysis text (max 2000)     |
| corrective\_action    | string | Required unless postponed       | Corrective action text       |
| preventive\_action    | string | Required unless postponed       | Preventive action text       |
| rca\_file             | file   | No                              | Optional RCA file (10MB max) |
| postponed\_to         | int    | Optional                        | Postpone days (1–5)          |
| postponed\_remark     | string | Required if postponed\_to given | Reason for postponement      |

#### Validation:

* `required_without:postponed_to` applies to RCA fields
* `postponed_remark`: `required_with:postponed_to|string|max:500`
* `rca_file`: `nullable|file|mimes:pdf,jpg,jpeg,png,gif,doc,docx|max:10240`

---

### 5. Reassign Incident

* **Endpoint:** `POST /api/incident/{id}/reassign`
* **Description:** Reassign an incident (allowed **only once**).

#### Request Body:

| Field                | Type   | Required | Description                |
| -------------------- | ------ | -------- | -------------------------- |
| assigned\_to         | int    | Yes      | New User ID                |
| assigned\_department | int    | Yes      | New Department ID          |
| category             | enum   | Yes      | Incident category          |
| comment              | string | No       | Optional reassignment note |
| reassign\_file       | file   | No       | Optional file (max 10MB)   |

#### Business Rule:

* Only **one** reassignment is allowed per incident.
* Second attempt returns: `400 - "You can reassign only once."`

---

### 6. Close Incident

* **Endpoint:** `POST /api/incident/{id}/close`
* **Description:** Close an incident after resolution.

#### Request Body:

| Field         | Type   | Required | Description                             |
| ------------- | ------ | -------- | --------------------------------------- |
| final\_remark | string | Yes      | Final note (max 2000 characters)        |
| close\_file   | file   | No       | Optional supporting document (max 10MB) |

---

## 🧾 Status Lifecycle

| Status     | Description                       |
| ---------- | --------------------------------- |
| Accepted   | Incident accepted                 |
| Assigned   | Assigned to a department/user     |
| Postponed  | Postponed during RCA submission   |
| Reassigned | Reassigned to new user/department |
| Closed     | Incident resolved and closed      |

---

## ✅ Validation Summary

| Action     | Fields Required                                                                                        |
| ---------- | ------------------------------------------------------------------------------------------------------ |
| Accept     | —                                                                                                      |
| Decline    | —                                                                                                      |
| Assign     | `assigned_to`, `assigned_department`, `category`, `comment?`, `file?`                                  |
| Submit RCA | `root_cause_analysis`, `corrective_action`, `preventive_action` OR `postponed_to` + `postponed_remark` |
| Reassign   | Same as Assign + `reassign_file` (optional)                                                            |
| Close      | `final_remark`, `close_file?`                                                                          |

---

## ⚠️ Error Codes

| HTTP Code | Description             |
| --------- | ----------------------- |
| 200       | Success                 |
| 400       | Business Rule Violation |
| 401       | Unauthorized            |
| 404       | Incident Not Found      |
| 422       | Validation Error        |
| 500       | Internal Server Error   |

---

## 📌 Notes

* Sanctum token and `domain: psri.com` header are required.
* File size limits: **10MB** for assignment/reassign/close/RCA uploads.
* Categories are enum values: use `config('constant.incident_category')`.

---

## 📄 Summary Table

| Action     | Endpoint                      | Method | Description               |
| ---------- | ----------------------------- | ------ | ------------------------- |
| Accept     | /api/incident/{id}/accept     | POST   | Accept an incident        |
| Decline    | /api/incident/{id}/decline    | POST   | Decline an incident       |
| Assign     | /api/incident/{id}/assign     | POST   | Assign to user/department |
| Submit RCA | /api/incident/{id}/submit-rca | POST   | Submit RCA or postpone    |
| Reassign   | /api/incident/{id}/reassign   | POST   | Reassign the incident     |
| Close      | /api/incident/{id}/close      | POST   | Close the incident        |

---
