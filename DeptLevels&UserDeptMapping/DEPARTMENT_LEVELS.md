# Department Levels API Documentation

This document describes the Department Levels API as implemented in the codebase (controller, service, and routes).
Endpoints are protected by Laravel Sanctum and many responses use the `DepartmentLevelResource`.

---

## Database / Resource fields

The backend `DepartmentLevel` model exposes the following fields (via the resource):

| Field | Type | Notes |
| ----- | ---- | ----- |
| id | integer | Primary key |
| level_order | integer | Ordering within department (auto-generated when not provided) |
| level_name | string | Display name (unique) |
| label_code | string | Short code (unique) |
| is_highest | boolean | Marks the highest level |
| status | string ('active'|'inactive') | |
| created_at | timestamp | nullable |
| updated_at | timestamp | nullable |

Example resource object:

```json
{
  "id": 1,
  "level_order": 1,
  "level_name": "Level 1",
  "label_code": "L1",
  "is_highest": true,
  "status": "active",
  "created_at": "2025-06-28T01:29:01.000000Z",
  "updated_at": "2025-06-28T01:29:01.000000Z"
}
```

---

## Base URL

```
https://your-api-domain.com/api/department-levels
```

---

## Authentication / Headers

All endpoints require a valid Bearer token.

Required headers (per project convention):

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## Supported Endpoints

> Notes: The route file registers `Route::apiResource('department-levels', DepartmentLevelController::class)->except(['destroy']);` — therefore DELETE is not exposed by default via the API routes. The controller contains a `destroy` method, but confirm routing if you expect DELETE to be available.

### 1. List Department Levels

* **Endpoint:** `GET /api/department-levels`
* **Description:** Returns paginated results by default (service uses the pagination trait). Supports query params:
  - `page` (pagination)
  - `per_page` (pagination size)
  - `search` (searches `level_name`, `label_code`, `status`)
  - `type` (special values: `active` or `highest` — see below)
  - `sort_by`, `sort_order`

Special `type` behavior implemented in controller:
- `?type=active` — returns active department levels ordered by `level_order` (non-paginated list).
- `?type=highest` — returns the single highest department level (non-paginated single resource).

**Example request (paginated):**

```bash
GET /api/department-levels?page=1&per_page=10&search=Senior
```

**Example response (paginated wrapper):**

```json
{
  "success": true,
  "status": 200,
  "message": "Department levels fetched successfully",
  "data": {
    "current_page": 1,
    "data": [ /* array of department-level resources */ ],
    "per_page": 10,
    "total": 42
  }
}
```

**Example: get all active levels (non-paginated resource collection):**

```http
GET /api/department-levels?type=active
```

Response: JSON array of `DepartmentLevelResource` objects (200).

**Example: get highest level:**

```http
GET /api/department-levels?type=highest
```

Response: single `DepartmentLevelResource` (200) or 404 if none found.

---

### 2. Create Department Level

* **Endpoint:** `POST /api/department-levels`
* **Description:** Create a new department level.

**Request body (validated by `DepartmentLevelRequest`):**

```json
{
  "level_name": "Level 2",
  "label_code": "L2",
  "level_order": 2,        // optional; when omitted the server auto-generates the next order
  "is_highest": false,
  "status": "active"
}
```

**Success response:** 201 with created `DepartmentLevelResource`.

**Validation examples:**
- `level_name` is required and unique (DB constraint `level_name_unique`).
- `label_code` is required and unique (`label_code_unique`).
- `level_order` is optional but must be integer >= 1 when present; DB may enforce uniqueness (`level_order_unique`).

---

### 3. Get Single Department Level

* **Endpoint:** `GET /api/department-levels/{id}`
* **Description:** Fetch a single department level by ID.

**Success response:** 200 with `DepartmentLevelResource`.
**Not found:** 404 with error message.

---

### 4. Update Department Level

* **Endpoint:** `PUT /api/department-levels/{id}`
* **Description:** Update an existing department level. Request is validated with `DepartmentLevelRequest` (unique rules ignore the current ID).

**Request body:** same fields as create. Example:

```json
{
  "level_name": "Level 1 - Senior",
  "label_code": "L1S",
  "level_order": 1,
  "is_highest": true,
  "status": "active"
}
```

**Success response:** 200 with updated `DepartmentLevelResource`.

---

### (DELETE) Remove Department Level — not exposed by default

The route registration currently excludes `destroy` from the resource routes (`->except(['destroy'])`). The controller contains a `destroy` method, but it will only be reachable if you expose the route. If enabled, `DELETE /api/department-levels/{id}` should return a success response on deletion or a 404 when the record isn't found.

---

## Implementation Notes / Gotchas

- The service auto-generates `level_order` when not supplied (see `DepartmentLevelService::getNextLevelOrder`).
- When `is_highest` is set to true on create/update, the service clears the previous highest level automatically.
- Database constraint names used in error handling: `level_name_unique`, `label_code_unique`, `level_order_unique` — client code (frontend hooks) maps these to friendly messages.
- The `list` method supports search across `level_name`, `label_code`, and `status`, and uses sorting (default by `level_order`).

---

## Summary Table

| Method | Endpoint                        | Description |
| ------ | ------------------------------- | ----------- |
| GET    | /api/department-levels          | List department levels (paginated)
| GET    | /api/department-levels?type=active | Get all active levels (non-paginated)
| GET    | /api/department-levels?type=highest | Get highest level (single resource)
| POST   | /api/department-levels          | Create a department level
| GET    | /api/department-levels/{id}     | Get a department level
| PUT    | /api/department-levels/{id}     | Update a department level

---