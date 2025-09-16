
# 📘 Incident API Documentation

This API provides endpoints to manage **Incidents** in your application.  
All endpoints are protected via **Laravel Sanctum** authentication.

---

## 🔐 Authentication

All requests must include:

```

Authorization: Bearer \<your\_token>
Content-Type: application/json
Accept: application/json
domain: psri.com

```

---

## 🌐 Base URL

```

[https://your-api-domain.com/api/incident](https://your-api-domain.com/api/incident)

````

---

## 🗃️ Database Structure

### `incidents` Table

| #  | Name                 | Type                                                             | Null | Default | Extra           |
|----|----------------------|------------------------------------------------------------------|------|---------|------------------|
| 1  | id                   | bigint(20) unsigned                                              | No   | None    | AUTO_INCREMENT   |
| 2  | incident_no          | varchar(100)                                                    | Yes  | NULL    |                  |
| 3  | type                 | enum('employee', 'internal_patient', 'medication_error')        | Yes  | NULL    |                  |
| 4  | incident_datetime    | datetime                                                        | No   | None    |                  |
| 5  | incident_place       | bigint(20) unsigned (FK to departments)                         | Yes  | NULL    |                  |
| 6  | employee_id          | bigint(20) unsigned                                             | Yes  | NULL    |                  |
| 7  | name                 | varchar(50)                                                     | Yes  | NULL    |                  |
| 8  | age                  | int(11)                                                         | Yes  | NULL    |                  |
| 9  | sex                  | enum('male', 'female', 'other')                                 | Yes  | NULL    |                  |
| 10 | description          | text                                                            | No   | None    |                  |
| 11 | mode                 | enum('OPD', 'IPD', 'Employee')                                  | No   | None    |                  |
| 12 | reporting_date       | date                                                            | Yes  | NULL    |                  |
| 13 | reporting_time       | time                                                            | Yes  | NULL    |                  |
| 14 | reported_by          | enum('Employee', 'Visitor')                                     | Yes  | NULL    |                  |
| 15 | uhid                 | int(10) unsigned                                                | Yes  | NULL    |                  |
| 16 | patient_name         | varchar(100)                                                    | Yes  | NULL    |                  |
| 17 | mobile               | bigint(20) unsigned                                             | Yes  | NULL    |                  |
| 18 | assigned_at          | timestamp                                                       | Yes  | NULL    |                  |
| 19 | final_remark         | text                                                            | Yes  | NULL    |                  |
| 20 | ip_no                | varchar(12)                                                     | Yes  | NULL    |                  |
| 21 | action               | enum('Yes', 'No')                                               | Yes  | NULL    |                  |
| 22 | status               | enum(...) (see below)                                           | No   | open    |                  |
| 23 | deleted_at           | timestamp                                                       | Yes  | NULL    |                  |
| 24 | created_at           | timestamp                                                       | Yes  | NULL    |                  |
| 25 | updated_at           | timestamp                                                       | Yes  | NULL    |                  |

---

### `incident_medication_error` Table

| #  | Name                             | Type             | Null | Default | Extra           |
|----|----------------------------------|------------------|------|---------|------------------|
| 1  | id                               | bigint unsigned  | No   | None    | AUTO_INCREMENT   |
| 2  | incident_id                      | bigint unsigned  | No   | None    |                  |
| 3  | room_no_floor                    | varchar(100)     | No   | None    |                  |
| 4  | ordered_drug_name                | varchar(200)     | No   | None    |                  |
| 5  | dosage                           | varchar(50)      | No   | None    |                  |
| 6  | route                            | varchar(50)      | No   | None    |                  |
| 7  | frequency                        | varchar(10)      | No   | None    |                  |
| 8  | medication_error_type           | varchar(100)     | No   | None    |                  |
| 9  | medication_error_category       | varchar(50)      | No   | None    |                  |
| 10 | medication_error_category_detail| varchar(200)     | No   | None    |                  |
| 11 | consultant                       | varchar(100)     | No   | None    |                  |
| 12 | medication_error_remark         | varchar(255)     | Yes  | NULL    |                  |
| 13 | created_at                       | timestamp        | Yes  | NULL    |                  |
| 14 | updated_at                       | timestamp        | Yes  | NULL    |                  |

---

## 🔧 Endpoints

### 1. Create an Incident

**Endpoint:** `POST /api/incident`  
**Description:** Create a new incident. Payload varies by `type`.

#### 🔸 Example Payloads

**Employee:**
```json
{
  "type": "employee",
  "incident_datetime": "2023-10-01 14:30:00",
  "incident_place": 1,
  "description": "harsh"
}
````

**Internal Patient:**

```json
{
  "type": "internal_patient",
  "incident_datetime": "2023-10-01 14:30:00",
  "incident_place": 1,
  "description": "Patient fell",
  "mode": "IPD",
  "uhid": 123456,
  "ip_no": "IPD001"
}
```

**Medication Error:**

```json
{
  "type": "medication_error",
  "incident_datetime": "2023-10-01 14:30:00",
  "incident_place": 1,
  "description": "Wrong dosage",
  "uhid": 123456,
  "room_no_floor": "Room 302",
  "ordered_drug_name": "Paracetamol",
  "dosage": "500mg",
  "route": "Oral",
  "frequency": "1x",
  "medication_error_type": "Wrong Dosage",
  "medication_error_category": "A",
  "medication_error_category_detail": "Incorrect strength",
  "sex": "Male",
  "age": 45,
  "consultant": "Dr. Smith",
  "medication_error_remark": "Was not double-checked"
}
```

---

## ✅ Validation Rules

