
# Observation Detail API Documentation

This API manages **Observation Actions and Details** such as assignment, RCA submission, postponement, reassignment, and closure.

All endpoints are protected via **Laravel Sanctum**.

---

## Database Table Structure

The `observation_details` table stores workflow-related information for observations.

| #   | Name                | Type                | Attributes     | Null | Default | Extra          |
|-----|--------------------|---------------------|----------------|------|---------|----------------|
| 1   | id                  | bigint(20) unsigned | Primary Key    | No   | None    | AUTO_INCREMENT |
| 2   | observation_id      | bigint(20) unsigned | Foreign Key    | No   | None    |                |
| 3   | assigned_to         | bigint(20) unsigned | User ID        | Yes  | NULL    |                |
| 4   | assigned_department | bigint(20) unsigned | Department ID  | Yes  | NULL    |                |
| 5   | due_at              | timestamp           | Due Date       | Yes  | NULL    |                |
| 6   | is_set_assignee     | tinyint(1)          | 0 = Not Set    | No   | 0       |                |
| 7   | qm_remarks          | text                | Remarks        | Yes  | NULL    |                |
| 8   | root_cause_analysis | text                | RCA Text       | Yes  | NULL    |                |
| 9   | corrective_action   | text                | Action         | Yes  | NULL    |                |
| 10  | preventive_action   | text                | Prevention     | Yes  | NULL    |                |
| 11  | postponed_to        | varchar(255)        | Postpone Date  | Yes  | NULL    |                |
| 12  | postponed_remark    | varchar(255)        | Postpone Reason| Yes  | NULL    |                |
| 13  | deleted_at          | timestamp           | Soft Delete    | Yes  | NULL    |                |
| 14  | created_at          | timestamp           | Created At     | Yes  | NULL    |                |
| 15  | updated_at          | timestamp           | Updated At     | Yes  | NULL    |                |

---

## Base URL

```
https://your-api-domain.com/api/observations
```

Replace `your-api-domain.com` with your actual API domain.

---

## Authentication

All endpoints require **Bearer Token (Laravel Sanctum)**.

### Required Headers:

