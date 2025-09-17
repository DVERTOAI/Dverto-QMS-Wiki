# ✅ **Indicator Actions API Documentation**

These APIs provide functionality to perform bulk updates and fetch indicator actions for departments.

---

## ✅ **Base URL**

```
{{site_url}}/api/indicator/bulk-update  
{{site_url}}/api/indicator/actions
```

---

## ✅ **Endpoints**

---

### 🔹 1. Bulk Update Department Indicator Data

* **Endpoint:**
  `POST /api/indicator/bulk-update`

* **Description:**
  Perform bulk updates to multiple department indicator records. The `id` refers to the **department indicator record ID**.

* **Payload Example:**

```json
{
    "updates": [
        { "id": 1, "entered_data": 55.5, "calculated_value": 60.0 }
    ]
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicators with actions fetched successfully",
    "data": {
        "departments": [ ... ],
        "indicators": [ ... ],
        "selected_department_id": "1"
    }
}
```

---

### 🔹 2. Get Indicators with Actions by Department ID

* **Endpoint:**
  `GET /api/indicator/actions?department_id={department_id}`

* **Description:**
  Retrieves the list of departments and indicators assigned to a specific department, including their descriptions and department-indicator assignments.

* **Example URL:**
  `GET {{site_url}}/api/indicator/actions?department_id=1`

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Indicators with actions fetched successfully",
    "data": {
        "departments": [
            { "id": 4, "name": "basement lab", ... },
            { "id": 1, "name": "Floor", ... },
            { "id": 2, "name": "ICU", ... },
            { "id": 3, "name": "Surgery", ... }
        ],
        "indicators": [
            {
                "id": 1,
                "name": "Patient Safety",
                "unit": "Percentage",
                "frequency": "Monthly",
                "formula": "(a1+b1)/2",
                "status": "Active",
                "descriptions": [
                    { "id": 1, "name": "D100", "title": "Patient Satisfaction Score", ... },
                    { "id": 2, "name": "D13", "title": "Score", ... }
                ],
                "department_indicators": [
                    {
                        "id": 1,
                        "indicator_id": 1,
                        "indicator_description_id": 1,
                        "department_id": 1,
                        "entered_data": "55.5",
                        "calculated_value": "60",
                        ...
                    },
                    ...
                ]
            }
        ],
        "selected_department_id": "1"
    }
}
```

---

## ✅ **Data Summary**

| Field                     | Description                                                                    |
| ------------------------- | ------------------------------------------------------------------------------ |
| updates                   | Array of update objects with `id`, `entered_data`, and `calculated_value`.     |
| departments               | List of all departments.                                                       |
| indicators                | List of all indicators with descriptions and department indicator assignments. |
| selected\_department\_id  | The department currently selected for fetching data.                           |
| department\_indicators.id | Unique ID of the department-indicator assignment record.                       |
| entered\_data             | Manually entered KPI data.                                                     |
| calculated\_value         | Calculated KPI value based on formula.                                         |

---

## ⚡ Usage Examples

1. **Bulk Update Department Indicator Data:**

   ```bash
   curl -X POST {{site_url}}/api/indicator/bulk-update \
   -H "Content-Type: application/json" \
   -d '{
         "updates": [
           { "id": 1, "entered_data": 55.5, "calculated_value": 60.0 }
         ]
       }'
   ```

2. **Fetch Indicator Actions for Department 1:**

   ```bash
   curl "{{site_url}}/api/indicator/actions?department_id=1"
   ```

