# Source API Documentation

This API provides endpoints to manage **Sources** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `src_sources` table contains the following fields:

| #   | Name           | Type                                    | Attributes     | Null | Default | Extra           |
|-----|----------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | name           | varchar(255)                            |               | No   | None    |                 |
| 3   | type           | enum('category', 'subcategory')         |               | No   | category|                 |
| 4   | status         | enum('active', 'inactive')              |               | No   | active  |                 |
| 5   | created_at     | timestamp                               |               | Yes  | NULL    |                 |
| 6   | updated_at     | timestamp                               |               | Yes  | NULL    |                 |

**Sample Row:**

| id | name         | type        | status  | created_at           | updated_at           |
|----|--------------|-------------|---------|----------------------|----------------------|
| 1  | Campus Link  | category    | active  | 2025-08-23 07:30:15  | 2025-08-23 07:30:15  |
| 2  | Online Portal| subcategory | active  | 2025-08-23 07:31:22  | 2025-08-23 07:31:22  |
| 3  | Legacy System| category    | inactive| 2025-08-23 07:32:10  | 2025-08-23 08:15:33  |

---

## Base URL

```
https://your-api-domain.com/api/sources
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

### 1. List All Sources

- **Endpoint:** `GET /api/sources`
- **Description:** Get a paginated list of sources with search, filtering, and sorting capabilities.

**Query Parameters:**
- `search` (optional): Search by name
- `type` (optional): Filter by type ('category' or 'subcategory')
- `status` (optional): Filter by status ('active' or 'inactive')
- `sort_by` (optional): Sort by id, name, type, or status
- `sort_order` (optional): asc or desc (default: desc)
- `page` (optional): Page number for pagination
- `per_page` (optional): Number of items per page (default: 10)

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/sources?search=campus&type=category&status=active&sort_by=name&sort_order=asc" \
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
  "message": "Sources fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "name": "Campus Link",
        "type": "category",
        "status": "active",
        "created_at": "2025-08-23T07:30:15.000000Z",
        "updated_at": "2025-08-23T07:30:15.000000Z"
      },
      {
        "id": 2,
        "name": "Online Portal",
        "type": "subcategory",
        "status": "active",
        "created_at": "2025-08-23T07:31:22.000000Z",
        "updated_at": "2025-08-23T07:31:22.000000Z"
      },
      {
        "id": 3,
        "name": "Legacy System",
        "type": "category",
        "status": "inactive",
        "created_at": "2025-08-23T07:32:10.000000Z",
        "updated_at": "2025-08-23T08:15:33.000000Z"
      }
    ],
    "first_page_url": "http://127.0.0.1:8000/api/sources?page=1",
    "from": 1,
    "last_page": 1,
    "last_page_url": "http://127.0.0.1:8000/api/sources?page=1",
    "links": [
      {
        "url": null,
        "label": "&laquo; Previous",
        "active": false
      },
      {
        "url": "http://127.0.0.1:8000/api/sources?page=1",
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
    "path": "http://127.0.0.1:8000/api/sources",
    "per_page": 10,
    "prev_page_url": null,
    "to": 3,
    "total": 3
  }
}
```

---

### 2. Create a Source

- **Endpoint:** `POST /api/sources`
- **Description:** Create a new source.

**Request Body Fields:**
- `name` (required): The source name
- `type` (required): Either 'category' or 'subcategory'
- `status` (required): Either 'active' or 'inactive'

**Example Request Body:**
```json
{
  "name": "Campus Link",
  "type": "category",
  "status": "active"
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/sources" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"name":"Campus Link","type":"category","status":"active"}'
```

**Example Response:**
```json
{
  "success": true,
  "status": 201,
  "message": "Source created successfully",
  "data": {
    "id": 4,
    "name": "Campus Link",
    "type": "category",
    "status": "active",
    "created_at": "2025-08-23T10:45:30.000000Z",
    "updated_at": "2025-08-23T10:45:30.000000Z"
  }
}
```

**Validation Error Example (Missing Fields):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["Source name is required."],
    "type": ["Source type is required."],
    "status": ["Status is required."]
  }
}
```

**Validation Error Example (Duplicate Name):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "This source name already exists."
    ]
  }
}
```

