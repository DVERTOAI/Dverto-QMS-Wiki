# Observations API Documentation

This API provides endpoints to manage **Observations** in your application. All endpoints are protected by authentication via Laravel Sanctum.

## Database Table Structure

The `observations` table contains the following fields:

| #   | Name             | Type                                    | Attributes     | Null | Default | Extra           |
|-----|------------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id               | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | observation_no   | varchar(255)                            | Unique        | No   | None    |                 |
| 3   | observation_datetime | datetime                                   |               | No   | None    |                 |
| 4   | observation_id   | bigint(20) unsigned (Indexed, FK)       |               | No   | None    |                 |
| 5   | description      | text                                    |               | No   | None    |                 |
| 6   | allocation_time  | timestamp                               |               | Yes  | NULL    |                 |
| 7   | status           | enum'open','accepted','declined','assigned','rca_submitted','closed','reassigned','postponed')  |               | No   | open    |                 |
| 8   | assigned_at      | timestamp                               |               | Yes  | NULL    |                 |
| 9  | final_remark     | text                                    |               | Yes  | NULL    |                 |
| 10  | close_remark     | text                                    |               | Yes  | NULL    |                 |
| 11  | spot_id          | bigint(20) unsigned (Indexed, FK to department_areas) |               | No   | None    |                 |
| 12  | deleted_at       | timestamp                               |               | Yes  | NULL    |                 |
| 13  | created_at       | timestamp                               |               | Yes  | NULL    |                 |
| 14  | updated_at       | timestamp                               |               | Yes  | NULL    |                 |

**Sample Row:**

| id | observation_no | observation_datetime | observation_id | description | status | spot_id | created_at           | updated_at           |
|----|---------------|------------------|------------------|----------------|-------------|--------|---------|----------------------|----------------------|
| 14 | OBS100014     | 2022-12-02 15:45:00            | 1              | Observed    | open   | 1       | 2025-07-05 09:35:18  | 2025-07-05 10:24:11  |

---

## Attachments Table Structure

The `attachments` table stores file attachments related to observations:

| #   | Name              | Type                                    | Attributes     | Null | Default | Extra           |
|-----|-------------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id                | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | documentable_type | varchar(255)                            |               | No   | None    |                 |
| 3   | documentable_id   | bigint(20) unsigned (Indexed, FK)       |               | No   | None    |                 |
| 4   | name              | varchar(255)                            |               | No   | None    |                 |
| 5   | file_path         | varchar(255)                            |               | No   | None    |                 |
| 6   | mime_type         | varchar(255)                            |               | No   | None    |                 |
| 7   | size              | bigint(20) unsigned                     |               | No   | None    |                 |
| 8   | status            | enum('active', 'inactive')              |               | No   | active  |                 |
| 9   | created_at        | timestamp                               |               | Yes  | NULL    |                 |
| 10  | updated_at        | timestamp                               |               | Yes  | NULL    |                 |

**Sample Attachment Row:**

| id | documentable_type | documentable_id | name | file_path | mime_type | size | status | created_at | updated_at |
|----|------------------|-----------------|------|-----------|-----------|------|--------|------------|------------|
| 1 | App\Models\Tenant\Observation\Observation | 14 | 1751708119_Prince module issues.docx.pdf | uploads/observations/OBS100014/1751708119_Prince module issues.docx.pdf | application/pdf | 569919 | active | 2025-07-05 09:35:19 | 2025-07-05 09:35:19 |

---

## Base URL

