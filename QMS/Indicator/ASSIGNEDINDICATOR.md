
# 📚 **Department Indicators API Documentation**

This API provides endpoints to manage **Department Indicators**, which link indicators and indicator descriptions to departments, with formulas and data.

---

## ✅ **Database Table Structure**

The `department_indicators` table structure:

| # | Name                       | Type                           | Attributes                            | Null | Default | Extra           |
| - | -------------------------- | ------------------------------ | ------------------------------------- | ---- | ------- | --------------- |
| 1 | id                         | bigint(20) unsigned            | Primary Key                           | No   | None    | AUTO\_INCREMENT |
| 2 | indicator\_id              | bigint(20) unsigned            | Foreign Key (indicators)              | No   | None    | cascadeOnDelete |
| 3 | indicator\_description\_id | bigint(20) unsigned (nullable) | Foreign Key (indicator\_descriptions) | Yes  | NULL    | cascadeOnDelete |
| 4 | department\_id             | bigint(20) unsigned            | Foreign Key (departments)             | No   | None    | cascadeOnDelete |
| 5 | entered\_data              | string (nullable)              |                                       | Yes  | NULL    |                 |
| 6 | calculated\_value          | string (nullable)              |                                       | Yes  | NULL    |                 |
| 7 | created\_at                | timestamp                      |                                       | Yes  | NULL    |                 |
| 8 | updated\_at                | timestamp                      |                                       | Yes  | NULL    |                 |

---

## 🌐 **Base URL**

```
{{site_url}}/api/indicator/assign
{{site_url}}/api/indicator/description
{{site_url}}/api/indicator/assign/formula
```

---

## ✅ **Endpoints**

### 🔹 1. Get Indicator Descriptions by Indicator ID

* **Endpoint:**
  `GET /api/indicator/description?indicator_id={id}`

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicator Descriptions fetched successfully",
    "data": {
        "data": [
            {
                "id": 5,
                "indicator_id": 3,
                "name": "D16",
                "title": "Denominor 2",
                "status": "active",
                "nabh_standard": "Standard XYZ",
                "indicator": { "id": 3, "name": "Safety s2", ... }
            },
            {
                "id": 4,
                "indicator_id": 3,
                "name": "D15",
                "title": "Denominor 1",
                "status": "active",
                "nabh_standard": "Standard XYZ",
                "indicator": { "id": 3, "name": "Safety s2", ... }
            }
        ]
    }
}
```

---

### 🔹 2. Assign Indicator to Department

* **Endpoint:**
  `POST /api/indicator/assign`

* **Payload Example:**

```json
{
    "indicator_id": 1,
    "department_id": 2,
    "formula": "(D11/D12)*100"
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicator assigned to department successfully",
    "data": null
}
```

---

### 🔹 3. List All Department Indicators

* **Endpoint:**
  `GET /api/indicator/assign`

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Department Indicators fetched successfully",
    "data": {
        "data": [
            {
                "id": 9,
                "indicator_id": 1,
                "indicator_description_id": 2,
                "department_id": 1,
                "entered_data": null,
                "calculated_value": null,
                "indicator": {
                    "id": 1,
                    "name": "Patient Safety",
                    "unit": "Percentage",
                    "frequency": "Monthly",
                    "formula": "(D11/D12)*100",
                    "status": "Active"
                },
                "department": {
                    "id": 1,
                    "name": "Floor",
                    "status": "active"
                }
            },
            {
                "id": 1,
                "indicator_id": 1,
                "indicator_description_id": 1,
                "department_id": 1,
                "entered_data": "55.5",
                "calculated_value": "60",
                "indicator": { ... },
                "department": { ... }
            }
        ]
    }
}
```

---

### 🔹 4. Update Department Indicator Formula

* **Endpoint:**
  `PUT /api/indicator/assign/formula`

* **Payload Example:**

```json
{
    "assignmentId": 1,
    "formula": "(a1+b1)/2"
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Formula updated successfully",
    "data": null
}
```

---

## ⚡ Summary Table

| Method | Endpoint                      | Description                                 |
| ------ | ----------------------------- | ------------------------------------------- |
| GET    | /api/indicator/description    | Get descriptions by `indicator_id`          |
| POST   | /api/indicator/assign         | Assign indicator to department with formula |
| GET    | /api/indicator/assign         | List all department indicators              |
| PUT    | /api/indicator/assign/formula | Update formula of an assigned indicator     |

---

## ✅ Notes

* Assignment formula supports any custom formula string.
* Related models are returned in the response for easy display in frontend.
* Pagination supported by default in listing endpoints.