**Validation Error Example (Invalid Type):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "type": [
      "Source type must be either category or subcategory."
    ]
  }
}
```

---

### 3. Get a Single Source

- **Endpoint:** `GET /api/sources/{id}`
- **Description:** Get details of a specific source by its ID.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/sources/1" \
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
  "message": "Source fetched successfully",
  "data": {
    "id": 1,
    "name": "Campus Link",
    "type": "category",
    "status": "active",
    "created_at": "2025-08-23T07:30:15.000000Z",
    "updated_at": "2025-08-23T07:30:15.000000Z"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Source not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update a Source

- **Endpoint:** `PUT /api/sources/{id}`
- **Description:** Update an existing source.

**Request Body Fields:**
- `name` (required): The source name
- `type` (required): Either 'category' or 'subcategory'
- `status` (required): Either 'active' or 'inactive'

**Example Request Body:**
```json
{
  "name": "Updated Campus Link",
  "type": "subcategory",
  "status": "active"
}
```

**Example Request:**
```bash
curl -X PUT "https://your-api-domain.com/api/sources/1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"name":"Updated Campus Link","type":"subcategory","status":"active"}'
```

**Example Response (Success):**
```json
{
  "success": true,
  "status": 200,
  "message": "Source updated successfully",
  "data": {
    "id": 1,
    "name": "Updated Campus Link",
    "type": "subcategory",
    "status": "active",
    "created_at": "2025-08-23T07:30:15.000000Z",
    "updated_at": "2025-08-23T11:20:45.000000Z"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Source not found",
  "data": null,
  "errors": null
}
```

**Validation Error Example (Duplicate Name):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": [
      "This source name already exists."
    ]
  }
}
```

**Validation Error Example (Invalid Type):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "type": [
      "Source type must be either category or subcategory."
    ]
  }
}
```

---

## Business Rules

### Name Uniqueness
- **All Sources**: Names must be unique across all sources regardless of status or type
- **Updates**: When updating, uniqueness is checked against all other sources (excluding the current record)

### Examples:
- ❌ **Blocked**: Create "Campus Link" when any existing source has the name "Campus Link"
- ✅ **Allowed**: Update source name to a unique name
- ❌ **Blocked**: Update source name to existing source name

---

## Summary Table

| Method | Endpoint                    | Description                    |
|--------|-----------------------------|--------------------------------|
| GET    | /api/sources                | List all sources               |
| POST   | /api/sources                | Create a source                |
| GET    | /api/sources/{id}           | Get a specific source          |
| PUT    | /api/sources/{id}           | Update a source                |

---

## Search and Filtering

The list endpoint supports the following query parameters:

| Parameter  | Type   | Description                           | Example                    |
|------------|--------|---------------------------------------|----------------------------|
| search     | string | Search by name                        | ?search=campus             |
| type       | string | Filter by type: category, subcategory | ?type=category             |
| status     | string | Filter by status: active, inactive    | ?status=active             |
| sort_by    | string | Sort by: id, name, type, status       | ?sort_by=name              |
| sort_order | string | Sort order: asc, desc                 | ?sort_order=desc           |
| page       | int    | Page number                           | ?page=2                    |
| per_page   | int    | Items per page                        | ?per_page=20               |

**Combined Example:**
```
GET /api/sources?search=campus&type=category&status=active&sort_by=name&sort_order=asc&page=1&per_page=15
```

---

## Type Values

The system supports the following type values:

| Value       | Description                           |
|-------------|---------------------------------------|
| category    | Main category source                  |
| subcategory | Subcategory under a main category     |

---

## Status Values

The system supports the following status values (configured in `config/constant.php`):

| Value    | Description                           |
|----------|---------------------------------------|
| active   | Source is currently active and in use |
| inactive | Source is disabled and not in use     |

---

## Error Codes

| HTTP Code | Description                           |
|-----------|---------------------------------------|
| 200       | Success                               |
| 201       | Created successfully                  |
| 400       | Bad Request / Validation Error        |
| 401       | Unauthorized                          |
| 404       | Source not found                      |
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
- **Table Name:** The model uses the `src_sources` table in the database.

---

## Contact

For any questions or support, contact the backend development team.
