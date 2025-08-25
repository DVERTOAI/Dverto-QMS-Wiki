---

Category Management API Documentation

This API provides endpoints to manage Category-Subcategory Mappings in your application. All endpoints are protected by authentication via Laravel Sanctum.


---

Database Table Structure

The src_category_management table contains the following fields:

#	Name	Type	Attributes	Null	Default	Extra

1	id	bigint(20) unsigned	Primary Key	No	None	AUTO_INCREMENT
2	category_id	bigint(20) unsigned	Foreign Key	No	None	
3	subcategory_id	bigint(20) unsigned	Foreign Key	No	None	
4	created_at	timestamp		Yes	NULL	
5	updated_at	timestamp		Yes	NULL	


Foreign Key Constraints:

category_id references src_sources.id (where type = 'category')

subcategory_id references src_sources.id (where type = 'subcategory')

Unique constraint on (category_id, subcategory_id) to prevent duplicates


Sample Row:

id	category_id	subcategory_id	created_at	updated_at

4	1	2	2025-08-23 13:46:06	2025-08-23 13:46:06
5	1	4	2025-08-23 13:46:06	2025-08-23 13:46:06



---

Base URL

https://your-api-domain.com/api/category-management

Replace your-api-domain.com with your actual API domain.


---

Authentication

All endpoints require a valid Bearer token obtained from the login endpoint.

Required Headers:

Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com


---

Endpoints

1. Get All Categories

Endpoint: GET /api/category-management/categories
Description: Retrieve all active categories for dropdown selection.

Example Request:

curl -X GET "https://your-api-domain.com/api/category-management/categories" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"

Example Response:

{
  "success": true,
  "status": 200,
  "message": "Categories fetched successfully",
  "data": [
    { "id": 1, "name": "Campus Link" },
    { "id": 2, "name": "Online Portal" },
    { "id": 3, "name": "Student Services" }
  ]
}


---

2. Get All Subcategories

Endpoint: GET /api/category-management/subcategories
Description: Retrieve all active subcategories for reference.

Example Request:

curl -X GET "https://your-api-domain.com/api/category-management/subcategories" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"

Example Response:

{
  "success": true,
  "status": 200,
  "message": "Subcategories fetched successfully",
  "data": [
    { "id": 4, "name": "Faculty Portal" },
    { "id": 5, "name": "Library System" },
    { "id": 6, "name": "Admin Panel" }
  ]
}


---

3. Get Category Mappings

Endpoint: GET /api/category-management/mappings
Description: Get available and mapped subcategories for a specific category.

Query Parameters:

category_id (required): The category ID


Example Request:

curl -X GET "https://your-api-domain.com/api/category-management/mappings?category_id=1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"

Example Response:

{
  "success": true,
  "status": 200,
  "message": "Category mappings fetched successfully",
  "data": {
    "category_id": 1,
    "available_subcategories": [
      { "id": 5, "name": "Library System" },
      { "id": 6, "name": "Admin Panel" }
    ],
    "mapped_subcategories": [
      { "id": 4, "name": "Faculty Portal" }
    ],
    "total_mapped": 1
  }
}


---

4. Update Category Mappings

Endpoint: POST /api/category-management/add-subcategory
Description: Update category-subcategory mappings by providing an array of subcategory IDs.

Request Body Fields:

category_id (required): The category ID

subcategory_id (required): Array of subcategory IDs


Behavior:

Adds subcategories not currently mapped

Removes subcategories not in the array

Keeps subcategories present in both

Empty array removes all mappings


Example Request:

curl -X POST "https://your-api-domain.com/api/category-management/add-subcategory" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"category_id":"1","subcategory_id":[4,5,6]}'

Example Response:

{
  "success": true,
  "status": 200,
  "message": "Category subcategories updated successfully",
  "data": {
    "category_id": 1,
    "available_subcategories": [],
    "mapped_subcategories": [
      { "id": 4, "name": "Faculty Portal" },
      { "id": 5, "name": "Library System" },
      { "id": 6, "name": "Admin Panel" }
    ],
    "total_mapped": 3
  }
}

Example (Remove All):

curl -X POST "https://your-api-domain.com/api/category-management/add-subcategory" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"category_id":"1","subcategory_id":[]}'

Validation Errors:

{
  "message": "The given data was invalid.",
  "errors": {
    "category_id": ["Category is required."],
    "subcategory_id": ["At least one subcategory must be selected."]
  }
}


---

Business Rules

One-to-Many: One category can map to multiple subcategories

Many-to-Many: One subcategory can be mapped to multiple categories

Unique Constraint: Each pair must be unique

Active Only: Only active categories/subcategories allowed

Atomic Updates: All changes happen in a transaction


Validation Rules:

category_id must exist and be active

subcategory_id must be an array of active subcategories

IDs must be integers



---

Summary Table

Method	Endpoint	Description

GET	/api/category-management/categories	Get all active categories
GET	/api/category-management/subcategories	Get all active subcategories
GET	/api/category-management/mappings	Get category mappings
POST	/api/category-management/add-subcategory	Update category-subcategory mappings



---

Error Codes

HTTP Code	Description

200	Success
400	Bad Request / Business Logic Error
401	Unauthorized
404	Category not found
422	Unprocessable Entity (Validation)
500	Internal Server Error



---

Use Cases

1. Initial Setup: Load categories and show mapped/available subcategories.


2. Adding Subcategories: Move items to mapped and save.


3. Removing Subcategories: Move items back to available and save.


4. Bulk Operations: Update multiple mappings in one call.


5. Reset/Clear All: Send an empty array to clear all mappings.




---

Notes

All routes require Sanctum token in Authorization header.

Include domain: psri.com header in all requests.

Responses and errors are JSON formatted.

Updates are transactional for data integrity.


