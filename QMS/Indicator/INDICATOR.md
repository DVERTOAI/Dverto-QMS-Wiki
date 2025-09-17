# 📊 **Indicator API Documentation**

This API provides endpoints to manage **Indicators** in your application.

---

## ✅ **Database Table Structure**

The `indicators` table structure:

| # | Name        | Type                                                      | Attributes  | Null | Default   | Extra           |
| - | ----------- | --------------------------------------------------------- | ----------- | ---- | --------- | --------------- |
| 1 | id          | bigint(20) unsigned                                       | Primary Key | No   | None      | AUTO\_INCREMENT |
| 2 | name        | varchar(255)                                              | Unique      | No   | None      |                 |
| 3 | unit        | varchar(255)                                              |             | Yes  | NULL      |                 |
| 4 | frequency   | enum('Daily', 'Weekly', 'Monthly', 'Quarterly', 'Yearly') |             | No   | 'Monthly' |                 |
| 5 | formula     | string (nullable)                                         |             | Yes  | NULL      |                 |
| 6 | status      | enum('Active', 'Inactive')                                |             | No   | 'Active'  |                 |
| 7 | created\_at | timestamp                                                 |             | Yes  | NULL      |                 |
| 8 | updated\_at | timestamp                                                 |             | Yes  | NULL      |                 |

---

## 🌐 **Base URL**

```
{{site_url}}/api/indicators
```

---

## ✅ **Endpoints**

### 🔹 1. Create Indicator

* **Endpoint:** `POST /api/indicators`
* **Description:** Create a new indicator.

**Payload Example:**

```json
{
    "name": "Safety s2",
    "unit": "Percentage",
    "frequency": "Monthly",
    "status": "active"
}
```

**Example Response:**

```json
{
    "success": true,
    "status": 201,
    "message": "Indicator created successfully",
    "data": {
        "name": "Safety s3",
        "unit": "Percentage",
        "frequency": "Monthly",
        "status": "active",
        "updated_at": "2025-09-17T13:45:06.000000Z",
        "created_at": "2025-09-17T13:45:06.000000Z",
        "id": 4
    }
}
```

---

### 🔹 2. Get All Indicators (Paginated)

* **Endpoint:** `GET /api/indicators`
* **Description:** Retrieve a paginated list of indicators.

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicators fetched successfully",
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 4,
                "name": "Safety s3",
                "unit": "Percentage",
                "frequency": "Monthly",
                "formula": null,
                "status": "Active",
                "created_at": "2025-09-17T13:45:06.000000Z",
                "updated_at": "2025-09-17T13:45:06.000000Z"
            },
            {
                "id": 3,
                "name": "Safety s2",
                "unit": "Percentage",
                "frequency": "Monthly",
                "formula": null,
                "status": "Active",
                "created_at": "2025-09-17T13:09:08.000000Z",
                "updated_at": "2025-09-17T13:09:08.000000Z"
            },
            {
                "id": 2,
                "name": "Safety s1",
                "unit": "Minut",
                "frequency": "Monthly",
                "formula": null,
                "status": "Active",
                "created_at": "2025-09-17T13:08:48.000000Z",
                "updated_at": "2025-09-17T13:08:48.000000Z"
            },
            {
                "id": 1,
                "name": "indicator Safety",
                "unit": "Percentage",
                "frequency": "Monthly",
                "formula": "(A + B) / 2",
                "status": "Active",
                "created_at": "2025-09-16T14:29:40.000000Z",
                "updated_at": "2025-09-17T08:35:18.000000Z"
            }
        ],
        "first_page_url": "...",
        "last_page_url": "...",
        "next_page_url": null,
        "prev_page_url": null,
        "per_page": 10,
        "total": 4
    }
}
```

---

### 🔹 3. Get Single Indicator

* **Endpoint:** `GET /api/indicators/{id}`
* **Description:** Retrieve details of a specific indicator by ID.

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Success",
    "data": {
        "id": 1,
        "name": "indicator Safety",
        "unit": "Percentage",
        "frequency": "Monthly",
        "formula": "(A + B) / 2",
        "status": "Active",
        "created_at": "2025-09-16T14:29:40.000000Z",
        "updated_at": "2025-09-17T08:35:18.000000Z"
    }
}
```

---

### 🔹 4. Update Indicator

* **Endpoint:** `PUT /api/indicators/{id}`
* **Description:** Update an existing indicator.

**Payload Example:**

```json
{
    "name": "Patient Safety",
    "unit": "Percentage",
    "frequency": "Monthly",
    "status": "active"
}
```

**Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicator updated successfully",
    "data": {
        "id": 1,
        "name": "Patient Safety",
        "unit": "Percentage",
        "frequency": "Monthly",
        "formula": "(A + B) / 2",
        "status": "active",
        "created_at": "2025-09-16T14:29:40.000000Z",
        "updated_at": "2025-09-17T13:46:57.000000Z"
    }
}
```

---

## ⚡ Summary Table

| Method | Endpoint             | Description                     |
| ------ | -------------------- | ------------------------------- |
| POST   | /api/indicators      | Create new indicator            |
| GET    | /api/indicators      | List all indicators (paginated) |
| GET    | /api/indicators/{id} | Get indicator by ID             |
| PUT    | /api/indicators/{id} | Update indicator                |

---

## ✅ Notes

* **frequency field valid values:** `Daily`, `Weekly`, `Monthly`, `Quarterly`, `Yearly`
* **status field valid values:** `Active`, `Inactive`
* API responses use standard success structure with `success`, `status`, `message`, and `data`.

