# 📚 **Indicator Descriptions API Documentation**

This API provides endpoints to manage **Indicator Descriptions** in your application.

---

## ✅ **Database Table Structure**

The `indicator_descriptions` table structure:

| # | Name           | Type                    | Attributes               | Null | Default | Extra           |
| - | -------------- | ----------------------- | ------------------------ | ---- | ------- | --------------- |
| 1 | id             | bigint(20) unsigned     | Primary Key              | No   | None    | AUTO\_INCREMENT |
| 2 | indicator\_id  | bigint(20) unsigned     | Foreign Key (indicators) | No   | None    | cascadeOnDelete |
| 3 | name           | varchar(255)            |                          | No   | None    |                 |
| 4 | title          | varchar(255)            |                          | No   | None    |                 |
| 5 | status         | enum(active, inactive)  | Default: active          | No   | active  |                 |
| 6 | nabh\_standard | varchar(255) (nullable) |                          | Yes  | NULL    |                 |
| 7 | created\_at    | timestamp               |                          | Yes  | NULL    |                 |
| 8 | updated\_at    | timestamp               |                          | Yes  | NULL    |                 |

---

## 🌐 **Base URL**

```
{{site_url}}/api/indicator/description
```

---

## ✅ **Endpoints**

### 🔹 1. Create Indicator Description

* **Endpoint:** `POST /api/indicator/description`
* **Description:** Create a new indicator description.

**Payload Example:**

```json
{
    "indicator_id": 4,
    "name": "D17",
    "title": "Denominor 2",
    "status": "active",
    "nabh_standard": "Standard XYZ"
}
```

**Example Response:**

```json
{
    "success": true,
    "status": 201,
    "message": "Indicator Description created successfully",
    "data": {
        "indicator_id": 4,
        "name": "D17",
        "title": "Denominor 2",
        "status": "active",
        "nabh_standard": "Standard XYZ",
        "updated_at": "2025-09-17T13:53:14.000000Z",
        "created_at": "2025-09-17T13:53:14.000000Z",
        "id": 6
    }
}
```

---

### 🔹 2. Get All Indicator Descriptions (Paginated)

* **Endpoint:** `GET /api/indicator/description`
* **Description:** Fetch all indicator descriptions with pagination.

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicator Descriptions fetched successfully",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 6,
                "indicator_id": 4,
                "name": "D17",
                "title": "Denominor 2",
                "status": "active",
                "nabh_standard": "Standard XYZ",
                "created_at": "2025-09-17T13:53:14.000000Z",
                "updated_at": "2025-09-17T13:53:14.000000Z",
                "indicator": {
                    "id": 4,
                    "name": "Safety s3",
                    "unit": "Percentage",
                    "frequency": "Monthly",
                    "formula": null,
                    "status": "active",
                    "created_at": "2025-09-17T13:45:06.000000Z",
                    "updated_at": "2025-09-17T13:45:06.000000Z"
                }
            },
            // ... Other descriptions
        ],
        "first_page_url": "...",
        "last_page_url": "...",
        "next_page_url": null,
        "prev_page_url": null,
        "per_page": 10,
        "total": 6
    }
}
```

---

### 🔹 3. Get Single Indicator Description

* **Endpoint:** `GET /api/indicator/description/{id}`
* **Description:** Get a single indicator description by ID.

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Success",
    "data": {
        "id": 1,
        "indicator_id": 1,
        "name": "D12",
        "title": "Satisfaction Score",
        "status": "active",
        "nabh_standard": "Standard XYZ",
        "created_at": "2025-09-16T14:30:01.000000Z",
        "updated_at": "2025-09-16T14:30:01.000000Z"
    }
}
```

---

### 🔹 4. Update Indicator Description

* **Endpoint:** `POST /api/indicator/description/{id}`
* **Description:** Update an existing indicator description.

**Payload Example:**

```json
{
    "indicator_id": 1,
    "name": "D100",
    "title": "Patient Satisfaction Score",
    "status": "active",
    "nabh_standard": "Standard XYZ"
}
```

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicator Description updated successfully",
    "data": {
        "id": 1,
        "indicator_id": 1,
        "name": "D100",
        "title": "Patient Satisfaction Score",
        "status": "active",
        "nabh_standard": "Standard XYZ",
        "created_at": "2025-09-16T14:30:01.000000Z",
        "updated_at": "2025-09-17T13:55:52.000000Z"
    }
}
```

---

## ⚡ Summary Table

| Method | Endpoint                        | Description                       |
| ------ | ------------------------------- | --------------------------------- |
| POST   | /api/indicator/description      | Create a new description          |
| GET    | /api/indicator/description      | List all descriptions (paginated) |
| GET    | /api/indicator/description/{id} | Get a description by ID           |
| post    | /api/indicator/description/{id} | Update a description              |

---

## ✅ Notes

* `status` valid values: `active`, `inactive`
* The `indicator_id` links to the `indicators` table.
* Paginated listing returns indicator description + related indicator data.
* API follows standard success response format:
  `{ success, status, message, data }`
