# Category Management API Documentation

This API provides endpoints to manage **Category-Subcategory Mappings** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `src_category_management` table contains the following fields:

| #   | Name           | Type                                    | Attributes     | Null | Default | Extra           |
|-----|----------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | category_id    | bigint(20) unsigned                     | Foreign Key   | No   | None    |                 |
| 3   | subcategory_id | bigint(20) unsigned                     | Foreign Key   | No   | None    |                 |
| 4   | created_at     | timestamp                               |               | Yes  | NULL    |                 |
| 5   | updated_at     | timestamp                               |               | Yes  | NULL    |                 |

**Foreign Key Constraints:**
- `category_id` references `src_sources.id` (where type='category')
- `subcategory_id` references `src_sources.id` (where type='subcategory')
- Unique constraint on `(category_id, subcategory_id)` to prevent duplicates

**Sample Row:**

| id | category_id | subcategory_id | created_at           | updated_at           |
|----|-------------|----------------|----------------------|----------------------|
| 4  | 1           | 2              | 2025-08-23 13:46:06  | 2025-08-23 13:46:06  |
| 5  | 1           | 4              | 2025-08-23 13:46:06  | 2025-08-23 13:46:06  |

---

## Base URL

```
https://your-api-domain.com/api/category-management
```
*Replace `your-api-domain.com` with your actual API domain.*

---

## Authentication

All endpoints require a valid Bearer token obtained from the login endpoint.

**Required Headers:**
```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```
- Replace `<your_token>` with the token received from your login response.

---

## Endpoints

### 1. Get All Categories

- **Endpoint:** `GET /api/category-management/categories`
- **Description:** Get all active categories for dropdown selection.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/category-management/categories" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response:**
```json
{
  "success": true,
  "status": 200,
  "message": "Categories fetched successfully",
  "data": [
    {
      "id": 1,
      "name": "Campus Link"
    },
    {
      "id": 2,
      "name": "Online Portal"
    },
    {
      "id": 3,
      "name": "Student Services"
    }
  ]
}
```

---

### 2. Get All Subcategories

- **Endpoint:** `GET /api/category-management/subcategories`
- **Description:** Get all active subcategories for reference.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/category-management/subcategories" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response:**
```json
{
  "success": true,
  "status": 200,
  "message": "Subcategories fetched successfully",
  "data": [
    {
      "id": 4,
      "name": "Faculty Portal"
    },
    {
      "id": 5,
      "name": "Library System"
    },
    {
      "id": 6,
      "name": "Admin Panel"
    }
  ]
}
```

---

### 3. Get Category Mappings

- **Endpoint:** `GET /api/category-management/mappings`
- **Description:** Get available and mapped subcategories for a specific category.

**Query Parameters:**
- `category_id` (required): The category ID to get mappings for

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/category-management/mappings?category_id=1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response:**
```json
{
  "success": true,
  "status": 200,
  "message": "Category mappings fetched successfully",
  "data": {
    "category_id": 1,
    "available_subcategories": [
      {
        "id": 5,
        "name": "Library System"
      },
      {
        "id": 6,
        "name": "Admin Panel"
      }
    ],
    "mapped_subcategories": [
      {
        "id": 4,
        "name": "Faculty Portal"
      }
    ],
    "total_mapped": 1
  }
}
```

---

### 4. Update Category Mappings

- **Endpoint:** `POST /api/category-management/add-subcategory`
- **Description:** Update category-subcategory mappings by providing an array of subcategory IDs. This endpoint handles both adding and removing mappings based on the provided array.

**Request Body Fields:**
- `category_id` (required): The category ID (must be an active category)
- `subcategory_id` (required): Array of subcategory IDs (each must be an active subcategory)

**Behavior:**
- **Adds** subcategories that are in the array but not currently mapped
- **Removes** subcategories that are currently mapped but not in the array
- **Keeps** subcategories that are both currently mapped and in the array
- **Empty array** removes all mappings for the category

