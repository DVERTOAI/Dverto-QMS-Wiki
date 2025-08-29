# User-Department Mapping API Documentation

This API manages mappings between users and department levels/roles for tenants. All endpoints require Laravel Sanctum authentication.

---

## Database Table Structure

The `user_department_mappings` table (example) contains:

| # | Name             | Type                     | Attributes  | Null | Default | Extra           |
| - | ---------------- | ------------------------ | ----------- | ---- | ------- | --------------- |
| 1 | id               | bigint(20) unsigned      | Primary Key | No   | None    | AUTO_INCREMENT  |
| 2 | user_id          | bigint(20) unsigned      | Foreign Key | No   | None    | indexed         |
| 3 | department_id    | bigint(20) unsigned      | Foreign Key | No   | None    | indexed         |
| 4 | dept_level_id    | bigint(20) unsigned      | Foreign Key | Yes  | NULL    | indexed         |
| 5 | role_start_date  | date                     |             | Yes  | NULL    |                 |
| 6 | role_end_date    | date                     |             | Yes  | NULL    |                 |
| 7 | status           | enum('active','inactive')|             | No   | active  |                 |
| 8 | created_at       | timestamp                |             | Yes  | NULL    |                 |
| 9 | updated_at       | timestamp                |             | Yes  | NULL    |                 |

**Sample Row:**

| id | user_id | department_id | dept_level_id | role_start_date | role_end_date | status | created_at           |
| -- | ------- | ------------- | ------------- | --------------- | ------------- | ------ | -------------------- |
| 5  | 12      | 2             | 4             | 2025-07-01      | 2026-06-30    | active | 2025-07-01 09:00:00  |

---

## Base URL

```
https://your-api-domain.com/api/user-department-mappings
```

There is also a department-scoped, paginated endpoint for convenience:
```
GET /api/departments/{department_id}/user-department-mappings
```

---

## Authentication & Headers

All endpoints require a valid Bearer token.

**Required Headers:**

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## Endpoints

### 1. List All Mappings
* **Endpoint:** `GET /api/user-department-mappings`
* **Description:** Returns a list or paginated list of mappings. Supports query params: `page`, `per_page`, `search` (user name, dept name), `department_id`, `user_id`, `status`, `sort_by`, `sort_order`.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/user-department-mappings?page=1&per_page=15&search=John" \
  -H "Authorization: Bearer <your_token>" -H "domain: psri.com"
```

**Example Response (paginated):**
```json
{
  "success": true,
  "status": 200,
  "message": "Mappings fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 5,
        "user_id": 12,
        "user": { "id": 12, "name": "John Doe" },
        "department_id": 2,
        "department": { "id": 2, "name": "Production" },
        "dept_level_id": 4,
        "role_start_date": "2025-07-01",
        "role_end_date": "2026-06-30",
        "status": "active"
      }
    ],
    "per_page": 15,
    "total": 1
  }
}
```

> Note: The API may return a simple array or a paginated object. Frontends should normalize by using `response.data` when paginated.

---

### 2. Department-scoped List (recommended for department-first UIs)
* **Endpoint:** `GET /api/departments/{department_id}/user-department-mappings`
* **Description:** Returns mappings scoped to the department, supports same query params for pagination and search.

**Example:**
```bash
GET /api/departments/2/user-department-mappings?page=1&per_page=10&search=smith
```

---

### 3. Create Mapping(s)
* **Endpoint:** `POST /api/user-department-mappings`
* **Description:** Create a single mapping or multiple mappings in one request depending on implementation.

**Single Example Body:**
```json
{
  "user_id": 12,
  "department_id": 2,
  "dept_level_id": 4,
  "role_start_date": "2025-07-01",
  "role_end_date": "2026-06-30"
}
```

**Bulk Example Body:**
```json
{
  "department_id": 2,
  "working_users": [
    { "user_id": 12, "dept_level_id": 4, "role_start_date": "2025-07-01" },
    { "user_id": 13, "dept_level_id": 4 }
  ]
}
```

**Success Response:**
```json
{ "message": "Created", "data": [ { "id": 21, "user_id": 12, "department_id": 2 } ] }
```

**Validation / Conflict (duplicate assignment) — HTTP 422:**
```json
{
  "message": "Duplicate assignment",
  "errors": {
    "user_id": ["User already assigned to this department level"]
  }
}
```

**Important:** To avoid 422 duplicate-assignment when editing mappings in bulk, preserve existing mapping `id` values for items that already exist and call `PUT`/`PATCH` for those items; create only new mappings without an `id`.

---

### 4. Update Mapping
* **Endpoint:** `PUT /api/user-department-mappings/{id}`
* **Description:** Update fields for an existing mapping. Preserve `id` when performing bulk edits so server treats them as updates rather than new creates.

**Example Body:**
```json
{
  "dept_level_id": 5,
  "role_start_date": "2025-08-01",
  "role_end_date": "2026-07-31"
}
```

**Success Response:**
```json
{ "message": "Updated", "data": { "id": 5, "dept_level_id": 5 } }
```

---

### 5. Delete Mapping
* **Endpoint:** `DELETE /api/user-department-mappings/{id}`
* **Description:** Remove a mapping. In bulk management forms, call delete for mappings removed from the UI that had an existing `id`.

**Success Response:**
```json
{ "message": "Deleted" }
```

---

## Frontend Integration Notes
- Prefer department-first flows: load `GET /api/departments/{id}/user-department-mappings` with `page`, `per_page`, `search` for server-side paging and searching.
- When pre-filling forms with existing mappings, keep the mapping `id` in the form state so updates are sent as `PUT` instead of creating duplicates.
- Normalize responses: server may return paginated objects — in the frontend check for `.data`.
- Send `page=1` when search query changes to reset paging.
- Use debounce on search input (e.g., 300ms) to avoid request churn.

---

## Implementation Notes / Gotchas
- SQL ambiguous-column errors can occur when joining users/departments; server queries should qualify columns (e.g., `users.name`, `departments.name`) to avoid errors.
- Bulk create/update flows must distinguish between new rows and existing rows by presence of `id`.
- Validation will reject duplicate active assignments — design UI to preserve `id` and perform updates instead of blind creates.

---

## Summary Table

| Method | Endpoint                                                  | Description                              |
| ------ | --------------------------------------------------------- | ---------------------------------------- |
| GET    | /api/user-department-mappings                             | List mappings (global)                   |
| GET    | /api/departments/{department_id}/user-department-mappings | List mappings for a department (paginated)|
| POST   | /api/user-department-mappings                             | Create mapping(s)                        |
| PUT    | /api/user-department-mappings/{id}                        | Update mapping                           |
| DELETE | /api/user-department-mappings/{id}                        | Delete mapping                           |

---

If you'd like this placed in another location (frontend repo or project wiki) or want sample API client snippets (axios/fetch) added, tell me where and I will add them.
