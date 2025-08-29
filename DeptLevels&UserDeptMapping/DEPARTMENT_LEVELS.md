# Department Levels API Documentation

This API manages Department Levels (hierarchical levels within a department). All endpoints require Laravel Sanctum authentication.

---

## Database Table Structure

The `department_levels` table (example) contains:

| # | Name          | Type                     | Attributes  | Null | Default | Extra           |
| - | ------------- | ------------------------ | ----------- | ---- | ------- | --------------- |
| 1 | id            | bigint(20) unsigned      | Primary Key | No   | None    | AUTO_INCREMENT  |
| 2 | department_id | bigint(20) unsigned      | Foreign Key | No   | None    | indexed         |
| 3 | name          | varchar(255)             |             | No   | None    |                 |
| 4 | level_number  | int                      |             | No   | 0       |                 |
| 5 | parent_id     | bigint(20) unsigned      | Nullable FK | Yes  | NULL    | references id   |
| 6 | status        | enum('active','inactive')|             | No   | active  |                 |
| 7 | created_at    | timestamp                |             | Yes  | NULL    |                 |
| 8 | updated_at    | timestamp                |             | Yes  | NULL    |                 |

**Sample Row:**

| id | department_id | name      | level_number | parent_id | status | created_at           | updated_at           |
| -- | ------------- | --------- | ------------ | --------- | ------ | -------------------- | -------------------- |
| 1  | 2             | Level 1   | 1            | NULL      | active | 2025-06-28 01:29:01  | 2025-06-28 01:29:01  |

---

## Base URL

```
https://your-api-domain.com/api/department-levels
```

---

## Authentication

Required headers for all requests:

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## Endpoints

### 1. List Department Levels
* **Endpoint:** `GET /api/department-levels`
* **Description:** Returns paginated list. Supports `page`, `per_page`, `search`, `department_id`, `status`, `sort_by`, `sort_order`.

**Example Response (paginated):**

```json
{
  "success": true,
  "status": 200,
  "message": "Department levels fetched successfully",
  "data": { "current_page":1, "data": [ {"id":1,"department_id":2,"name":"Level 1","level_number":1,"parent_id":null,"status":"active"} ], "per_page":10, "total":1 }
}
```

---

### 2. Create Department Level
* **Endpoint:** `POST /api/department-levels`
* **Body:**
```json
{
  "department_id": 2,
  "name": "Level 2",
  "level_number": 2,
  "parent_id": 1,
  "status": "active"
}
```

**Success Response:**
```json
{ "message":"Created", "data": { "id": 2, "department_id":2, "name":"Level 2", "level_number":2 } }
```

**Validation Error Example:**
```json
{ "message":"The name field is required.", "errors": { "name": ["The name field is required."] } }
```

---

### 3. Get Single Department Level
* **Endpoint:** `GET /api/department-levels/{id}`
* **Response:** single level object (404 if not found).

---

### 4. Update Department Level
* **Endpoint:** `PUT /api/department-levels/{id}`
* **Body:** same fields as create; partial updates allowed depending on implementation.

**Success Response:**
```json
{ "message":"Updated", "data": { "id":1, "name":"Level 1 - Renamed" } }
```

---

### 5. Delete Department Level
* **Endpoint:** `DELETE /api/department-levels/{id}`
* **Description:** Removes the record (or soft-deactivates, depending on server behavior).

**Success Response:**
```json
{ "message": "Deleted" }
```

---

## Notes
* `department_id` should refer to an existing department.
* `level_number` indicates ordering; lower numbers are higher in hierarchy.
* `parent_id` allows building a tree; avoid circular parent relationships.
* Validation: `name` required, `department_id` required, `level_number` numeric.
* Consider soft-delete or cascade handling for children when deleting a level.

---

If you'd like this file placed in a different location (repo wiki or frontend repo), tell me where and I will move it.
