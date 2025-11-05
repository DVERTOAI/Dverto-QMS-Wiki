# External Incident API Documentation

This API provides endpoints to create and manage **External Incidents** (incidents reported by visitors/external users). All endpoints are protected by authentication where applicable and expect a `domain` header for tenant routing. Files and attachments are supported for the create endpoint.

---

## Database Table Structure

The `incidents` table (tenant database) contains the following fields (relevant columns shown):

| # | Name             | Type                                               | Attributes  | Null | Default | Extra           |
| - | ---------------- | -------------------------------------------------- | ----------- | ---- | ------- | --------------- |
| 1 | id               | bigint(20) unsigned                                | Primary Key | No   | None    | AUTO_INCREMENT  |
| 2 | incident_no      | varchar(100)                                       |             | Yes  | NULL    |                 |
| 3 | type             | enum(...) (values from `config('constant.incident_type')`) |             | Yes  | NULL    |                 |
| 4 | incident_datetime| datetime                                           |             | No   | None    |                 |
| 5 | incident_place   | bigint (foreignId -> department_areas)             |             | Yes  | NULL    |                 |
| 6 | employee_id      | bigint (foreignId -> users)                        |             | Yes  | NULL    |                 |
| 7 | name             | varchar(50)                                        |             | Yes  | NULL    |                 |
| 8 | age              | integer                                            |             | Yes  | NULL    |                 |
| 9 | sex              | enum(...) (values from `config('constant.genders')`) |           | Yes  | NULL    |                 |
|10 | description      | text                                               |             | No   | None    |                 |
|11 | mode             | enum('OPD','IPD','Employee')                       |             | No   | None    |                 |
|12 | reporting_date   | date                                               |             | Yes  | NULL    |                 |
|13 | reporting_time   | time                                               |             | Yes  | NULL    |                 |
|14 | reported_by      | enum('Employee','Visitor')                         |             | Yes  | NULL    |                 |
|15 | uhid             | unsigned integer                                   |             | Yes  | NULL    |                 |
|16 | patient_name     | varchar(100)                                       |             | Yes  | NULL    |                 |
|17 | mobile           | unsignedBigInteger                                 |             | Yes  | NULL    |                 |
|18 | assigned_at      | timestamp                                          |             | Yes  | NULL    |                 |
|19 | final_remark     | text                                               |             | Yes  | NULL    |                 |
|20 | ip_no            | varchar(12)                                        |             | Yes  | NULL    |                 |
|21 | action           | enum('Yes','No')                                   |             | Yes  | NULL    |                 |
|22 | status           | enum(...) (values from `config('constant.incident_status')`) |     | No   | open    |                 |
|23 | deleted_at       | timestamp (soft deletes)                           |             | Yes  | NULL    |                 |
|24 | created_at       | timestamp                                          |             | Yes  | NULL    |                 |
|25 | updated_at       | timestamp                                          |             | Yes  | NULL    |                 |

**Notes:**
- The `type` enum values are derived from `config('constant.incident_type')`. If you add a new type in config, you must update tenant schemas or provide a migration for existing tenants.
- `incident_no` is generated using the number series logic and may include a suffix per incident type.

---

## Base URL

```
https://your-api-domain.com/api/external-incidents
```

---

## Authentication & Headers

Some endpoints are public (submit external incident); others require authentication. All requests should include tenant routing header.

**Required Headers:**

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>   # if endpoint requires auth
domain: psri.com                      # tenant domain header
```

---

## Endpoints

### 1. Create External Incident (Submit)

* **Endpoint:** `POST /api/external-incidents`
* **Description:** Submit a new external incident (visitor). Accepts form-data for attachments.
* **Auth:** Public (no auth required) by default; depends on route middleware.

**Example Request Body (JSON):**

```json
{
  "incident_date": "2025-10-17",
  "incident_time": "12:34",
  "incident_place": 5,
  "name": "John Doe",
  "age": 34,
  "sex": "male",
  "description": "Slipped in hallway",
  "mode": "OPD",
  "reported_by": "Visitor",
  "mobile": "9876543210",
  "reported_at": "2025-10-17T12:34:00"
}
```

**Form-data example (with files):**
- attachment: file[] (one or more files)
- Other fields as above.

**Example Successful Response (201):**

```json
{
  "success": true,
  "status": 201,
  "message": "External incident created successfully",
  "data": {
    "id": 123,
    "incident_no": "INC000123X",
    "type": "external",
    "incident_datetime": "2025-10-17T12:34:00.000000Z",
    "name": "John Doe",
    "mobile": "9876543210",
    "status": "open",
    "submitted_type": "external"
  }
}
```

**Validation Error Example:**

```json
{
  "message": "The description field is required.",
  "errors": {
    "description": [
      "The description field is required."
    ]
  }
}
```

---

### 2. Send OTP (if used)

* **Endpoint:** `POST /api/external-incidents/send-otp`
* **Description:** (Optional) Send an OTP to reporter's mobile when verifying external submissions.
* **Auth:** Public

**Example Request Body:**

```json
{
  "mobile": "9876543210"
}
```

**Example Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "OTP sent to mobile",
  "data": null
}
```

---

### 3. Verify OTP (if used)

* **Endpoint:** `POST /api/external-incidents/verify-otp`
* **Description:** (Optional) Verify the OTP to confirm mobile ownership before creating the incident.
* **Auth:** Public

**Example Request Body:**

```json
{
  "mobile": "9876543210",
  "otp": "123456"
}
```

**Example Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "OTP verified",
  "data": null
}
```

---

### 4. Get External Incident (Public view)

* **Endpoint:** `GET /api/external-incidents/{id}`
* **Description:** Retrieve the external incident details.
* **Auth:** Public or protected depending on your routes.

**Example Response:**

```json
{
  "id": 123,
  "incident_no": "INC000123X",
  "type": "external",
  "incident_datetime": "2025-10-17T12:34:00.000000Z",
  "name": "John Doe",
  "mobile": "9876543210",
  "status": "open",
  "description": "Slipped in hallway",
  "documents": []
}
```

---

## Summary Table

| Method | Endpoint                             | Description                      |
| ------ | ------------------------------------ | ------------------------------- |
| POST   | /api/external-incidents              | Create external incident         |
| POST   | /api/external-incidents/send-otp     | Send OTP to mobile (optional)    |
| POST   | /api/external-incidents/verify-otp   | Verify OTP (optional)            |
| GET    | /api/external-incidents/{id}         | Get external incident details    |

---

## Notes & Tips

- The `type` column is enum-based and is derived from `config('constant.incident_type')`. Be careful when updating config: existing tenant databases must be migrated to include any new enum values.
- For tenants that haven't had their enum updated, the service includes a runtime fallback mapping to a safe type and will log when this mapping occurs.
- Attachments are stored under `public/uploads/incidents/{incident_no}` and referenced in the `incident_documents` table.
- If you want emails enabled for external incidents, set `mail.enabled` to `true` in your config and ensure the `App\Mail\IncidentEmail` mailable is present.

---

If you'd like, I can also:
- Add example Postman collection entries for these endpoints.
- Provide the SQL ALTER statement to add `external` to existing tenants' `incidents.type` enum.

---
