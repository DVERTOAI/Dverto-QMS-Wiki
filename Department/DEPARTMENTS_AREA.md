# Department Area API Documentation

This API provides endpoints to manage **Department Areas** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `department_areas` table contains the following fields:

| #   | Name           | Type                                    | Attributes     | Null | Default | Extra           |
|-----|----------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | department_id  | bigint(20) unsigned (Indexed, FK)       |               | No   | None    |                 |
| 3   | name           | varchar(255)                            |               | No   | None    |                 |
| 4   | status         | enum('active', 'inactive')              |               | No   | active  |                 |
| 5   | created_at     | timestamp                               |               | Yes  | NULL    |                 |
| 6   | updated_at     | timestamp                               |               | Yes  | NULL    |                 |

**Sample Row:**

| id | department_id | name    | status  | created_at           | updated_at           |
|----|--------------|---------|---------|----------------------|----------------------|
| 2  | 2            | Account | active  | 2025-06-29 01:36:15  | 2025-06-29 01:36:15  |

---

## Base URL

```
https://your-api-domain.com/api/department-areas
```
*Replace `your-api-domain.com` with your actual API domain.*

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
- Replace `<your_token>` with the token received from your login response.

---

## Endpoints

### 1. List All Department Areas

- **Endpoint:** `GET /api/department-areas`
- **Description:** Get a paginated list of department areas, including their departments.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/department-areas" \
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
  "message": "Department areas fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 2,
        "department_id": 2,
        "name": "Account",
        "status": "active",
        "created_at": "2025-06-29T01:36:15.000000Z",
        "updated_at": "2025-06-29T01:36:15.000000Z",
        "department": {
          "id": 2,
          "name": "Production",
          "status": "active",
          "created_at": "2025-06-28T01:29:01.000000Z",
          "updated_at": "2025-06-28T01:29:01.000000Z"
        }
      }
    ],
    "first_page_url": "http://127.0.0.1:8000/api/department-areas?page=1",
    "from": 1,
    "last_page": 1,
    "last_page_url": "http://127.0.0.1:8000/api/department-areas?page=1",
    "links": [
      {
        "url": null,
        "label": "&laquo; Previous",
        "active": false
      },
      {
        "url": "http://127.0.0.1:8000/api/department-areas?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Next &raquo;",
        "active": false
      }
    ],
    "next_page_url": null,
    "path": "http://127.0.0.1:8000/api/department-areas",
    "per_page": 10,
    "prev_page_url": null,
    "to": 1,
    "total": 1
  }
}
```

---

### 2. Create a Department Area

- **Endpoint:** `POST /api/department-areas`
- **Description:** Create a new department area.

**Example Request Body:**
```json
{
  "department_id": 2,
  "name": "Research & Development"
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/department-areas" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"department_id":2,"name":"Research & Development"}'
```

**Example Response:**
```json
{
  "message": "Created",
  "data": {
    "id": 5,
    "department_id": 2,
    "name": "Research & Development"
  }
}
```

**Validation Error Example (Missing Fields):**
```json
{
  "errors": {
    "department_id": ["The department id field is required."],
    "name": ["The name field is required."]
  }
}
```

**Validation Error Example (Duplicate Name):**
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

### 3. Get a Single Department Area

- **Endpoint:** `GET /api/department-areas/{id}`
- **Description:** Get details of a specific department area by its ID.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/department-areas/2" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response (Success):**
```json
{
  "id": 2,
  "department_id": 2,
  "name": "Account",
  "status": "active",
  "created_at": "2025-06-29T01:36:15.000000Z",
  "updated_at": "2025-06-29T01:36:15.000000Z",
  "department": {
    "id": 2,
    "name": "Production"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Department area not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update a Department Area

- **Endpoint:** `PUT /api/department-areas/{id}`
- **Description:** Update an existing department area.

**Example Request Body:**
```json
{
  "department_id": 3,
  "name": "Product Testing"
}
```

**Example Request:**
```bash
curl -X PUT "https://your-api-domain.com/api/department-areas/2" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"department_id":3,"name":"Product Testing"}'
```

**Example Response (Success):**
```json
{
  "message": "Updated",
  "data": {
    "id": 2,
    "department_id": 3,
    "name": "Product Testing"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Department area not found",
  "data": null,
  "errors": null
}
```

**Validation Error Example (Duplicate Name):**
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

### 5. Delete a Department Area

- **Endpoint:** `DELETE /api/department-areas/{id}`
- **Description:** Delete a department area by its ID.

**Example Request:**
```bash
curl -X DELETE "https://your-api-domain.com/api/department-areas/2" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response (Success):**
```json
{
  "message": "Deleted"
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Department area not found",
  "data": null,
  "errors": null
}
```

---

## Summary Table

| Method | Endpoint                      | Description                       |
|--------|-------------------------------|-----------------------------------|
| GET    | /api/department-areas         | List all department areas         |
| POST   | /api/department-areas         | Create a department area          |
| GET    | /api/department-areas/{id}    | Get a specific department area    |
| PUT    | /api/department-areas/{id}    | Update a department area          |
| DELETE | /api/department-areas/{id}    | Delete a department area          |

---

## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **Content-Type:** Always set to `application/json`.
- **Error Handling:** All error responses are in JSON format.

---

## Contact

For any questions or support, contact the backend development team.
