# 🎯 **Coordinator Color Assignment API Documentation**

This API provides endpoints to **manage color assignments for coordinators**, including creating, updating, retrieving, and deleting assignment records. Each assignment links a **color code**, **department**, and one or more **admins (coordinators)** with a scheduled mock drill time.

---

## ✅ **Database Tables**

### 1. `coordinator_color_assignments`

| # | Column        | Type            | Attributes                   | Default | Description                  |
| - | ------------- | --------------- | ---------------------------- | ------- | ---------------------------- |
| 1 | id            | bigint UNSIGNED | Primary Key, Auto Increment  | —       | Unique assignment ID         |
| 2 | color_code_id | bigint UNSIGNED | Foreign Key → color_codes.id | —       | Linked color code            |
| 3 | department_id | bigint UNSIGNED | Foreign Key → departments.id | —       | Linked department            |
| 4 | scheduled_at  | datetime        | Not Null                     | —       | Scheduled mock drill time    |
| 5 | created_at    | timestamp       | Nullable                     | NULL    | Record creation timestamp    |
| 6 | updated_at    | timestamp       | Nullable                     | NULL    | Record last update timestamp |

---

### 2. `coordinator_color_assignment_admins`

| # | Column                          | Type            | Attributes                                     | Default | Description                  |
| - | ------------------------------- | --------------- | ---------------------------------------------- | ------- | ---------------------------- |
| 1 | id                              | bigint UNSIGNED | Primary Key, Auto Increment                    | —       | Unique record ID             |
| 2 | coordinator_color_assignment_id | bigint UNSIGNED | Foreign Key → coordinator_color_assignments.id | —       | Assignment reference         |
| 3 | admin_id                        | bigint UNSIGNED | Foreign Key → admins.id                        | —       | Linked admin user            |
| 4 | created_at                      | timestamp       | Nullable                                       | NULL    | Record creation timestamp    |
| 5 | updated_at                      | timestamp       | Nullable                                       | NULL    | Record last update timestamp |

---

## ✅ **Base URL**

```
{{site_url}}/api/mockdrill/drill-assignments
```

---

## ✅ **Endpoints**

---

### 🔹 1. Get All Drill Assignments

* **Endpoint:**

  ```
  GET /api/mockdrill/drill-assignments
  ```

* **Description:**
  Fetch a paginated list of all coordinator color assignments with related color codes, departments, and assigned admins.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Assignments fetched successfully",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 3,
                "color_code_id": 3,
                "department_id": 1,
                "scheduled_at": "2025-10-05 10:30:00",
                "created_at": "2025-10-07 12:03:26",
                "updated_at": "2025-10-07 12:03:26",
                "color_code": {
                    "id": 3,
                    "name": "Danger"
                },
                "users": [
                    { "id": 1, "name": "Tenant Admin" },
                    { "id": 3, "name": "Harsh Nishad" },
                    { "id": 4, "name": "Rauneet" }
                ],
                "department": {
                    "id": 1,
                    "name": "Floor"
                }
            }
        ],
        "per_page": 10,
        "total": 3
    }
}
```

---

### 🔹 2. Get Single Drill Assignment

* **Endpoint:**

  ```
  GET /api/mockdrill/drill-assignments/{id}
  ```

* **Description:**
  Retrieve detailed information for a specific coordinator color assignment by ID.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Success",
    "data": {
        "id": 2,
        "color_code_id": 3,
        "department_id": 1,
        "scheduled_at": "2025-10-05 10:30:00",
        "created_at": "2025-10-07 12:02:38",
        "updated_at": "2025-10-07 12:02:38",
        "color_code": {
            "id": 3,
            "name": "Danger"
        },
        "users": [],
        "department": {
            "id": 1,
            "name": "Floor"
        }
    }
}
```

---

### 🔹 3. Create Drill Assignment

* **Endpoint:**

  ```
  POST /api/mockdrill/drill-assignments
  ```

* **Payload:**

```json
{
  "color_code_id": 3,
  "department_id": 1,
  "scheduled_at": "2025-10-05 10:30:00",
  "admin_ids": [1, 3, 4]
}
```

* **Description:**
  Create a new coordinator color assignment with a linked color code, department, scheduled date/time, and one or more admins.

* **Example Response:**

```json
{
    "success": true,
    "status": 201,
    "message": "Assignment created successfully",
    "data": {
        "id": 3,
        "color_code_id": 3,
        "department_id": 1,
        "scheduled_at": "2025-10-05 10:30:00",
        "created_at": "2025-10-07 12:03:26",
        "updated_at": "2025-10-07 12:03:26"
    }
}
```

---

### 🔹 4. Update Drill Assignment

* **Endpoint:**

  ```
  PUT /api/mockdrill/drill-assignments/{id}
  ```

* **Payload:**

```json
{
  "color_code_id": 3,
  "department_id": 2,
  "scheduled_at": "2025-10-10 15:00:00",
  "admin_ids": [2]
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Assignment updated successfully",
    "data": {
        "id": 2,
        "color_code_id": 3,
        "department_id": 2,
        "scheduled_at": "2025-10-10 15:00:00",
        "created_at": "2025-10-07 12:02:38",
        "updated_at": "2025-10-07 12:32:37",
        "color_code": {
            "id": 3,
            "name": "Danger"
        },
        "users": [],
        "department": {
            "id": 1,
            "name": "Floor"
        }
    }
}
```

---

### 🔹 5. Delete Drill Assignment

* **Endpoint:**

  ```
  DELETE /api/mockdrill/drill-assignments/{id}
  ```

* **Description:**
  Delete a coordinator color assignment and its related admin mappings.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Assignment deleted successfully",
    "data": null
}
```

---

## ⚡ **Usage Examples**

1. **Fetch All Drill Assignments**

   ```bash
   curl {{site_url}}/api/mockdrill/drill-assignments
   ```

2. **Fetch Single Drill Assignment**

   ```bash
   curl {{site_url}}/api/mockdrill/drill-assignments/1
   ```

3. **Create Drill Assignment**

   ```bash
   curl -X POST {{site_url}}/api/mockdrill/drill-assignments \
   -H "Content-Type: application/json" \
   -d '{
         "color_code_id": 3,
         "department_id": 1,
         "scheduled_at": "2025-10-05 10:30:00",
         "admin_ids": [1,3,4]
       }'
   ```

4. **Update Drill Assignment**

   ```bash
   curl -X PUT {{site_url}}/api/mockdrill/drill-assignments/2 \
   -H "Content-Type: application/json" \
   -d '{
         "color_code_id": 3,
         "department_id": 2,
         "scheduled_at": "2025-10-10 15:00:00",
         "admin_ids": [2]
       }'
   ```

5. **Delete Drill Assignment**

   ```bash
   curl -X DELETE {{site_url}}/api/mockdrill/drill-assignments/1
   ```

---

## 🧩 **Relationships**

| Relation        | Table                                              | Description                                       |
| --------------- | -------------------------------------------------- | ------------------------------------------------- |
| `color_code_id` | `color_codes`                                      | Linked color representing the drill level or type |
| `department_id` | `departments`                                      | Department assigned for the mock drill            |
| `admin_ids`     | `admins` via `coordinator_color_assignment_admins` | Coordinators responsible for the assignment       |

