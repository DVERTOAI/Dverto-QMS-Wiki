
# Documents API Documentation

This API manages **Documents, Permissions, Acknowledgments, Status, and Public URLs**.  
All endpoints require **Laravel Sanctum authentication** unless noted otherwise.

---

## Base URL

```

[https://your-api-domain.com/api](https://your-api-domain.com/api)

````

## Headers

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
````

---

## Database Table Structure

### document\_uploads

| #  | Name                 | Type         | Null | Default | Extra           |
| -- | -------------------- | ------------ | ---- | ------- | --------------- |
| 1  | id                   | bigint(20)   | No   | None    | AUTO\_INCREMENT |
| 2  | document\_name       | varchar(255) | No   | None    |                 |
| 3  | document\_no         | varchar(255) | No   | None    | UNIQUE          |
| 4  | document\_category   | varchar(255) | No   | None    |                 |
| 5  | document\_file\_path | varchar(255) | No   | None    |                 |
| 6  | search\_keywords     | json         | Yes  | NULL    |                 |
| 7  | is\_active           | tinyint(1)   | No   | 1       |                 |
| 8  | public\_token        | varchar(255) | Yes  | NULL    | UNIQUE          |
| 9  | created\_at          | timestamp    | Yes  | NULL    |                 |
| 10 | updated\_at          | timestamp    | Yes  | NULL    |                 |

---

## 1. List Documents

**GET** `/documents`

**Response Example:**

```json
{
  "success": true,
  "status": 200,
  "message": "Documents fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "document_category": "HR Policy Updated",
        "document_no": "DOC-2025-01",
        "document_name": "Updated Employee Handbook",
        "document_file_path": "documents/2025/09/DOC-2025-01/Updated_Employee_Handbook.pdf",
        "search_keywords": ["employee","handbook","policy"],
        "is_active": true,
        "public_token": null,
        "view_url": "http://localhost/storage/documents/2025/09/DOC-2025-01/Updated_Employee_Handbook.pdf",
        "download_url": "http://127.0.0.1:8000/api/documents/download/1",
        "permissions": []
      }
    ],
    "total": 4
  }
}
```

---

## 2. Upload Document

**POST** `/documents`

**Request (Multipart Form Data):**

| Field               | Type   | Required | Description            |
| ------------------- | ------ | -------- | ---------------------- |
| document\_name      | string | Yes      | Name of the document   |
| document\_no        | string | Yes      | Unique document number |
| document\_category  | string | Yes      | Category               |
| search\_keywords\[] | array  | No       | Keywords for search    |
| select\_document    | file   | Yes      | File (100 KB – 10 MB)  |

**Validation Error Example:**

```json
{
  "message": "This document number is already taken.",
  "errors": {
    "document_no": ["This document number is already taken."]
  }
}
```

**Success Response:**

```json
{
  "success": true,
  "status": 201,
  "message": "Document uploaded successfully",
  "data": {
    "id": 4,
    "document_name": "Updated Employee Handbook",
    "document_no": "DOC-133025-02",
    "document_category": "HR Policy Updated",
    "document_file_path": "tenant/psri/documents/2025/09/DOC-133025-02/Updated_Employee_Handbook.pdf",
    "search_keywords": ["employee","handbook","policy"],
    "is_active": true,
    "view_url": "http://localhost/storage/tenant/psri/documents/2025/09/DOC-133025-02/Updated_Employee_Handbook.pdf",
    "download_url": "http://127.0.0.1:8000/api/documents/download/4"
  }
}
```

---

## 3. Download Document

**GET** `/documents/download/{id}`

**Response:** File stream (download).

---

## 4. Acknowledge Document

**POST** `/documents/{id}/acknowledge`

**Payload:**

```json
{ "acknowledged": true }
```

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Acknowledgment recorded successfully",
  "data": {
    "id": 5,
    "document_id": 1,
    "admin_id": 1,
    "read": false,
    "download": false,
    "print": false,
    "acknowledged": true,
    "acknowledged_at": "2025-09-16T06:41:38.000000Z",
    "created_at": "2025-09-16T03:48:39.000000Z",
    "updated_at": "2025-09-16T06:41:38.000000Z"
  }
}
```

---

## 5. Get Document Acknowledgments

**GET** `/documents/{id}/acknowledgments`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Acknowledgments fetched successfully",
  "data": [
    {
      "id": 5,
      "document_id": 1,
      "admin_id": 1,
      "acknowledged": true,
      "acknowledged_at": "2025-09-16T06:41:38.000000Z",
      "admin": {
        "id": 1,
        "name": "Tenant Admin",
        "email": "admin@tenant.test"
      }
    }
  ]
}
```

---

## 6. Toggle Document Status

**POST** `/documents/{id}/toggle-status`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Document status updated successfully",
  "data": {
    "id": 1,
    "document_no": "DOC-2025-01",
    "is_active": true
  }
}
```

---

## 7. Assign Permissions

**POST** `/document-permissions`

**Payload:**

```json
{
  "document_id": 1,
  "permissions": {
    "2": { "read": true, "download": true },
    "3": { "read": true },
    "4": { "print": true }
  }
}
```

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Permissions assigned successfully.",
  "data": null
}
```

---

## 8. Get Document Permissions

**GET** `/document-permissions?document_id={id}`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Document permissions fetched successfully",
  "data": [
    {
      "id": 2,
      "document_id": 1,
      "admin_id": 2,
      "read": true,
      "download": true,
      "print": true
    },
    {
      "id": 3,
      "document_id": 1,
      "admin_id": 3,
      "read": true,
      "download": false,
      "print": false
    }
  ]
}
```

---

## 9. Generate Public URL

**POST** `/public-urls/{id}/generate`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Public URL generated successfully",
  "data": {
    "public_url": "http://127.0.0.1:8000/api/public-urls/DOC-2025-01/acknowledgments"
  }
}
```

---

## 10. List Public URLs

**GET** `/public-urls`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Documents fetched successfully",
  "data": [
    {
      "id": 1,
      "document_name": "Updated Employee Handbook",
      "document_no": "DOC-2025-01",
      "public_token": "DOC-2025-01",
      "is_active": true,
      "download_url": "http://127.0.0.1:8000/api/documents/download/1"
    }
  ]
}
```

---

## 11. Remove Public URL

**DELETE** `/public-urls/{id}`

**Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Public URL removed successfully",
  "data": null
}
```

---

## Summary Table

| Method | Endpoint                                | Description                   |
| ------ | --------------------------------------- | ----------------------------- |
| GET    | /documents                              | List all documents            |
| POST   | /documents                              | Upload a new document         |
| GET    | /documents/download/{id}                | Download a document           |
| POST   | /documents/{id}/acknowledge             | Acknowledge a document        |
| GET    | /documents/{id}/acknowledgments         | Get acknowledgments           |
| POST   | /documents/{id}/toggle-status           | Toggle document active status |
| POST   | /document-permissions                   | Assign permissions            |
| GET    | /document-permissions?document\_id={id} | Get document permissions      |
| POST   | /public-urls/{id}/generate              | Generate public URL           |
| GET    | /public-urls                            | List all public URLs          |
| DELETE | /public-urls/{id}                       | Remove public URL             |



