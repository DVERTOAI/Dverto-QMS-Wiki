# Settings API Documentation

This API provides endpoints to manage **Settings** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `settings` table contains the following fields:

| #   | Name           | Type                                    | Attributes     | Null | Default | Extra           |
|-----|----------------|-----------------------------------------|---------------|------|---------|-----------------|
| 1   | id             | bigint(20) unsigned                     | Primary Key   | No   | None    | AUTO_INCREMENT  |
| 2   | group          | varchar(100)                            |               | No   | None    |                 |
| 3   | key            | varchar(100)                            |               | No   | None    |                 |
| 4   | value          | text                                    |               | No   | None    |                 |
| 5   | value_type     | enum('string', 'number', 'boolean', 'json') |           | No   | string  |                 |
| 6   | created_at     | timestamp                               |               | Yes  | NULL    |                 |
| 7   | updated_at     | timestamp                               |               | Yes  | NULL    |                 |

**Sample Row:**

| id | group  | key           | value                | value_type | created_at           | updated_at           |
|----|--------|---------------|----------------------|------------|----------------------|----------------------|
| 1  | app    | company_name  | PSRI Technologies    | string     | 2025-06-29 01:36:15  | 2025-06-29 01:36:15  |

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
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```
- Replace `<your_token>` with the token received from your login response.


## Endpoints

### 1. List All Settings

- **Endpoint:** `GET /api/settings`
- **Description:** Get a paginated list of all settings.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/settings" \
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
            },
            {
                "id": 2,
                "group": "contact_information",
                "key": "phone no",
                "value": "8299471328",
                "value_type": "string",
                "updated_by": 2,
                "created_at": "2025-08-26T04:40:43.000000Z",
                "updated_at": "2025-08-26T04:40:43.000000Z"
            }
        ],
        "QMS": [
            {
                "id": 3,
                "group": "QMS",
                "key": "phone no",
                "value": "82994891328",
                "value_type": "string",
                "updated_by": 2,
                "created_at": "2025-08-26T04:41:46.000000Z",
                "updated_at": "2025-08-26T05:36:59.000000Z"
            }
        ]
    }
}
```

---

### 2. Create a Setting

- **Endpoint:** `POST /api/settings`
- **Description:** Create a new setting.

**Example Request Body:**
```json
{
  "group": "app",
  "key": "max_file_size",
  "value": "10485760",
  "value_type": "number"
}
```

**Example Request:**
```bash
curl -X POST "https://your-api-domain.com/api/settings" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "Authorization: Bearer <your_token>" \
  -H "domain: psri.com" \
  -d '{"group":"app","key":"max_file_size","value":"10485760","value_type":"number"}'
```

**Example Response:**
```json
{
  "success": true,
  "status": 201,
  "message": "Setting created successfully",
  "data": {
    "id": 2,
    "group": "app",
    "key": "max_file_size",
    "value": "10485760",
    "value_type": "number",
    "created_at": "2025-06-29T01:36:15.000000Z",
    "updated_at": "2025-06-29T01:36:15.000000Z"
  }
}
```

**Validation Error Example (Missing Fields):**
```json
{
  "success": false,
  "status": 422,
  "message": "The given data was invalid.",
  "data": null,
  "errors": {
    "group": ["The group field is required."],
    "key": ["The key field is required."],
    "value": ["The value field is required."],
    "value_type": ["The value type field is required."]
  }
}
```

**Validation Error Example (Invalid Value Type):**
```json
{
  "success": false,
  "status": 422,
  "message": "The given data was invalid.",
  "data": null,
  "errors": {
    "value_type": ["The selected value type is invalid."]
  }
}
```

---

### 3. Get a Single Setting

- **Endpoint:** `GET /api/settings?group=qms`
- **Description:** Get details of a specific setting by its ID.

**Example Request:**
```bash
curl -X GET "https://your-api-domain.com/api/settings/1" \
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
    "message": "Settings retrieved successfully",
    "data": {
        "phone no": {
            "id": 3,
            "group": "QMS",
            "key": "phone no",
            "value": "82994891328",
            "value_type": "string",
            "updated_by": 2,
            "created_at": "2025-08-26T04:41:46.000000Z",
            "updated_at": "2025-08-26T05:36:59.000000Z"
        }
    }
}
```

**Example Response (Not Found):**
```json
{
  "success": false,
  "status": 404,
  "message": "Setting not found",
  "data": null,
  "errors": null
}
```


## Summary Table

| Method | Endpoint              | Description           | 
|--------|-----------------------|-----------------------|
| GET    | /api/settings         | List all settings     | 
| POST   | /api/settings         | Create a setting      |
| GET    | /api/settings/group={name}    | Get a specific setting| 


---

## Value Types

The `value_type` field accepts the following values:

| Type    | Description                           | Example Value        |
|---------|---------------------------------------|---------------------|
| string  | Text values                          | "PSRI Technologies" |
| number  | Numeric values                       | "10485760"          |
| boolean | Boolean values (true/false)          | "true"              |
| json    | JSON formatted data                  | '{"key": "value"}'  |

---

## Notes

- **Authentication:** All routes require a valid Sanctum token in the `Authorization` header.
- **Domain:** The `domain: psri.com` header is required for all requests.
- **Content-Type:** Always set to `application/json`.
- **Error Handling:** All error responses are in JSON format.
- **Value Storage:** All values are stored as text, but `value_type` indicates how they should be interpreted.

---

## Contact

For any questions or support, contact the backend development team.