# Public Tabs API Documentation

This API allows **public access** to assigned tabs for departments.
No authentication is required.

---

## Base URL

```
{{site_url}}/api/public
```

## Headers

```http
Content-Type: application/json
Accept: application/json
```

---

## 1. Get Departments

**GET** `/departments`

**Description:** Fetch all departments.

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "Departments fetched successfully",
  "data": [
    {
      "id": 4,
      "name": "basement lab",
      "status": "inactive",
      "created_at": "2025-09-09T08:08:31.000000Z",
      "updated_at": "2025-09-18T02:49:29.000000Z"
    },
    {
      "id": 1,
      "name": "Floor",
      "status": "active",
      "created_at": "2025-08-04T03:02:49.000000Z",
      "updated_at": "2025-08-04T03:02:49.000000Z"
    },
    {
      "id": 2,
      "name": "ICU",
      "status": "active",
      "created_at": "2025-08-20T06:34:37.000000Z",
      "updated_at": "2025-08-20T06:34:37.000000Z"
    },
    {
      "id": 3,
      "name": "Surgery",
      "status": "active",
      "created_at": "2025-08-20T07:56:13.000000Z",
      "updated_at": "2025-08-20T07:56:13.000000Z"
    }
  ]
}
```

---

## 2. Get Assigned Tabs by Department

**GET** `/tabs`

**Query Parameters:**

* `department_id` (required) - ID of the department
* `category` (optional) - Tab category, e.g., `MSDS` or `RISK`
* `search` (optional) - Search by tab name

**Example Request:**

```
{{site_url}}/api/public/tabs?department_id=1&category=MSDS&search=Chemical
```

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "Assigned tabs fetched successfully",
  "data": {
    "data": [
      {
        "id": 1,
        "name": "Chemical Handling Guidelines",
        "category": "MSDS"
      }
    ],
    "count": 1
  }
}
```

---

## Notes

* Tabs will only return if assigned to the given department.
* `category` and `search` are optional filters.
* No authentication required for public endpoints.