```
https://your-api-domain.com/api/observation
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

### 1. List All Observations

- **Endpoint:** `GET /api/observation`
- **Description:** Get a paginated list of observations.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/observation" \
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
  "message": "Observations fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 15,
        "observation_no": "OBS100015",
       "observation_datetime": "2022-12-02T15:45:00.000000Z",
        "observation_id": 1,
        "description": "Observed rare bird species near the lake",
        "allocation_time": null,
        "status": "open",
        "assigned_at": null,
        "final_remark": null,
        "close_remark": null,
        "spot_id": 1,
        "deleted_at": null,
        "created_at": "2025-07-05T12:45:06.000000Z",
        "updated_at": "2025-07-05T12:45:06.000000Z"
      },
      {
        "id": 14,
        "observation_no": "OBS100014",
        "observation_datetime": "2022-12-02T15:45:00.000000Z",
        "observation_id": 1,
        "description": "Observed",
        "allocation_time": null,
        "status": "open",
        "assigned_at": "2025-07-01T13:50:00.000000Z",
        "final_remark": null,
        "close_remark": null,
        "spot_id": 1,
        "deleted_at": null,
        "created_at": "2025-07-05T09:35:18.000000Z",
        "updated_at": "2025-07-05T10:24:11.000000Z"
      }
    ],
    "first_page_url": "http://127.0.0.1:8000/api/observation?page=1",
    "from": 1,
    "last_page": 1,
    "last_page_url": "http://127.0.0.1:8000/api/observation?page=1",
    "links": [
      {
        "url": null,
        "label": "&laquo; Previous",
        "active": false
      },
      {
        "url": "http://127.0.0.1:8000/api/observation?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Next &raquo;",
        "active": false
      }
    ],
    "next_page_url": null,
    "path": "http://127.0.0.1:8000/api/observation",
    "per_page": 10,
    "prev_page_url": null,
    "to": 2,
    "total": 2
  }
}
```

---

### 2. Create an Observation

- **Endpoint:** `POST /api/observation`
- **Description:** Create a new observation with optional file attachments.

**Example Request Body:**
```json
{
  "observation_datetime": "2022-12-02 15:45:00",
  "spot_id": 1,
  "description": "Observed rare bird species near the lake"
}
```

**Example Request (with file upload):**
```bash
curl -X POST "https://your-api-domain.com/api/observation" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -F "observation_date=2025-07-01" \
  -F "observation_time=14:05:11" \
  -F "spot_id=1" \
  -F "description=Observed rare bird species near the lake" \
  -F "attachment[]=@/path/to/file.pdf"
```

**Example Response:**
```json
{
  "status": true,
  "status_code": 201,
  "message": "Observation created successfully!",
  "data": {
    "id": 15,
    "observation_no": "OBS100015",
    "observation_datetime": "2022-12-02T15:45:00.000000Z",
    "observation_id": 1,
    "description": "Observed rare bird species near the lake",
    "allocation_time": null,
    "status": "open",
    "assigned_at": null,
    "final_remark": null,
    "close_remark": null,
    "spot_id": 1,
    "deleted_at": null,
    "created_at": "2025-07-05T12:45:06.000000Z",
    "updated_at": "2025-07-05T12:45:06.000000Z"
  }
}
```

**Validation Rules:**
- `observation_date`: Required, valid date
- `observation_time`: Required, valid time format
- `spot_id`: Required, must exist in department_areas table
- `description`: Required, string
- `attachment[]`: Optional, file types: jpg, png, pdf, doc, docx, max size: 2MB

**Validation Error Example:**
```json
{
  "errors": {
    "observation_date": ["The observation date field is required."],
    "observation_time": ["The observation time field is required."],
    "spot_id": ["The spot id field is required."],
    "description": ["The description field is required."]
  }
}
```

---

### 3. Get a Single Observation

