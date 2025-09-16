# Working Hours API Documentation

This API provides endpoints to manage **Working Hours** for hospital departments. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `working_hours` table contains the following fields:

| # | Name           | Type                       | Attributes      | Null | Default | Extra           |
| - | -------------- | -------------------------- | --------------- | ---- | ------- | --------------- |
| 1 | id             | bigint(20) unsigned        | Primary Key     | No   | None    | AUTO_INCREMENT  |
| 2 | department_id  | bigint(20) unsigned        | Foreign Key     | No   | None    |                 |
| 3 | day_of_week    | enum('monday', ..., 'sunday') |               | No   | None    |                 |
| 4 | open_time      | time                       |                 | No   | None    |                 |
| 5 | close_time     | time                       |                 | No   | None    |                 |
| 6 | is_closed      | boolean                    |                 | No   | false   |                 |
| 7 | created_at     | timestamp                  |                 | Yes  | NULL    |                 |
| 8 | updated_at     | timestamp                  |                 | Yes  | NULL    |                 |

**Sample Row:**

| id | department_id | day_of_week | open_time | close_time | is_closed | created_at         | updated_at         |
| -- | ------------ | ----------  | --------- | ---------- | --------- | ------------------ | ------------------ |
| 1  | 2            | monday      | 09:00:00  | 17:00:00   | false     | 2025-08-23 09:00:00| 2025-08-23 09:00:00|

---

## Base URL

```
https://your-api-domain.com/api/working-hours
```

---

## Authentication

All endpoints require a valid Bearer token obtained from the login endpoint.

**Required Headers:**

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## Endpoints

### 1. List Working Hours by Department

* **Endpoint:** `GET /api/working-hours?department_id={id}`
* **Description:** Retrieve working hours for a specific department.

**Example Request:**

```bash
curl -X GET "https://your-api-domain.com/api/working-hours?department_id=2" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Working hours fetched successfully",
  "data": [
    {
      "id": 1,
      "department_id": 2,
      "day_of_week": "monday",
      "open_time": "09:00:00",
      "close_time": "17:00:00",
      "is_closed": false
    }
  ]
}
```

---

### 2. Create Working Hours

* **Endpoint:** `POST /api/working-hours`
* **Description:** Add working hours for a department and day.

**Example Request Body:**

```json
{
  "department_id": 2,
  "day_of_week": "monday",
  "open_time": "09:00:00",
  "close_time": "17:00:00",
  "is_closed": false
}
```

**Example Response:**

```json
{
  "message": "Created",
  "data": {
    "id": 1,
    "department_id": 2,
    "day_of_week": "monday",
    "open_time": "09:00:00",
    "close_time": "17:00:00",
    "is_closed": false
  }
}
```

**Validation Error Example (Duplicate Entry):**

```json
{
  "message": "Working hours for this department and day already exist.",
  "errors": {
    "department_id": [
      "Duplicate entry for department and day."
    ]
  }
}
```

---

### 3. Get Working Hours by ID

* **Endpoint:** `GET /api/working-hours/{id}`
* **Description:** Get details of a specific working hour entry.

**Example Response:**

```json
{
  "id": 1,
  "department_id": 2,
  "day_of_week": "monday",
  "open_time": "09:00:00",
  "close_time": "17:00:00",
  "is_closed": false
}
```

**Not Found Example:**

```json
{
  "success": false,
  "status": 404,
  "message": "Working hour not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update Working Hours

* **Endpoint:** `PUT /api/working-hours/{id}`
* **Description:** Update an existing working hour entry.

**Example Request Body:**

```json
{
  "open_time": "10:00:00",
  "close_time": "18:00:00",
  "is_closed": false
}
```

**Example Response:**

```json
{
  "message": "Updated",
  "data": {
    "id": 1,
    "department_id": 2,
    "day_of_week": "monday",
    "open_time": "10:00:00",
    "close_time": "18:00:00",
    "is_closed": false
  }
}
```

---

### 5. Delete Working Hours

* **Endpoint:** `DELETE /api/working-hours/{id}`
* **Description:** Delete a working hour entry.

**Example Response:**

```json
{
  "message": "Deleted"
}
```

---

## Summary Table

| Method | Endpoint                       | Description                    |
| ------ | ------------------------------ | ------------------------------ |
| GET    | /api/working-hours             | List working hours by department|
| POST   | /api/working-hours             | Create working hours           |
| GET    | /api/working-hours/{id}        | Get working hour by ID         |
| PUT    | /api/working-hours/{id}        | Update working hours           |
| DELETE | /api/working-hours/{id}        | Delete working hours           |

---

## Notes

* **Authentication:** All routes require Sanctum authentication.
* **Header Requirement:** `domain: psri.com` is mandatory.
* **Validation:** Only one entry per department and day.
* **is_closed:** If true, department is closed for that day.

---