```
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## Observation Workflow Endpoints

### 1. Accept Observation

- **Endpoint:** `POST /api/observations/{id}/accept`
- **Description:** Accept an observation to initiate action workflow.

---

### 2. Assign Observation

- **Endpoint:** `POST /api/observations/{id}/assign`
- **Description:** Assign observation to a user and department.

#### Request Body:

| Field               | Type   | Required | Description                        |
|--------------------|--------|----------|------------------------------------|
| assigned_to         | int    | Yes      | User ID to assign the observation |
| assigned_department | int    | Yes      | Department ID                      |
| allocation_time     | string | Yes      | Time limit (immediate, 2_days, 5_days, 7_days, 15_days) |
| qm_remarks          | string | Yes      | Quality Manager remarks           |
| assignment_file     | file   | No       | PDF or Image (jpg,jpeg,png,gif) max 10MB |

#### Validation Rules:

- `qm_remarks` : `required|string|max:1000`
- `allocation_time` : `required|in:immediate,2_days,5_days,7_days,15_days`
- `assigned_department` : `required|exists:tenant.departments,id`
- `assigned_to` : `required|exists:tenant.users,id`
- `assignment_file` : `nullable|file|mimes:pdf,jpg,jpeg,png,gif|max:10240`

---

### 3. Submit RCA (Root Cause Analysis)

- **Endpoint:** `POST /api/observations/{id}/rca`
- **Description:** Submit Root Cause Analysis and action plans. Users may also postpone.

#### Request Body:

| Field                | Type   | Required | Description                          |
|---------------------|--------|----------|--------------------------------------|
| root_cause_analysis  | string | Required if not postponed | Description of root cause |
| corrective_action    | string | Required if not postponed | Immediate corrective action |
| preventive_action    | string | Required if not postponed | Preventive action plan |
| postpone_to          | int    | Optional | Postpone duration (1-5 days) |
| postpone_reason      | string | Required if `postpone_to` is present | Reason for postponement |
| rca_file              | file   | No       | File (pdf, jpg, jpeg, png, gif, doc, docx), max 10MB |

#### Validation:

- `root_cause_analysis`, `corrective_action`, `preventive_action` are **required unless postponed**.
- `postpone_to`: `nullable|integer|in:1,2,3,4,5`
- `postpone_reason`: `required_with:postpone_to|string|max:500`
- `rca_file`: `nullable|file|mimes:pdf,jpg,jpeg,png,gif,doc,docx|max:10240`

#### Example Request:

```json
{
    "postpone_to": 3,
    "postpone_reason": "Need more time to gather additional information."
}
```

#### Example Response:

```json
{
    "success": true,
    "status": 200,
    "message": "RCA submitted successfully!",
    "data": {
        "id": 8,
        "observation_no": "OBS10008",
        "status": "Postponed"
    }
}
```

---

### 4. Reassign Observation

- **Endpoint:** `POST /api/observations/{id}/reassign`
- **Description:** Reassign observation (allowed only once)

#### Request Body:

Same as **Assign Observation**, but with `reassign_file` (optional)

- `reassign_file`: `nullable|file|mimes:pdf,jpg,jpeg,png,gif|max:10240`

#### Business Rule:

| Condition           | Result                     |
|-------------------|----------------------------|
| First reassignment | Allowed                    |
| Second reassignment| 400 Error: "You can reassign only once." |

---

### 5. Close Observation

- **Endpoint:** `POST /api/observations/{id}/close`
- **Description:** Close the observation after completion.

#### Request Body:

| Field         | Type   | Required | Description               |
|---------------|--------|----------|---------------------------|
| final_remark  | string | Yes      | Final remark (max 2000)    |
| close_remark  | string | Yes      | Close note (max 2000)      |
| close_file    | file   | No       | File (pdf, jpg, jpeg, png, gif, doc, docx), max 10MB |

---

## Status Workflow

| Status      | Description                          |
|-------------|--------------------------------------|
| Accepted    | Observation accepted for action      |
| Assigned    | Assigned to user/department          |
| Postponed   | Action postponed via RCA             |
| Reassigned  | Observation reassigned               |
| Closed      | Observation workflow completed       |

---

## Validation Summary

| Action        | Required Fields                                                                 |
|---------------|--------------------------------------------------------------------------------|
| Accept        | No additional fields                                                            |
| Assign        | `assigned_to`, `assigned_department`, `allocation_time`, `qm_remarks`, `file` (optional) |
| RCA           | `root_cause_analysis`, `corrective_action`, `preventive_action`, OR `postpone_to` and `postpone_reason` |
| Reassign      | Same as Assign but with `reassign_file` (optional)                             |
| Close         | `final_remark`, `close_remark`, `close_file` (optional)                       |

---

## Error Codes

| HTTP Code | Description                           |
|-----------|---------------------------------------|
| 200       | Success                               |
| 400       | Business Rule Violation               |
| 401       | Unauthorized                          |
| 404       | Observation not found                 |
| 422       | Validation Error                      |
| 500       | Internal Server Error                 |

---

## Notes

- All routes require **Sanctum token** authentication.
- `domain: psri.com` header is mandatory.
- File uploads are optional for assignment, RCA, reassignment, and closure.
- Maximum file sizes: 10MB for assignment/reassign/close files; 2MB for attachments.
- Status updates and business rules are enforced at API level.

---

## Summary Table

| Action         | Endpoint                        | Method | Description                  |
|----------------|---------------------------------|--------|------------------------------|
| Accept         | /api/observations/{id}/accept   | POST   | Accept observation           |
| Assign         | /api/observations/{id}/assign   | POST   | Assign to user/department    |
| RCA Submission | /api/observations/{id}/rca      | POST   | Submit RCA & postpone        |
| Reassign       | /api/observations/{id}/reassign | POST   | Reassign observation         |
| Close          | /api/observations/{id}/close    | POST   | Close observation            |

---

## Contact

For any support or queries, contact the **Backend Development Team**.
