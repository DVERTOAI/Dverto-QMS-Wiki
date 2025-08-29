# Holidays API Documentation

This API provides endpoints to manage **Holidays** in the application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `holidays` table contains the following fields (example schema):

| # | Name         | Type                        | Attributes  | Null | Default | Extra           |
| - | ------------ | --------------------------- | ----------- | ---- | ------- | --------------- |
| 1 | id           | bigint(20) unsigned         | Primary Key | No   | None    | AUTO_INCREMENT  |
| 2 | name         | varchar(255)                |             | No   | None    |                 |
| 3 | date         | date                        |             | No   | None    |                 |
| 4 | is_recurring | tinyint(1)                  |             | No   | 0       |                 |
| 5 | type         | varchar(50)                 |             | Yes  | NULL    | (e.g. public)   |
| 6 | status       | enum('active','inactive')   |             | No   | active  |                 |
| 7 | created_at   | timestamp                   |             | Yes  | NULL    |                 |
| 8 | updated_at   | timestamp                   |             | Yes  | NULL    |                 |

**Sample Row:**

| id | name           | date       | is_recurring | type   | status | created_at           | updated_at           |
| -- | -------------- | ---------- | ------------ | ------ | ------ | -------------------- | -------------------- |
| 1  | New Year's Day | 2026-01-01 | 1            | public | active | 2025-12-01 10:00:00  | 2025-12-01 10:00:00  |

---

## Base URL

```
https://your-api-domain.com/api/holidays
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

### 1. List Holidays

* **Endpoint:** `GET /api/holidays`
* **Description:** Retrieve a paginated list of holidays. Supports query params: `page`, `per_page`, `search` (by name), `year`, `status`, and `sort_by`/`sort_order`.

**Example Request:**

```bash
curl -X GET "https://your-api-domain.com/api/holidays?page=1&per_page=10&year=2026" \
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
  "message": "Holidays fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "name": "New Year's Day",
        "date": "2026-01-01",
        "is_recurring": 1,
        "type": "public",
        "status": "active",
        "created_at": "2025-12-01T10:00:00.000000Z",
        "updated_at": "2025-12-01T10:00:00.000000Z"
      }
    ],
    "per_page": 10,
    "total": 1
  }
}
```

---

### 2. Create Holiday

* **Endpoint:** `POST /api/holidays`
* **Description:** Add a new holiday.

**Example Request Body:**

```json
{
  "name": "New Year's Day",
  "date": "2026-01-01",
  "is_recurring": true,
  "type": "public",
  "status": "active"
}
```

**Example Response:**

```json
{
  "message": "Created",
  "data": {
    "id": 1,
    "name": "New Year's Day",
    "date": "2026-01-01",
    "is_recurring": 1,
    "type": "public",
    "status": "active"
  }
}
```

**Validation Error Example (missing or invalid fields):**

```json
{
  "message": "The date field is required.",
  "errors": {
    "date": [
      "The date field is required."
    ]
  }
}
```

**Conflict Example (duplicate date + name):**

```json
{
  "message": "A holiday already exists for the specified date.",
  "errors": {
    "date": [
      "A holiday already exists for this date."
    ]
  }
}
```

---

### 3. Get a Single Holiday

* **Endpoint:** `GET /api/holidays/{id}`
* **Description:** Get details of a specific holiday by its ID.

**Example Response:**

```json
{
  "id": 1,
  "name": "New Year's Day",
  "date": "2026-01-01",
  "is_recurring": 1,
  "type": "public",
  "status": "active",
  "created_at": "2025-12-01T10:00:00.000000Z",
  "updated_at": "2025-12-01T10:00:00.000000Z"
}
```

**Not Found Example:**

```json
{
  "success": false,
  "status": 404,
  "message": "Holiday not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update a Holiday

* **Endpoint:** `PUT /api/holidays/{id}`
* **Description:** Update an existing holiday.

**Example Request Body:**

```json
{
  "name": "New Year's Day (Observed)",
  "date": "2026-01-01",
  "is_recurring": true,
  "type": "public",
  "status": "active"
}
```

**Example Response:**

```json
{
  "message": "Updated",
  "data": {
    "id": 1,
    "name": "New Year's Day (Observed)",
    "date": "2026-01-01",
    "is_recurring": 1,
    "type": "public",
    "status": "active"
  }
}
```

**Validation Error Example:**

```json
{
  "message": "The name has already been taken.",
  "errors": {
    "name": [
      "The name has already been taken."
    ]
  }
}
```

---

### 5. Delete a Holiday

* **Endpoint:** `DELETE /api/holidays/{id}`
* **Description:** Remove a holiday from the system.

**Example Response:**

```json
{
  "message": "Deleted"
}
```

---

## Summary Table

| Method | Endpoint               | Description                |
| ------ | ---------------------- | -------------------------- |
| GET    | /api/holidays          | List holidays (paginated)  |
| POST   | /api/holidays          | Create a holiday           |
| GET    | /api/holidays/{id}     | Get a specific holiday     |
| PUT    | /api/holidays/{id}     | Update a holiday           |
| DELETE | /api/holidays/{id}     | Delete a holiday           |

---

## Notes

* **Authentication:** All routes require Sanctum authentication.
* **Header Requirement:** `domain: psri.com` is mandatory (per existing API conventions).
* **Validation:** `name` and `date` are required; `date` should be a valid date; duplicate holidays for the same `date` should be prevented.
* **is_recurring:** When true, the holiday repeats yearly (frontend/backend should apply logic accordingly).
* **Status:** Defaults to `active` unless specified otherwise.

---

If you want this added to the project wiki or the repo root as `HOLIDAYS_API_DOCUMENTATION.md`, tell me which location to use and I will add the file there.
