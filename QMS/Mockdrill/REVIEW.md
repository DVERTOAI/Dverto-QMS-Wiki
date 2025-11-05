# 🧯 Mock Drill Reviews API Documentation

The **Mock Drill Reviews API** enables coordinators, QMS reviewers, and approvers to manage and review mock drill performance, compliance, and remarks efficiently.

**Base URL:**
`{tenant_domain}/api/mockdrill/reviews`

**Authentication:**
Bearer Token (Laravel Sanctum)

---

## 🧩 Headers

All requests must include:

```
Authorization: Bearer {your_token}
Accept: application/json
Content-Type: application/json
X-Tenant-ID: {tenant_id}
```

---

## 📘 1. Get All Reviews

**Endpoint:**
`GET /api/mockdrill/reviews`

**Description:**
Retrieve all mock drill review records with checklist, color code, and coordinator details.

**Query Parameters (optional):**

| Parameter     | Type                 | Description             |
| ------------- | -------------------- | ----------------------- |
| color_code_id | integer              | Filter by color code    |
| scheduled_at  | string (Y-m-d H:i:s) | Filter by schedule date |

**cURL Example:**

```bash
curl -X GET "https://your-domain.com/api/mockdrill/reviews?color_code_id=1&scheduled_at=2025-01-15%2010:00:00" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Tenant-ID: your-tenant-id" \
  -H "Accept: application/json"
```

**Response (200 OK):**

```json
{
  "success": true,
  "data": [
    {
      "id": 11,
      "checklist_id": 1,
      "coordinator_id": 1,
      "color_code_id": 2,
      "scheduled_at": "2025-01-15 10:00:00",
      "is_compliance": 1,
      "review": "Team assembled within 5 minutes",
      "created_at": "2025-10-07T15:02:31.000000Z",
      "updated_at": "2025-10-07T15:02:31.000000Z"
    },
    {
      "id": 12,
      "checklist_id": 2,
      "coordinator_id": 1,
      "color_code_id": 2,
      "scheduled_at": "2025-01-15 10:00:00",
      "is_compliance": 0,
      "review": "All equipment functional",
      "created_at": "2025-10-07T15:02:31.000000Z",
      "updated_at": "2025-10-07T15:02:31.000000Z"
    }
  ]
}
```

---

## 🧾 2. Create Review (Submit)

**Endpoint:**
`POST /api/mockdrill/reviews`

**Description:**
Submit mock drill review results for one or more checklists.

**Request Body (multipart/form-data):**

```json
{
  "color_code_id": 1,
  "scheduled_at": "2025-01-15 10:00:00",
  "checklist": {
    "1": 1,
    "2": 0
  },
  "review": {
    "1": "Team assembled within 5 minutes",
    "2": "Equipment not functional"
  },
  "attachment": {
    "1": "(file)",
    "2": "(file)"
  }
}
```

**cURL Example:**

```bash
curl -X POST "https://your-domain.com/api/mockdrill/reviews" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-Tenant-ID: your-tenant-id" \
  -F "color_code_id=1" \
  -F "scheduled_at=2025-01-15 10:00:00" \
  -F "checklist[1]=1" \
  -F "checklist[2]=0" \
  -F "review[1]=Team assembled within 5 minutes" \
  -F "review[2]=Equipment not functional" \
  -F "attachment[1]=@/path/to/file1.jpg" \
  -F "attachment[2]=@/path/to/file2.pdf"
```

**Response (201 Created):**

```json
{
  "success": true,
  "message": "Review submitted successfully.",
  "total_checklists": 2
}
```

---

## 📋 3. Get Reviews by Color Code & Schedule

**Endpoint:**
`GET /api/mockdrill/reviews/list`

**Description:**
Retrieve reviews for a specific color code and scheduled drill date.

**Query Parameters:**

| Parameter     | Type                   | Required | Description         |
| ------------- | ---------------------- | -------- | ------------------- |
| color_code_id | integer                | ✅        | Color code ID       |
| scheduled_at  | datetime (Y-m-d H:i:s) | ✅        | Drill schedule time |

**Response (200 OK):**

```json
{
  "success": true,
  "reviews": [
    {
      "checklist": "Emergency Response Team Assembly",
      "is_compliance": 1,
      "review": "Team assembled within 5 minutes"
    },
    {
      "checklist": "Equipment Check",
      "is_compliance": 0,
      "review": "All equipment functional"
    }
  ]
}
```

