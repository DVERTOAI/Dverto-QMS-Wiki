# Enhancement Proposal: Add Searching, Sorting, and Pagination to API Endpoints

## Overview

To improve the usability and scalability of API endpoints, we propose adding **searching**, **sorting**, and improved **pagination** features. These enhancements will allow clients to filter, sort, and efficiently page through data based on their needs.

---

## Proposed Features

### 1. Searching
- **Feature:** Filter data by name or related fields.
- **How:** Accept query parameters like `search` that match against specified fields.
- **Example Request:**
  ```http
  GET /api/resources?search=account
  ```

### 2. Sorting
- **Feature:** Sort data by any column (e.g., `name`, `status`).
- **How:** Accept `sort_by` and `sort_order` query parameters.
  - `sort_by`: field name to sort (e.g., `name`)
  - `sort_order`: `asc` or `desc`
- **Example Request:**
  ```http
  GET /api/resources?sort_by=name&sort_order=asc
  ```

### 3. Pagination
- **Feature:** Allow clients to set the number of results per page and select the page.
- **How:** Accept `page` and `per_page` query parameters.
- **Defaults:**
  - `per_page` default: 10
  - `page` default: 1
- **Example Request:**
  ```http
  GET /api/resources?page=2&per_page=25
  ```

---

## Example: Combined Usage

```http
GET /api/resources?search=production&sort_by=created_at&sort_order=desc&page=1&per_page=5
```

---

## Updated Endpoint Documentation

### List All Resources (with Search, Sort, Pagination)

**Endpoint:** `GET /api/resources`

**Query Parameters:**

| Name        | Type    | Description                                               |
|-------------|---------|-----------------------------------------------------------|
| search      | string  | Search term for resource name or related fields          |
| sort_by     | string  | Field to sort by (`name`, `status`, etc.)   |
| sort_order  | string  | `asc` or `desc`                                           |
| page        | int     | Page number (default: 1)                                  |
| per_page    | int     | Results per page (default: 10)                            |

**Example Request:**

```bash
curl -X GET "https://your-api-domain.com/api/resources?search=account&sort_by=name&sort_order=asc&page=1&per_page=10" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>"
```

**Example Response:**

*(Response structure remains as in your original documentation, but results are filtered/sorted/paginated as requested.)*

---

## Notes

- All new query parameters are **optional**. If omitted, the API returns default paginated results.
- Searching and sorting should be **case-insensitive** for user-friendliness.
- The backend should validate and sanitize all input to prevent SQL injection or errors.

---

## Implementation Steps (Backend)

1. Update the controller method to handle and validate the new query parameters.
2. Apply search filter using Eloquent/Laravel query scopes.
3. Add sorting logic by column and direction.
4. Use Laravel's built-in pagination with custom per-page support.
5. Update API documentation and client SDKs if needed.
6. Add tests for the new features.

---

## Contact

For questions or help implementing these features, contact the backend development team.