# Certificate Management API Documentation

This API provides endpoints to manage **Certificates** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `src_certificates` table contains the following fields:

| Field              | Type      | Description                    |
|--------------------|-----------|--------------------------------|
| id                 | bigint    | Primary key                    |
| department_id      | bigint    | Foreign key to departments     |
| category_id        | bigint    | Foreign key to src_sources     |
| subcategory_id     | bigint    | Foreign key to src_sources     |
| certificate_title  | string    | Certificate name               |
| certificate_number | string    | Certificate number             |
| issuing_authority  | string    | Authority that issued cert     |
| issue_date         | date      | Date certificate was issued    |
| valid_upto         | date      | Certificate expiry date        |
| alert_days         | integer   | Days before expiry to alert    |
| created_at         | timestamp | Record creation time           |
| updated_at         | timestamp | Record last update time        |

---

## Base URL

```
https://your-api-domain.com/api/src-management/certificates
```

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

---

## Endpoints

### 1. List All Certificates

- **Endpoint:** `GET /api/src-management/certificates`
- **Description:** Get a paginated list of certificates with search, filtering, and sorting capabilities.

**Query Parameters:**
- `search` (optional): Search by certificate title, number, or issuing authority
- `department_id` (optional): Filter by department
- `category_id` (optional): Filter by category
- `subcategory_id` (optional): Filter by subcategory
- `sort_by` (optional): Sort by id, certificate_title, issue_date, valid_upto
- `sort_order` (optional): asc or desc (default: desc)
- `page` (optional): Page number for pagination
- `per_page` (optional): Number of items per page (default: 10)

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/src-management/certificates?search=license&department_id=1" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

---

### 2. Create a Certificate

- **Endpoint:** `POST /api/src-management/certificates`
- **Description:** Create a new certificate with optional file attachments.

**Request Body Fields:**
- `department_id` (required): Department ID
- `category_id` (required): Category ID from src_sources
- `subcategory_id` (required): Subcategory ID from src_sources
- `certificate_title` (required): Certificate name
- `certificate_number` (required): Certificate number
- `issuing_authority` (required): Issuing authority name
- `issue_date` (required): Issue date (YYYY-MM-DD)
- `valid_upto` (required): Expiry date (YYYY-MM-DD)
- `alert_days` (required): Alert days before expiry (1-365)
- `attachment[]` (optional): File attachments (jpg,png,pdf,doc,docx, max 10MB each)

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/src-management/certificates" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -F "department_id=1" \
  -F "category_id=2" \
  -F "subcategory_id=3" \
  -F "certificate_title=Fire Safety Certificate" \
  -F "certificate_number=FSC-2024-001" \
  -F "issuing_authority=Fire Department" \
  -F "issue_date=2024-01-15" \
  -F "valid_upto=2025-01-15" \
  -F "alert_days=30" \
  -F "attachment[]=@/path/to/certificate.pdf"
```

---

### 3. Get a Single Certificate

- **Endpoint:** `GET /api/src-management/certificates/{id}`
- **Description:** Get details of a specific certificate by its ID.

---

### 4. Update a Certificate

- **Endpoint:** `PUT /api/src-management/certificates/{id}`
- **Description:** Update an existing certificate.

---

### 5. Renew a Certificate

- **Endpoint:** `POST /api/src-management/certificates/{id}/renew`
- **Description:** Create a renewed version of an existing certificate.

**Request Body Fields:**
- `issue_date` (required): New issue date
- `valid_upto` (required): New expiry date
- `attachment[]` (optional): New file attachments

---

### 6. Update Certificate Permissions

- **Endpoint:** `POST /api/src-management/certificates/permissions`
- **Description:** Update user permissions for certificates.

**Request Body:**
```json
{
  "1": [
    {
      "admin_id": 2,
      "view": true,
      "add": false
    }
  ]
}
```

---

### 7. Get Certificate Permissions

- **Endpoint:** `GET /api/src-management/certificates/{id}/permissions`
- **Description:** Get user permissions for a specific certificate.

---



## Business Rules

- **Expiry Date**: Must be after issue date
- **Alert Days**: Must be between 1-365 days
- **File Types**: Only jpg, png, pdf, doc, docx allowed
- **File Size**: Maximum 10MB per file
- **Permissions**: Admin and QMS users see all certificates, others see only permitted ones

---

## Error Codes

| HTTP Code | Description                           |
|-----------|---------------------------------------|
| 200       | Success                               |
| 201       | Created successfully                  |
| 400       | Bad Request / Validation Error        |
| 401       | Unauthorized                          |
| 404       | Certificate not found                 |
| 422       | Unprocessable Entity (Validation)     |
| 500       | Internal Server Error                 |