---

## 🧩 4. Get Remarks (QMS / Approver)

**Endpoint:**
`GET /api/mockdrill/reviews/remarks`

**Description:**
Retrieve QM and approver remarks for a given color code and drill schedule.

**Query Parameters:**

| Parameter     | Type     | Required | Description    |
| ------------- | -------- | -------- | -------------- |
| color_code_id | integer  | ✅        | Color code ID  |
| scheduled_at  | datetime | ✅        | Scheduled date |

**Response (200 OK):**

```json
{
  "success": true,
  "remarks": "Overall performance was satisfactory",
  "approver_remarks": "Approved with minor recommendations",
  "status": "approved"
}
```

---

## 🧠 5. Save or Update Remarks

**Endpoint:**
`POST /api/mockdrill/reviews/remarks`

**Description:**
Save or update remarks from QM or Approver.

**Request Body (JSON):**

```json
{
  "color_code_id": 1,
  "scheduled_at": "2025-01-15 10:00:00",
  "qm_remarks": "Overall performance was satisfactory",
  "approver_remarks": "Approved with minor recommendations",
  "status": "approved"
}
```

**cURL Example:**

```bash
curl -X POST "https://your-domain.com/api/mockdrill/reviews/remarks" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "X-Tenant-ID: your-tenant-id" \
  -d '{
    "color_code_id": 1,
    "scheduled_at": "2025-01-15 10:00:00",
    "qm_remarks": "Overall performance was satisfactory",
    "approver_remarks": "Approved with minor recommendations",
    "status": "approved"
  }'
```

**Response (200 OK):**

```json
{
  "success": true,
  "message": "Remarks saved successfully."
}
```

---

## 🧭 6. Get All Color Codes

**Endpoint:**
`GET /api/mockdrill/reviews/colors`

**Description:**
Retrieve all color codes available for mock drills.

**Response (200 OK):**

```json
{
  "success": true,
  "colors": [
    {
      "id": 1,
      "name": "Code Red",
      "code": "#FF0000"
    },
    {
      "id": 2,
      "name": "Code Blue",
      "code": "#0000FF"
    }
  ]
}
```

---

## 🧑‍🔬 7. Database Schema Reference

### **Table: mockdrill_reviews**

| Field          | Type      | Description                      |
| -------------- | --------- | -------------------------------- |
| id             | bigint    | Primary Key                      |
| checklist_id   | bigint    | Linked checklist ID              |
| coordinator_id | bigint    | Coordinator who submitted        |
| color_code_id  | bigint    | Associated color code            |
| scheduled_at   | datetime  | Drill schedule date              |
| is_compliance  | boolean   | 1 = Compliant, 0 = Non-compliant |
| review         | text      | Coordinator review               |
| created_at     | timestamp | Creation time                    |
| updated_at     | timestamp | Last updated                     |
| deleted_at     | timestamp | Soft delete (nullable)           |

---

### **Table: mockdrill_review_remarks**

| Field            | Type      | Description                                       |
| ---------------- | --------- | ------------------------------------------------- |
| id               | bigint    | Primary Key                                       |
| color_code_id    | bigint    | Linked color code                                 |
| scheduled_at     | datetime  | Schedule date                                     |
| qm_remarks       | text      | Remarks by QMS reviewer                           |
| approver_remarks | text      | Remarks by approver                               |
| status           | enum      | Review status (`pending`, `approved`, `rejected`) |
| created_at       | timestamp | Creation time                                     |
| updated_at       | timestamp | Last updated                                      |
| deleted_at       | timestamp | Soft delete (nullable)                            |

---

## 🧪 Example Testing Data (Postman)

### Authorization

```
Bearer: YOUR_TOKEN
X-Tenant-ID: tenant_001
```

### Example POST Body (Submit Review)

```json
{
  "color_code_id": 1,
  "scheduled_at": "2025-01-15 10:00:00",
  "checklist": { "1": 1, "2": 0 },
  "review": { "1": "Good response", "2": "Needs improvement" }
}
```

### Example POST Body (Save Remarks)

```json
{
  "color_code_id": 1,
  "scheduled_at": "2025-01-15 10:00:00",
  "qm_remarks": "Satisfactory",
  "approver_remarks": "Approved",
  "status": "approved"
}
```
