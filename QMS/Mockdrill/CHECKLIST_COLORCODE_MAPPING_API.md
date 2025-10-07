# 🧾 **Checklist–Color Code Mapping API Documentation**

This API manages **checklist assignments for color codes** in mock drills.
It allows you to **view**, **assign**, and **update** checklists mapped to a specific color code.

---

## ✅ **Database Table: `checklist_colorcode_mappings`**

| # | Column        | Type            | Attributes                   | Default | Description                  |
| - | ------------- | --------------- | ---------------------------- | ------- | ---------------------------- |
| 1 | id            | bigint UNSIGNED | Primary Key, Auto Increment  | —       | Unique mapping ID            |
| 2 | checklist_id  | bigint UNSIGNED | Foreign Key → checklists.id  | —       | Linked checklist             |
| 3 | color_code_id | bigint UNSIGNED | Foreign Key → color_codes.id | —       | Linked color code            |
| 4 | created_at    | timestamp       | Nullable                     | NULL    | Record creation timestamp    |
| 5 | updated_at    | timestamp       | Nullable                     | NULL    | Record last update timestamp |

---

## ✅ **Base URL**

```
{{site_url}}/api/mockdrill/{color_code_id}/mappings
```

---

## ✅ **Endpoints**

---

### 🔹 1. Get Mapped & Available Checklists

* **Endpoint:**

  ```
  GET /api/mockdrill/{color_code_id}/mappings
  ```

* **Description:**
  Retrieve a list of **checklists currently mapped** to a given color code and any **available checklists** that can be mapped.

* **Example Request:**

```
GET /api/mockdrill/1/mappings
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Color code mappings fetched successfully",
    "data": {
        "color_code_id": 1,
        "available_checklists": [],
        "mapped_checklists": [
            {
                "id": 2,
                "name": "helio"
            },
            {
                "id": 1,
                "name": "helio buddy"
            }
        ],
        "total_mapped": 2
    }
}
```

---

### 🔹 2. Assign or Update Checklist Mappings

* **Endpoint:**

  ```
  POST /api/mockdrill/{color_code_id}/mappings
  ```

* **Description:**
  Assign or update one or more checklists for a specific color code.
  Existing mappings will be replaced with the new list of `checklist_ids`.

* **Payload:**

```json
{
  "checklist_ids": [2, 1]
}
```

* **Example Response:**

```json
{
    "success": true,
    "status": 200,
    "message": "Color code checklist mappings updated successfully",
    "data": []
}
```

---

## ⚡ **Usage Examples**

1. **Fetch Mapped Checklists for a Color Code**

   ```bash
   curl {{site_url}}/api/mockdrill/1/mappings
   ```

2. **Update Checklist Mappings for a Color Code**

   ```bash
   curl -X POST {{site_url}}/api/mockdrill/1/mappings \
   -H "Content-Type: application/json" \
   -d '{"checklist_ids": [2,1]}'
   ```

---

## 🧩 **Relationships**

| Relation        | Table         | Description                                     |
| --------------- | ------------- | ----------------------------------------------- |
| `checklist_id`  | `checklists`  | Checklist linked to the color code              |
| `color_code_id` | `color_codes` | Color code defining mock drill type or severity |

---

## ⚙️ **Validation Rules**

| Field             | Type    | Required | Description                        |
| ----------------- | ------- | -------- | ---------------------------------- |
| `checklist_ids`   | array   | ✅ Yes    | Array of checklist IDs to assign   |
| `checklist_ids.*` | integer | ✅ Yes    | Each must exist in `checklists.id` |

