# 📝 **Checklist API Documentation**

This API provides endpoints to manage checklists, including creating, updating, fetching all, and fetching a single checklist.

---

## ✅ **Database Table: `checklists`**

| # | Column     | Type                      | Attributes                  | Default | Description                                  |
| - | ---------- | ------------------------- | --------------------------- | ------- | -------------------------------------------- |
| 1 | id         | bigint UNSIGNED           | Primary Key, Auto Increment | —       | Unique checklist ID                          |
| 2 | name       | varchar(255)              | Not Null                    | —       | Checklist name                               |
| 3 | status     | enum('active','inactive') | Not Null                    | active  | Checklist status                             |
| 4 | type       | varchar(255)              | Not Null                    | —       | Type of checklist (e.g., Department, Safety) |
| 5 | is_header  | tinyint(1)                | Not Null                    | 0       | Indicates if this checklist is a header      |
| 6 | created_at | timestamp                 | Nullable                    | NULL    | Record creation timestamp                    |
| 7 | updated_at | timestamp                 | Nullable                    | NULL    | Last record update timestamp                 |

---

## ✅ **Base URL**

```
{{site_url}}/api/mockdrill/checklist
```

---

## ✅ **Endpoints**

---

### 🔹 1. Get All Checklists

* **Endpoint:**
  `GET /api/mockdrill/checklist`

* **Description:**
  Fetch a paginated list of all checklists.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Checklists fetched successfully",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 2,
                "name": "helio buddy",
                "status": "active",
                "type": "Department",
                "is_header": 1,
                "created_at": "2025-09-27T06:22:35.000000Z",
                "updated_at": "2025-09-27T06:22:35.000000Z"
            },
            {
                "id": 1,
                "name": "helio",
                "status": "active",
                "type": "Department",
                "is_header": 1,
                "created_at": "2025-09-26T13:51:10.000000Z",
                "updated_at": "2025-09-27T06:21:09.000000Z"
            }
        ],
        "per_page": 10,
        "total": 2
    }
}
```

---

### 🔹 2. Get Single Checklist

* **Endpoint:**
  `GET /api/mockdrill/checklist/{id}`

* **Description:**
  Retrieve details of a specific checklist by ID.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Success",
    "data": {
        "id": 1,
        "name": "helio",
        "status": "active",
        "type": "Department",
        "is_header": 1,
        "created_at": "2025-09-26T13:51:10.000000Z",
        "updated_at": "2025-09-27T06:21:09.000000Z"
    }
}
```

---

### 🔹 3. Create Checklist

* **Endpoint:**
  `POST /api/mockdrill/checklist`

* **Payload:**

```json
{
    "name": "helio buddy",
    "type": "Department",
    "is_header": true
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 201,
    "message": "Checklist created successfully",
    "data": {
        "id": 2,
        "name": "helio buddy",
        "type": "Department",
        "is_header": true,
        "created_at": "2025-09-27T06:22:35.000000Z",
        "updated_at": "2025-09-27T06:22:35.000000Z"
    }
}
```

---

### 🔹 4. Update Checklist

* **Endpoint:**
  `PUT /api/mockdrill/checklist/{id}`

* **Payload:**

```json
{
    "name": "helio buddy",
    "type": "Department",
    "is_header": true
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Checklist updated successfully",
    "data": {
        "id": 2,
        "name": "helio buddy",
        "status": "active",
        "type": "Department",
        "is_header": 1,
        "created_at": "2025-09-27T06:22:35.000000Z",
        "updated_at": "2025-09-27T06:31:26.000000Z"
    }
}
```

---

## ⚡ **Usage Examples**

1. **Fetch All Checklists**

   ```bash
   curl {{site_url}}/api/mockdrill/checklist
   ```

2. **Fetch Single Checklist**

   ```bash
   curl {{site_url}}/api/mockdrill/checklist/1
   ```

3. **Create Checklist**

   ```bash
   curl -X POST {{site_url}}/api/mockdrill/checklist \
   -H "Content-Type: application/json" \
   -d '{"name":"helio buddy","type":"Department","is_header":true}'
   ```

4. **Update Checklist**

   ```bash
   curl -X PUT {{site_url}}/api/mockdrill/checklist/2 \
   -H "Content-Type: application/json" \
   -d '{"name":"helio buddy","type":"Department","is_header":true}'
   ```