| Field                       | Required | Type/Format        | Conditional On                          |
| --------------------------- | -------- | ------------------ | --------------------------------------- |
| type                        | ✅        | enum               | Always                                  |
| incident\_datetime          | ✅        | Y-m-d H\:i\:s      | Always                                  |
| incident\_place             | ✅        | department ID      | Always                                  |
| description                 | ✅        | string (max: 5000) | Always                                  |
| mode                        | ✅        | OPD/IPD            | if type = internal\_patient             |
| uhid                        | ✅        | digits (1-6)       | if type = internal\_patient or med\_err |
| ip\_no                      | ✅        | max 9 chars        | if mode = IPD                           |
| ordered\_drug\_name         | ✅        | string             | if type = medication\_error             |
| medication\_error\_category | ✅        | string             | if type = medication\_error             |
| medication\_error\_remark   | ✅\*      | string             | if medication\_error\_type = Others     |

---

## 📎 Attachments

* Field: `attachment[]`
* Type: `multipart/form-data`
* Allowed formats: `jpg`, `png`, `pdf`, `doc`, `docx`
* Max file size: **10 MB each**
* Storage path: `uploads/incidents/{incident_no}/filename`

---

### 2. List All Incidents

**Endpoint:** `GET /api/incident`
**Description:** Paginated incident list
**Query Params (optional):** `page`, `status`, `type`, `date_range`


**Response:**

```json
{
  "success": true,
  "message": "Incidents fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 11,
        "incident_no": "INCM100011",
        "type": "medication_error",
        "incident_datetime": "2023-10-01T14:30:00",
        "status": "open",
        "description": "Wrong medication administered"
      }
    ],
    "total": 12,
    "per_page": 10
  }
}

```
---

### 3. Get a Single Incident

**Endpoint:** `GET /api/incident/{id}`
**Description:** Returns full incident details (with type-specific and attachment data)

**Response:**
```json
{
    "success": true,
    "status": 200,
    "message": "Observations fetched successfully",
    "data": {
        "id": 10,
        "incident_no": "INCM100010",
        "incident_datetime": null,
        "description": "Patient received the wrong medication.",
        "status": "assigned",
        "reported_by": "Employee",
        "type": "medication_error",
        "mode": "OPD",
        "uhid": 1256,
        "ip_no": null,
        "action": null,
        "incident_place": 1,
        "employee_id": null,
        "documents": [
            {
                "id": 14,
                "documentable_type": "App\\Models\\Tenant\\Incident\\Incident",
                "documentable_id": 10,
                "name": "1753516002_User_API_Documentation.pdf",
                "file_path": "http://127.0.0.1:8000/storage/incident/2025/07/INCM100010/1753516002_1753516002_User_API_Documentation.pdf",
                "status": "active"
            },
        ],
        "area": {
            "id": 1,
            "name": "1 Floor",
            "status": "active"
        },
        "medication_error_details": {
            "room_no_floor": "Room 101, Floor 1",
            "ordered_drug_name": "Aspirin",
            "dosage": "500 mg",
            "route": "Oral",
            "frequency": "Once daily",
            "medication_error_type": "Wrong medication",
            "medication_error_category": "Administration error",
            "medication_error_category_detail": "Wrong drug administered",
            "sex": null,
            "age": null,
            "consultant": "Dr. Smith",
            "medication_error_remark": "hi"
        },
        "incident_details": {
            "id": 9,
            "root_cause_analysis": null,
            "corrective_action": null,
            "preventive_action": null,
            "postponed_to": null,
            "postponed_remark": null,
            "is_set_assignee": 1,
            "category": "incident",
            "comment": null,
            "assigned_department": {
                "id": 1,
                "name": "1 Floor"
            },
            "assigned_to": {
                "id": 1,
                "name": "John Doe",
                "email": "john.doe@company.com"
            },
            "due_at": null
        }
    }
}

```
---

### 4. Update an Incident

**Endpoint:** `PUT /api/incident/{id}`
**Description:** Update incident with same validation as `POST`

**Response:**
``` json

{
    "id": 52,
    "incident_no": "INC-0127--DEB",
    "type": "medication_error",
    "incident_datetime": "2023-10-01T14:30:00.000000Z",
    "incident_place": "1",
    "employee_id": 1,
    "name": "harsh Nisad",
    "age": 45,
    "sex": "Male",
    "description": "Patient received the wrong medication.",
    "mode": "OPD",
    "reporting_date": null,
    "reporting_time": null,
    "reported_by": "Employee",
    "uhid": "1256",
    "patient_name": null,
    "mobile": null,
    "assigned_at": null,
    "final_remark": null,
    "ip_no": null,
    "action": null,
    "status": "open",
    "deleted_at": null,
    "created_at": "2025-07-28T05:56:58.000000Z",
    "updated_at": "2025-07-28T05:58:52.000000Z"
}


```
---

## 📋 Summary Table

| Method | Endpoint           | Description                |
| ------ | ------------------ | -------------------------- |
| GET    | /api/incident      | List all incidents         |
| POST   | /api/incident      | Create an incident         |
| GET    | /api/incident/{id} | Get a specific incident    |
| PUT    | /api/incident/{id} | Update a specific incident |

---

## 🏷️ Status Values

* `open`
* `accepted`
* `declined`
* `assigned`
* `reassigned`
* `rca_submitted`
* `closed`
* `postponed`

---

## 🔍 Notes

* **incident\_no**: Auto-generated in format `INCM######`
* **Soft Deletes**: Enabled using `deleted_at`
* **Type-Based Logic**: Fields & rules vary based on `type`
* **Validation**: Centralized in `IncidentRequest.php`
* **Attachments**: Stored under incident folder structure

---
