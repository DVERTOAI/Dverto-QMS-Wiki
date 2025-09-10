# Designation API Documentation

This API provides endpoints to manage **Designations** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `designations` table contains the following fields:

| #   | Name           | Type                                    | Attributes     | Null | Default | Extra           |
|-----|----------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | name           | varchar(255)                            |               | No   | None    |                 |
| 3   | status         | enum('active', 'inactive')              |               | No   | active  |                 |
| 4   | created_at     | timestamp                               |               | Yes  | NULL    |                 |
| 5   | updated_at     | timestamp                               |               | Yes  | NULL    |                 |

**Sample Row:**

| id | name       | status  | created_at           | updated_at           |
|----|------------|---------|----------------------|----------------------|
| 1  | Manager    | active  | 2025-07-14 09:30:15  | 2025-07-14 09:30:15  |
| 2  | Developer  | active  | 2025-07-14 09:31:22  | 2025-07-14 09:31:22  |
| 3  | Analyst    | inactive| 2025-07-14 09:32:10  | 2025-07-14 10:15:33  |

---

## Base URL

```
https://your-api-domain.com/api/designations
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

### 1. List All Designations

- **Endpoint:** `GET /api/designations`
- **Description:** Get a paginated list of designations with search and sorting capabilities.

**Query Parameters:**
- `search` (optional): Search by name or status
- `sort` (optional): Sort by id, name, or status
- `order` (optional): asc or desc (default: asc)
- `page` (optional): Page number for pagination
- `per_page` (optional): Number of items per page

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/designations?search=manager&sort=name&order=asc" \
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
  "message": "Designations fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "name": "Manager",
        "status": "active",
        "created_at": "2025-07-14T09:30:15.000000Z",
        "updated_at": "2025-07-14T09:30:15.000000Z"
      },
      {
        "id": 2,
        "name": "Developer",
        "status": "active",
        "created_at": "2025-07-14T09:31:22.000000Z",
        "updated_at": "2025-07-14T09:31:22.000000Z"
      },
      {
        "id": 3,
        "name": "Analyst",
        "status": "inactive",
        "created_at": "2025-07-14T09:32:10.000000Z",
        "updated_at": "2025-07-14T10:15:33.000000Z"
      }
    ],
    "first_page_url": "http://127.0.0.1:8000/api/designations?page=1",
    "from": 1,
    "last_page": 1,
    "last_page_url": "http://127.0.0.1:8000/api/designations?page=1",
    "links": [
      {
        "url": null,
        "label": "&laquo; Previous",
        "active": false
      },
      {
        "url": "http://127.0.0.1:8000/api/designations?page=1",
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
    "path": "http://127.0.0.1:8000/api/designations",
    "per_page": 10,
    "prev_page_url": null,
    "to": 3,
    "total": 3
  }
}
```

---

### 2. Create a Designation

- **Endpoint:** `POST /api/designations`
- **Description:** Create a new designation.

**Request Body Fields:**
- `name` (required): The designation name
- `status` (required): Either 'active' or 'inactive'

**Example Request Body:**
```json
{
  "name": "Senior Developer",
  "status": "active"
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/designations" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"name":"Senior Developer","status":"active"}'
```

**Example Response:**
```json
{
  "success": true,
  "status": 201,
  "message": "Designation created successfully",
  "data": {
    "id": 4,
    "name": "Senior Developer",
    "status": "active",
    "created_at": "2025-07-14T10:45:30.000000Z",
    "updated_at": "2025-07-14T10:45:30.000000Z"
  }
}
```

**Validation Error Example (Missing Fields):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The name field is required."],
    "status": ["The status field is required."]
  }
}
```

**Validation Error Example (Duplicate Active Name):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "The name has already been taken."
    ]
  }
}
```

**Note:** You can create a designation with the same name as an existing one only if the existing designation has 'inactive' status.

---

### 3. Get a Single Designation