**Example Request Body:**
```json
{
  "category_id": "1",
  "subcategory_id": [4, 5, 6]
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/category-management/add-subcategory" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"category_id":"1","subcategory_id":[4,5,6]}'
```

**Example Response:**
```json
{
  "success": true,
  "status": 200,
  "message": "Category subcategories updated successfully",
  "data": {
    "category_id": 1,
    "available_subcategories": [],
    "mapped_subcategories": [
      {
        "id": 4,
        "name": "Faculty Portal"
      },
      {
        "id": 5,
        "name": "Library System"
      },
      {
        "id": 6,
        "name": "Admin Panel"
      }
    ],
    "total_mapped": 3
  }
}
```

**Remove All Mappings (Empty Array):**
```bash
curl -X POST "https://your-api-domain.com/api/category-management/add-subcategory" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"category_id":"1","subcategory_id":[]}'
```

**Validation Error Example (Missing Fields):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "category_id": ["Category is required."],
    "subcategory_id": ["At least one subcategory must be selected."]
  }
}
```

**Validation Error Example (Invalid Category):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "category_id": [
      "Selected category does not exist or is not active."
    ]
  }
}
```

**Validation Error Example (Non-Array Format):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "subcategory_id": [
      "Subcategories must be provided as an array."
    ]
  }
}
```

**Validation Error Example (Invalid Subcategory):**
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "subcategory_id.1": [
      "One or more selected subcategories do not exist or are not active."
    ]
  }
}

---

## Business Rules

### Category-Subcategory Mapping Rules
- **One-to-Many**: One category can have multiple subcategories mapped to it
- **Many-to-Many**: One subcategory can be mapped to multiple categories
- **Unique Constraint**: Each category-subcategory pair can only exist once in the database
- **Active Only**: Only active categories and subcategories can be used in mappings
- **Atomic Updates**: All mapping changes for a category happen in a single transaction

### Validation Rules
- **Category ID**: Must exist in `src_sources` table with `type='category'` and `status='active'`
- **Subcategory IDs**: Each must exist in `src_sources` table with `type='subcategory'` and `status='active'`
- **Array Format**: Subcategory IDs must be provided as an array (empty array is valid)
- **Integer Values**: All IDs must be valid integers

---

## Summary Table

| Method | Endpoint                                    | Description                           |
|--------|---------------------------------------------|---------------------------------------|
| GET    | /api/category-management/categories         | Get all active categories             |
| GET    | /api/category-management/subcategories      | Get all active subcategories          |
| GET    | /api/category-management/mappings           | Get category mappings                 |
| POST   | /api/category-management/add-subcategory    | Update category-subcategory mappings  |


## Error Codes

| HTTP Code | Description                           |
|-----------|---------------------------------------|
| 200       | Success                               |
| 400       | Bad Request / Business Logic Error    |
| 401       | Unauthorized                          |
| 404       | Category not found                    |
| 422       | Unprocessable Entity (Validation)     |
| 500       | Internal Server Error                 |

---

## Use Cases

### 1. **Initial Setup**
- Load categories in dropdown
- User selects a category
- Display available and mapped subcategories

### 2. **Adding Subcategories**
- User moves items from available to mapped list
- Click "Save Changes" to persist

### 3. **Removing Subcategories**
- User moves items from mapped to available list
- Click "Save Changes" to persist

### 4. **Bulk Operations**
- User can move multiple items at once
- Single API call updates all mappings

### 5. **Reset/Clear All**
- Send empty array to remove all mappings
- All subcategories return to available list

---

## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **Content-Type:** Always set to `application/json`.
- **Error Handling:** All error responses are in JSON format with detailed validation messages.
- **Tenant Connection:** All operations use the tenant database connection.
- **Transaction Safety:** All mapping updates happen within database transactions.
- **Real-time Updates:** API returns current state after each update operation.

---

## Contact

For any questions or support, contact the backend development team.
```
