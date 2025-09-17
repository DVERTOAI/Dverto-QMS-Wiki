Here is the structured **wiki documentation** for your KPI Graph API:

---

# 📊 **KPI Graph API Documentation**

This API provides endpoints to fetch KPI graph metadata and KPI chart data for departments.

---

## ✅ **Base URL**

```
{{site_url}}/api/indicator/kpi-graph-data
{{site_url}}/api/indicator/kpi-chart-data
```

---

## ✅ **Endpoints**

---

### 🔹 1. Get KPI Graph Metadata

* **Endpoint:**
  `GET /api/indicator/kpi-graph-data`

* **Description:**
  Fetches a list of all departments and available years for KPI graph representation.

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "KPI graph data fetched successfully",
    "data": {
        "departments": [
            { "id": 4, "name": "basement lab", "status": "active", ... },
            { "id": 1, "name": "Floor", "status": "active", ... },
            { "id": 2, "name": "ICU", "status": "active", ... },
            { "id": 3, "name": "Surgery", "status": "active", ... }
        ],
        "years": [2025]
    }
}
```

---

### 🔹 2. Get KPI Chart Data

* **Endpoint:**
  `GET /api/indicator/kpi-chart-data?department_id={department_id}&year={year}`

* **Description:**
  Retrieves KPI data for the selected department and year in chart-friendly format.

* **Query Parameters:**

  * `department_id`: ID of the department (e.g., 1)
  * `year`: Year to filter data (e.g., 2025)

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "KPI chart data fetched successfully",
    "data": {
        "labels": [
            "Jan", "Feb", "Mar", "Apr", "May", "Jun",
            "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
        ],
        "dataset": [
            null, null, null, null, null, null, null, null, 60, null, null, null
        ],
        "department_id": "1",
        "year": "2025"
    }
}
```

---

## ✅ **Data Summary**

| Field          | Description                                     |
| -------------- | ----------------------------------------------- |
| departments    | List of available departments for selection     |
| years          | List of available years for KPI data            |
| labels         | List of months (Jan-Dec)                        |
| dataset        | KPI values for each month (nullable if no data) |
| department\_id | The selected department ID                      |
| year           | The selected year for KPI data                  |

---

## ⚡ Usage Example

1. **Fetch Graph Metadata:**

   ```bash
   curl {{site_url}}/api/indicator/kpi-graph-data
   ```

2. **Fetch KPI Chart Data for Department 1 in Year 2025:**

   ```bash
   curl "{{site_url}}/api/indicator/kpi-chart-data?department_id=1&year=2025"
   ```

