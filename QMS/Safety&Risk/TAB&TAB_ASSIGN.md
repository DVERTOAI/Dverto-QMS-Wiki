# Tab Assignment API Documentation

This API manages **Tabs, Department Assignments, and Category Filters**.
All endpoints require **Laravel Sanctum authentication** unless noted otherwise.

---

## Base URL

```
https://your-api-domain.com/api/safety-risk
```

## Headers

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```

---

## 🗄 Database Tables

### `tabs`

| # | Column     | Type            | Null | Default | Extra          |
| - | ---------- | --------------- | ---- | ------- | -------------- |
| 1 | id         | bigint UNSIGNED | No   | None    | AUTO_INCREMENT |
| 2 | name       | varchar(255)    | No   | None    |                |
| 3 | category   | varchar(255)    | No   | None    |                |
| 4 | status     | varchar(50)     | No   | active  |                |
| 5 | created_at | timestamp       | Yes  | NULL    |                |
| 6 | updated_at | timestamp       | Yes  | NULL    |                |

### `tab_assign`

| # | Column        | Type            | Null | Default | Extra                 |
| - | ------------- | --------------- | ---- | ------- | --------------------- |
| 1 | id            | bigint UNSIGNED | No   | None    | AUTO_INCREMENT        |
| 2 | tab_id        | bigint UNSIGNED | No   | None    | FK → `tabs.id`        |
| 3 | department_id | bigint UNSIGNED | No   | None    | FK → `departments.id` |
| 4 | created_at    | timestamp       | Yes  | NULL    |                       |
| 5 | updated_at    | timestamp       | Yes  | NULL    |                       |

---

## 1. List Tabs

**GET** `/tabs`

**Query Parameters:**

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| category  | string | No       | Filter tabs by category                     |
| status    | string | No       | Filter tabs by status (`active`/`inactive`) |
| search    | string | No       | Search by tab name                          |

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "Tabs fetched successfully",
  "data": [
    {
      "id": 1,
      "name": "Fire Safety",
      "category": "Safety",
      "status": "active"
    },
    {
      "id": 2,
      "name": "Equipment Handling",
      "category": "Operations",
      "status": "active"
    }
  ]
}
```

---

## 2. Create Tab

**POST** `/tabs`

**Payload:**

```json
{
  "name": "New Tab Name",
  "category": "Safety",
  "status": "active"
}
```

**Success Response:**

```json
{
  "success": true,
  "status": 201,
  "message": "Tab created successfully",
  "data": {
    "id": 3,
    "name": "New Tab Name",
    "category": "Safety",
    "status": "active"
  }
}
```

---

---

## 3. Get Single Tab

**GET** `/tabs/{id}`

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "Tab fetched successfully",
  "data": {
    "id": 1,
    "name": "Fire Safety",
    "category": "RISK",
    "status": "active",
    "created_at": "2025-10-08T10:00:00.000000Z",
    "updated_at": "2025-10-08T10:00:00.000000Z"
  }
}
```

---

## 4. Update Tab

**PUT** `/tabs/{id}`

**Request (JSON):**

```json
{
  "name": "Fire Safety Updated",
  "category": "RISK",
  "status": "inactive"
}
```

**Success Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Tab updated successfully",
  "data": {
    "id": 1,
    "name": "Fire Safety Updated",
    "category": "RISK",
    "status": "inactive",
    "created_at": "2025-10-08T10:00:00.000000Z",
    "updated_at": "2025-10-08T11:15:00.000000Z"
  }
}
```


## 5. Assign Tabs to Department (Bulk)

**POST** `/tab-assignments/{departmentId}`

**Payload:**

```json
{
  "tab_ids": [1, 2, 5]
}
```

**Description:** Assign multiple tabs to a department. Existing assignments will be replaced.

**Success Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Department tab mappings updated successfully",
  "data": {
    "department_id": 2,
    "assigned_tabs": [
      { "id": 1, "name": "Fire Safety", "category": "Safety" },
      { "id": 2, "name": "Equipment Handling", "category": "Operations" },
      { "id": 5, "name": "Chemical Safety", "category": "Safety" }
    ]
  }
}
```

---

## 6. Get Department Tab Assignments

**GET** `/tab-assignments/{departmentId}`

**Optional Query Parameters:**

| Parameter | Type   | Required | Description                      |
| --------- | ------ | -------- | -------------------------------- |
| category  | string | No       | Filter assigned tabs by category |

**Response Example:**

```json
{
    "success": true,
    "status": 200,
    "message": "Department tab mappings fetched successfully",
    "data": {
        "department_id": 1,
        "category": null,
        "available_tabs": [],
        "mapped_tabs": [
            {
                "id": 1,
                "name": "Chemical Handling Guidelines",
                "category": "MSDS"
            },
            {
                "id": 2,
                "name": "Electrical Hazard Assessment",
                "category": "RISK"
            }
        ],
        "total_mapped": 2
    }
}
```


---

## Summary Table

| Method | Endpoint                                | Description                          |
| ------ | --------------------------------------- | ------------------------------------ |
| GET    | /tabs                                   | List all tabs                        |
| POST   | /tabs                                   | Create a new tab                     |
| GET    | /tabs/{id}                              | Get a single tab                     |
| PUT    | /tabs/{id}                              | Update a tab                         |
| POST   | /tab-assignments/{departmentId}         | Assign multiple tabs to a department |
| GET    | /tab-assignments/{departmentId}         | List tabs assigned to a department   |

