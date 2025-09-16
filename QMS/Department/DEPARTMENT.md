
# Department API Documentation

This API provides endpoints to manage **Departments** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `departments` table contains the following fields:

| # | Name        | Type                       | Attributes  | Null | Default | Extra           |
| - | ----------- | -------------------------- | ----------- | ---- | ------- | --------------- |
| 1 | id          | bigint(20) unsigned        | Primary Key | No   | None    | AUTO\_INCREMENT |
| 2 | name        | varchar(255)               |             | No   | None    |                 |
| 3 | status      | enum('active', 'inactive') |             | No   | active  |                 |
| 4 | created\_at | timestamp                  |             | Yes  | NULL    |                 |
| 5 | updated\_at | timestamp                  |             | Yes  | NULL    |                 |

**Sample Row:**

| id | name       | status | created\_at         | updated\_at         |
| -- | ---------- | ------ | ------------------- | ------------------- |
| 2  | Production | active | 2025-06-28 01:29:01 | 2025-06-28 01:29:01 |

---

## Base URL

```
https://your-api-domain.com/api/departments
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

### 1. List All Departments

* **Endpoint:** `GET /api/departments`
* **Description:** Retrieve a list of departments.

**Example Request:**

```bash
curl -X GET "https://your-api-domain.com/api/departments" \
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
  "message": "Departments fetched successfully",
  "data": [
    {
      "id": 2,
      "name": "Production",
      "status": "active",
      "created_at": "2025-06-28T01:29:01.000000Z",
      "updated_at": "2025-06-28T01:29:01.000000Z"
    }
  ]
}
```

---

### 2. Create a Department

* **Endpoint:** `POST /api/departments`
* **Description:** Add a new department.

**Example Request Body:**

```json
{
  "name": "Finance"
}
```

**Example Response:**

```json
{
  "message": "Created",
  "data": {
    "id": 6,
    "name": "Finance",
    "status": "active"
  }
}
```

**Validation Error Example (Missing or Duplicate Name):**

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

### 3. Get a Single Department

* **Endpoint:** `GET /api/departments/{id}`
* **Description:** Get details of a specific department by its ID.

**Example Response:**

```json
{
  "id": 2,
  "name": "Production",
  "status": "active",
  "created_at": "2025-06-28T01:29:01.000000Z",
  "updated_at": "2025-06-28T01:29:01.000000Z"
}
```

**Not Found Example:**

```json
{
  "success": false,
  "status": 404,
  "message": "Department not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update a Department

* **Endpoint:** `PUT /api/departments/{id}`
* **Description:** Update an existing department.

**Example Request Body:**

```json
{
  "name": "Quality Assurance"
}
```

**Example Response:**

```json
{
  "message": "Updated",
  "data": {
    "id": 2,
    "name": "Quality Assurance",
    "status": "active"
  }
}
```



## Summary Table

| Method | Endpoint              | Description               |
| ------ | --------------------- | ------------------------- |
| GET    | /api/departments      | List all departments      |
| POST   | /api/departments      | Create a department       |
| GET    | /api/departments/{id} | Get a specific department |
| PUT    | /api/departments/{id} | Update a department       |

---

## Notes

* **Authentication:** All routes require Sanctum authentication.
* **Header Requirement:** `domain: psri.com` is mandatory.
* **Validation:** Name must be unique and not empty.
* **Status:** Defaults to `active` unless specified otherwise.

---