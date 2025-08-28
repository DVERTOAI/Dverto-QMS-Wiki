# Settings API Documentation

This API provides endpoints to manage **Settings** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `settings` table contains the following fields:

| #   | Name           | Type                                           | Attributes     | Null | Default | Extra           |
|-----|----------------|------------------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                            | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | group          | varchar(100)                                   |               | No   | None    |                 |
| 3   | key            | varchar(100)                                   |               | No   | None    |                 |
| 4   | value          | text                                           |               | No   | None    |                 |
| 5   | value_type     | enum('string', 'number', 'boolean', 'json', 'document_id') |  | No   | string  |                 |
| 6   | updated_by     | bigint(20) unsigned                            | Foreign Key   | Yes  | NULL    |                 |
| 7   | created_at     | timestamp                                      |               | Yes  | NULL    |                 |
| 8   | updated_at     | timestamp                                      |               | Yes  | NULL    |                 |

**Sample Rows:**

| id | group  | key           | value                | value_type  | updated_by | created_at           | updated_at           |
|----|--------|---------------|----------------------|-------------|------------|----------------------|----------------------|
| 1  | app    | company_name  | PSRI Technologies    | string      | 2          | 2025-06-29 01:36:15  | 2025-06-29 01:36:15  |
| 2  | app    | site_logo     | 123                  | document_id | 2          | 2025-08-28 10:15:30  | 2025-08-28 10:15:30  |

---

## Base URL

```
https://your-api-domain.com/api/settings
```
*Replace `your-api-domain.com` with your actual API domain.*

---

## Authentication

All endpoints require a valid Bearer token obtained from the login endpoint.

**Required Headers:**
```http
Content-Type: application/json (for JSON requests)
Content-Type: multipart/form-data (for file uploads)
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```
- Replace `<your_token>` with the token received from your login response.

---

## Endpoints

### 1. List All Settings

- **Endpoint:** `GET /api/settings`
- **Description:** Get all settings grouped by category.

**Query Parameters:**
- `group` (optional): Filter settings by group name

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/settings?group=app" \
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
    "message": "Settings retrieved successfully",
    "data": {
        "contact_information": [
            {
                "id": 1,
                "group": "contact_information",
                "key": "street_address",
                "value": "123 Health Street",
                "value_type": "string",
                "updated_by": 2,
                "created_at": "2025-08-26T04:39:36.000000Z",
                "updated_at": "2025-08-26T04:39:36.000000Z"
            }
        ],
        "app": [
            {
                "id": 2,
                "group": "app",
                "key": "site_logo",
                "value": {
                    "document_id": 123,
                    "document": {
                        "id": 123,
                        "name": "logo.png",
                        "file_path": "setting/2024/12/app_site_logo/logo.png",
                        "mime_type": "image/png",
                        "size": 45678
                    },
                    "url": "https://your-domain.com/storage/setting/2024/12/app_site_logo/logo.png"
                },
                "value_type": "document_id",
                "updated_by": 2,
                "created_at": "2025-08-28T10:15:30.000000Z",
                "updated_at": "2025-08-28T10:15:30.000000Z"
            }
        ]
    }
}
```

---

### 2. Create/Update Settings (Text Only)

- **Endpoint:** `POST /api/settings`
- **Description:** Create or update settings with text values.
- **Content-Type:** `application/json`

**Request Body Schema:**
```json
{
    "group": "string (required, max:100)",
    "settings": [
        {
            "key": "string (required, max:100)",
            "value": "string|number|boolean|array (required)"
        }
    ]
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/settings" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{
    "group": "app",
    "settings": [
        {
            "key": "company_name",
            "value": "PSRI Technologies"
        },
        {
            "key": "maintenance_mode",
            "value": true
        },
        {
            "key": "max_users",
            "value": 500
        },
        {
            "key": "features",
            "value": {"chat": true, "reports": false}
        }
    ]
}'
```

**Example Response:**
```json
{
    "success": true,
    "status": 201,
    "message": "Settings created/updated successfully",
    "data": [
        {
            "id": 1,
            "group": "app",
            "key": "company_name",
            "value": "PSRI Technologies",
            "value_type": "string",
            "updated_by": 2,
            "created_at": "2025-08-28T10:30:15.000000Z",
            "updated_at": "2025-08-28T10:30:15.000000Z"
        }
    ]
}
```

---

### 3. Create/Update Settings with File Upload

- **Endpoint:** `POST /api/settings`
- **Description:** Create or update settings that include file uploads (images, documents).
- **Content-Type:** `multipart/form-data`

**Form Data Fields:**
- `group`: string (required, max:100)
- `settings[0][key]`: string (required, max:100)
- `settings[0][value]`: string (optional, description or text value)
- `settings[0][attachments][]`: file (optional, image/document files)

**File Upload Restrictions:**
- **Allowed file types:** jpeg, jpg, png, gif, webp, pdf, doc, docx
- **Maximum file size:** 10MB per file
- **Multiple files:** Supported per setting


**Example Response (File Upload):**
```json
{
    "success": true,
    "status": 201,
    "message": "Settings created/updated successfully",
    "data": [
        {
            "id": 3,
            "group": "general",
            "key": "site_logo",
            "value": {
                "document_id": 456,
                "document": {
                    "id": 456,
                    "name": "1735392000_logo.png",
                    "file_path": "setting/2025/08/general_site_logo/1735392000_logo.png",
                    "mime_type": "image/png",
                    "size": 125440
                },
                "url": "https://your-domain.com/storage/setting/2025/08/general_site_logo/1735392000_logo.png"
            },
            "value_type": "document_id",
            "updated_by": 2,
            "created_at": "2025-08-28T12:00:00.000000Z",
            "updated_at": "2025-08-28T12:00:00.000000Z"
        }
    ]
}
```

---

### 4. Mixed Settings (Text + Files)

You can combine text settings and file uploads in a single request:

**Example Request (Postman/Form-data):**
```
POST /api/settings
Content-Type: multipart/form-data

