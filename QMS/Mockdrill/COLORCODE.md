# 🎨 **Color Code API Documentation**

This API provides endpoints to manage **Color Codes** including creating, updating, and retrieving color code records.

---

## ✅ **Database Table: `color_codes`**

| # | Column     | Type                      | Attributes                  | Default | Description                            |
| - | ---------- | ------------------------- | --------------------------- | ------- | -------------------------------------- |
| 1 | id         | bigint UNSIGNED           | Primary Key, Auto Increment | —       | Unique color code ID                   |
| 2 | name       | varchar(255)              | Not Null                    | —       | Color name (e.g., Danger, Success)     |
| 3 | hex_color  | varchar(255)              | Not Null                    | —       | Hex value of the color (e.g., #FF4C4C) |
| 4 | status     | enum('active','inactive') | Not Null                    | active  | Color code status                      |
| 5 | created_at | timestamp                 | Nullable                    | NULL    | Record creation timestamp              |
| 6 | updated_at | timestamp                 | Nullable                    | NULL    | Record last update timestamp           |

---

## ✅ **Base URL**

```
{{site_url}}/api/color-codes
```

---

## ✅ **Endpoints**

---

### 🔹 1. Get All Color Codes

* **Endpoint:**
  `GET /api/color-codes`

* **Description:**
  Fetch a paginated list of all color codes.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Color codes fetched successfully",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 1,
                "name": "Danger",
                "hex_color": "#FF4C4C",
                "status": "active",
                "created_at": "2025-09-27T06:50:00.000000Z",
                "updated_at": "2025-09-27T06:50:00.000000Z"
            },
            {
                "id": 2,
                "name": "Success",
                "hex_color": "#28A745",
                "status": "active",
                "created_at": "2025-09-27T06:55:00.000000Z",
                "updated_at": "2025-09-27T06:55:00.000000Z"
            }
        ],
        "per_page": 10,
        "total": 2
    }
}
```

---

### 🔹 2. Get Single Color Code

* **Endpoint:**
  `GET /api/color-codes/{id}`

* **Description:**
  Retrieve details of a specific color code by ID.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Success",
    "data": {
        "id": 1,
        "name": "Danger",
        "hex_color": "#FF4C4C",
        "status": "active",
        "created_at": "2025-09-27T06:50:00.000000Z",
        "updated_at": "2025-09-27T06:50:00.000000Z"
    }
}
```

---

### 🔹 3. Create Color Code

* **Endpoint:**
  `POST /api/color-codes`

* **Payload:**

```json
{
    "name": "Danger",
    "hex_color": "#FF4C4C"
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 201,
    "message": "Color code created successfully",
    "data": {
        "id": 1,
        "name": "Danger",
        "hex_color": "#FF4C4C",
        "status": "active",
        "created_at": "2025-09-27T06:50:00.000000Z",
        "updated_at": "2025-09-27T06:50:00.000000Z"
    }
}
```

---

### 🔹 4. Update Color Code

* **Endpoint:**
  `PUT /api/color-codes/{id}`

* **Payload:**

```json
{
    "name": "Danger Updated",
    "hex_color": "#FF3333"
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Color code updated successfully",
    "data": {
        "id": 1,
        "name": "Danger Updated",
        "hex_color": "#FF3333",
        "status": "active",
        "created_at": "2025-09-27T06:50:00.000000Z",
        "updated_at": "2025-09-27T07:05:00.000000Z"
    }
}
```

---

## ⚡ **Usage Examples**

1. **Fetch All Color Codes**

   ```bash
   curl {{site_url}}/api/color-codes
   ```

2. **Fetch Single Color Code**

   ```bash
   curl {{site_url}}/api/color-codes/1
   ```

3. **Create Color Code**

   ```bash
   curl -X POST {{site_url}}/api/color-codes \
   -H "Content-Type: application/json" \
   -d '{"name":"Danger","hex_color":"#FF4C4C"}'
   ```

4. **Update Color Code**

   ```bash
   curl -X PUT {{site_url}}/api/color-codes/1 \
   -H "Content-Type: application/json" \
   -d '{"name":"Danger Updated","hex_color":"#FF3333"}'
   ```