- **Endpoint:** `GET /api/observation/{id}`
- **Description:** Get details of a specific observation by its ID, including user and department area information.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/observation/14" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com"
```

**Example Response (Success):**
```json
{
  "success": true,
  "status": 200,
  "message": "Success",
  "data": {
    "id": 14,
    "observation_no": "OBS100014",
    "observation_datetime": "2022-12-02T15:45:00.000000Z",
    "observation_id": 1,
    "description": "Observed",
    "allocation_time": null,
    "status": "open",
    "assigned_at": "2025-07-01T13:50:00.000000Z",
    "final_remark": null,
    "close_remark": null,
    "spot_id": 1,
    "deleted_at": null,
    "created_at": "2025-07-05T09:35:18.000000Z",
    "updated_at": "2025-07-05T10:24:11.000000Z",
    "observer": {
      "id": 1,
      "employee_id": null,
      "name": "Landlord Super Admin",
      "email": "harsh@gmail.com",
      "username": "",
      "mobile": null,
      "status": "active",
      "designation": null,
      "department_id": null,
      "department_head": 0,
      "properties": null,
      "email_verified_at": null,
      "deleted_at": null,
      "created_at": "2025-06-25T16:29:17.000000Z",
      "updated_at": "2025-06-25T16:29:17.000000Z"
    },
    "department_area": {
      "id": 1,
      "department_id": 2,
      "name": "Floor",
      "status": "active",
      "created_at": "2025-07-04T14:54:07.000000Z",
      "updated_at": "2025-07-04T14:54:07.000000Z",
      "department": {
        "id": 2,
        "name": "Production",
        "status": "active",
        "created_at": "2025-06-28T01:29:01.000000Z",
        "updated_at": "2025-06-28T01:29:01.000000Z"
      }
    }
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Observation not found",
  "data": null,
  "errors": null
}
```

---

### 4. Update an Observation

- **Endpoint:** `PUT /api/observation/{id}`
- **Description:** Update an existing observation.

**Example Request Body:**
```json
{
  "observation_datetime":"2022-12-02 15:45:00",
  "spot_id": 1,
  "description": "Updated observation description"
}
```

**Example Request:**
```bash
curl -X PUT "https://your-api-domain.com/api/observation/14" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"observation_date":"2025-07-01","observation_time":"14:05:15","spot_id":1,"description":"Updated observation description"}'
```

**Example Response (Success):**
```json
{
  "success": true,
  "status": 200,
  "message": "Observation updated successfully!",
  "data": {
    "id": 14,
    "observation_no": "OBS100014",
   "observation_datetime": "2022-12-02T15:45:00.000000Z",
    "observation_id": 1,
    "description": "Updated observation description",
    "allocation_time": null,
    "status": "open",
    "assigned_at": "2025-07-01T13:50:00.000000Z",
    "final_remark": null,
    "close_remark": null,
    "spot_id": 1,
    "deleted_at": null,
    "created_at": "2025-07-05T09:35:18.000000Z",
    "updated_at": "2025-07-05T10:24:11.000000Z"
  }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Observation not found",
  "data": null,
  "errors": null
}
```

## File Attachments

### Supported File Types and Limits

- **Allowed file types:** JPG, PNG, PDF, DOC, DOCX
- **Maximum file size:** 2MB per file
- **Multiple files:** Supported via `attachment[]` array

### File Storage Structure

Files are stored in the following directory structure:
```
uploads/observations/{observation_no}/filename
```

Example: `uploads/observations/OBS100014/1751708119_Prince module issues.docx.pdf`

### Attachment Data Structure

Each attachment record contains:
- `documentable_type`: "App\\Models\\Tenant\\Observation\\Observation"
- `documentable_id`: The observation ID
- `name`: Original filename
- `file_path`: Storage path
- `mime_type`: File MIME type
- `size`: File size in bytes
- `status`: "active" or "inactive"

---

## Summary Table

| Method | Endpoint                 | Description                    |
|--------|--------------------------|--------------------------------|
| GET    | /api/observation         | List all observations          |
| POST   | /api/observation         | Create an observation          |
| GET    | /api/observation/{id}    | Get a specific observation     |
| PUT    | /api/observation/{id}    | Update an observation          |

---

## Status Values

The observation status can be one of the following:
- `open`: Initial status for new observations
- `In Progress`: Observation is being worked on
- `Closed`: Observation has been completed

---

## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **Auto-generated Fields:** `observation_no` is automatically generated with format "OBS" + 6-digit number.
- **Observer Assignment:** The `observation_id` is automatically set to the authenticated user.
- **Department Area Link:** The `spot_id` references the department_areas table.
- **File Uploads:** Use multipart/form-data encoding when uploading files.
- **Soft Deletes:** Observations are soft-deleted (marked as deleted but not removed from database).
- **Timestamps:** All timestamps are in ISO 8601 format with timezone information.

---

## Contact

For any questions or support, contact the backend development team.