group: general
settings[0][key]: site_name
settings[0][value]: My Website
settings[1][key]: maintenance_mode  
settings[1][value]: true
settings[2][key]: max_users
settings[2][value]: 500
settings[3][key]: features
settings[3][value]: {"chat":true,"reports":false}
settings[4][key]: site_logo
settings[4][attachments]: [FILE: qms_logo.jpg]
```

---

## File Storage Structure

When files are uploaded, they are stored using the following directory structure:
```
storage/app/public/setting/{year}/{month}/{group}_{key}/{timestamp_filename}
```

**Example:**
- Group: `general`
- Key: `site_logo` 
- File: `logo.png`
- Storage Path: `storage/app/public/setting/2025/08/general_site_logo/1735392000_logo.png`
- Public URL: `https://your-domain.com/storage/setting/2025/08/general_site_logo/1735392000_logo.png`

---

## Value Types

| Type        | Description                              | Example Value                    | Storage Method    |
|-------------|------------------------------------------|----------------------------------|-------------------|
| string      | Text values                              | "PSRI Technologies"              | Direct in DB      |
| number      | Numeric values                           | 500, 10.5                        | Direct in DB      |
| boolean     | Boolean values                           | true, false                      | Direct in DB      |
| json        | JSON formatted data                      | {"chat": true, "reports": false} | Direct in DB      |
| document_id | File uploads (images, PDFs, docs)        | Document ID reference            | File + DB record  |

---

## Error Responses

### Validation Errors (422)
```json
{
    "success": false,
    "status": 422,
    "message": "The given data was invalid.",
    "errors": {
        "group": ["The group field is required."],
        "settings.0.key": ["The settings.0.key field is required."],
        "settings.0.attachments.0": ["The file must be an image (jpeg, jpg, png, gif, webp) or document (pdf, doc, docx)."],
        "settings.0.attachments.0": ["File size must not exceed 10MB."]
    }
}
```

### File Upload Errors
```json
{
    "success": false,
    "status": 422,
    "message": "File validation failed.",
    "errors": {
        "settings.0.attachments.0": [
            "The file must be an image (jpeg, jpg, png, gif, webp) or document (pdf, doc, docx).",
            "File size must not exceed 10MB."
        ]
    }
}
```

---

## Summary Table

| Method | Endpoint                    | Description                      | Content-Type           |
|--------|-----------------------------|----------------------------------|------------------------|
| GET    | /api/settings               | List all settings                | application/json       |
| GET    | /api/settings?group={name}  | Get settings by group            | application/json       |
| POST   | /api/settings               | Create/update text settings      | application/json       |
| POST   | /api/settings               | Create/update with file uploads  | multipart/form-data    |

---

## File Upload Features

### Supported File Types
- **Images:** jpeg, jpg, png, gif, webp
- **Documents:** pdf, doc, docx
- **Maximum Size:** 10MB per file
- **Multiple Files:** Supported per setting key

### File Replacement
- **Automatic Cleanup:** Old files are automatically deleted when new files are uploaded
- **Database Cleanup:** Old document records are removed
- **Storage Optimization:** No orphaned files remain in storage

### File Access
- **Public URL:** Files are accessible via public storage URLs
- **Document Metadata:** File name, size, MIME type included in response
- **Security:** Files stored with timestamp prefixes to prevent conflicts

---



## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **File Updates:** When updating file settings, old files are automatically deleted from storage.
- **Document Integration:** Files are stored using the existing Document model with polymorphic relationships.
- **Storage Path:** Files follow the pattern: `setting/{year}/{month}/{group}_{key}/{timestamp_filename}`
- **Error Handling:** All error responses are in JSON format with detailed validation messages.

---

## Contact

For any questions or support, contact the backend development team.