- **Endpoint:** `GET /api/designations/{id}`
- **Description:** Get details of a specific designation by its ID.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/designations/1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response (Success):**
```json
{
  "success": true,
  "status": 200,
  "message": null,
  "data": {
    "id": 1,
    "name": "Manager",
    "status": "active",
    "created_at": "2025-07-14T09:30:15.000000Z",
    "updated_at": "2025-07-14T09:30:15.000000Z"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Designation not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update a Designation

- **Endpoint:** `PUT /api/designations/{id}`
- **Description:** Update an existing designation.

**Request Body Fields:**
- `name` (required): The designation name
- `status` (required): Either 'active' or 'inactive'

**Example Request Body:**
```json
{
  "name": "Senior Manager",
  "status": "active"
}
```

**Example Request:**
```bash
curl -X PUT "https://your-api-domain.com/api/designations/1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"name":"Senior Manager","status":"active"}'
```

**Example Response (Success):**
```json
{
  "success": true,
  "status": 200,
  "message": "Designation updated successfully",
  "data": {
    "id": 1,
    "name": "Senior Manager",
    "status": "active",
    "created_at": "2025-07-14T09:30:15.000000Z",
    "updated_at": "2025-07-14T11:20:45.000000Z"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Designation not found",
  "data": null,
  "errors": null
}
```

**Validation Error Example (Duplicate Active Name):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "The name has already been taken."
    ]
  }
}
```

**Validation Error Example (Invalid Status):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "status": [
      "The selected status is invalid."
    ]
  }
}
```

---

## Business Rules

### Name Uniqueness
- **Active Designations**: Names must be unique among all active designations
- **Inactive Designations**: You can create a new designation with the same name as an inactive one
- **Updates**: When updating, uniqueness is checked only against other active designations (excluding the current record)

### Examples:
- ✅ **Allowed**: Create "Manager" when existing "Manager" is inactive
- ❌ **Blocked**: Create "Manager" when existing "Manager" is active
- ✅ **Allowed**: Update designation name to existing inactive designation name
- ❌ **Blocked**: Update designation name to existing active designation name

---

## Summary Table

| Method | Endpoint                    | Description                    |
|--------|-----------------------------|--------------------------------|
| GET    | /api/designations           | List all designations          |
| POST   | /api/designations           | Create a designation           |
| GET    | /api/designations/{id}      | Get a specific designation     |
| PUT    | /api/designations/{id}      | Update a designation           |

---

## Search and Filtering

The list endpoint supports the following query parameters:

| Parameter | Type   | Description                           | Example                    |
|-----------|--------|---------------------------------------|----------------------------|
| search    | string | Search by name or status              | ?search=manager            |
| sort      | string | Sort by: id, name, status             | ?sort=name                 |
| order     | string | Sort order: asc, desc                 | ?order=desc                |
| page      | int    | Page number                           | ?page=2                    |
| per_page  | int    | Items per page                        | ?per_page=20               |

**Combined Example:**
```
GET /api/designations?search=dev&sort=name&order=asc&page=1&per_page=15
```

---

## Status Values

The system supports the following status values (configured in `config/constant.php`):

| Value    | Description                    |
|----------|--------------------------------|
| active   | Designation is currently active and in use |
| inactive | Designation is disabled and not in use     |

---

## Error Codes

| HTTP Code | Description                           |
|-----------|---------------------------------------|
| 200       | Success                               |
| 201       | Created successfully                  |
| 400       | Bad Request / Validation Error        |
| 401       | Unauthorized                          |
| 404       | Designation not found                 |
| 422       | Unprocessable Entity (Validation)     |
| 500       | Internal Server Error                 |

---

## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **Content-Type:** Always set to `application/json`.
- **Error Handling:** All error responses are in JSON format.
- **Pagination:** The list endpoint returns paginated results by default.
- **Tenant Connection:** All operations use the tenant database connection.

---

## Contact

For any questions or support, contact the backend development team.