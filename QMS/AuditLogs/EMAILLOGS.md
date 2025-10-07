# Email Logs API Documentation

This API provides endpoints to manage and retrieve **Email Logs** for auditing and troubleshooting.
All endpoints are protected by authentication via **Laravel Sanctum**.

---

## Database Table Structure

The `email_logs` table contains the following fields:

| #  | Name            | Type                  | Attributes  | Null | Default | Extra          |
| -- | --------------- | --------------------- | ----------- | ---- | ------- | -------------- |
| 1  | id              | bigint                | Primary Key | No   | None    | AUTO_INCREMENT |
| 2  | recipient_email | varchar(255)          |             | No   | None    |                |
| 3  | cc_email        | varchar(255)          |             | Yes  | NULL    |                |
| 4  | bcc_email       | varchar(255)          |             | Yes  | NULL    |                |
| 5  | subject         | text                  |             | No   | None    |                |
| 6  | body            | longtext              |             | No   | None    |                |
| 7  | sender_email    | varchar(255)          |             | No   | None    |                |
| 8  | status          | enum('sent','failed') |             | No   | sent    |                |
| 9  | error_message   | text                  |             | Yes  | NULL    |                |
| 10 | sent_at         | timestamp             |             | Yes  | NULL    |                |
| 11 | created_at      | timestamp             |             | Yes  | NULL    |                |
| 12 | updated_at      | timestamp             |             | Yes  | NULL    |                |

**Sample Row:**

| id | recipient_email                               | subject                                         | status | sent_at             |
| -- | --------------------------------------------- | ----------------------------------------------- | ------ | ------------------- |
| 1  | [admin@tenant.test](mailto:admin@tenant.test) | Your Observation Has Been Accepted - OBS-92-QMS | sent   | 2025-09-29 11:06:42 |

---

## Base URL

```
{{site_url}}/api/email-logs
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

### 1. List All Email Logs

* **Endpoint:** `GET /api/email-logs`
* **Description:** Retrieve a paginated list of email logs with filters (status, date range) and search.

#### Query Parameters

| Parameter   | Type    | Example                       | Description                                |
| ----------- | ------- | ----------------------------- | ------------------------------------------ |
| `status`    | string  | `sent` / `failed` / `pending` | Filter by email status                     |
| `from_date` | date    | `2025-09-01`                  | Fetch logs sent **on or after** this date  |
| `to_date`   | date    | `2025-09-30`                  | Fetch logs sent **on or before** this date |
| `search`    | string  | `admin@tenant.test`           | Search across recipient, subject, sender   |

**Example Request:**

```bash
curl -X GET "{{site_url}}/api/email-logs" \
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
  "message": "Email logs fetched successfully",
  "data": {
    "current_page": 1,
    "data": [
      {
        "id": 1,
        "recipient_email": "admin@tenant.test",
        "subject": "Your Observation Has Been Accepted - OBS-92-QMS",
        "sender_email": "qms@cityhospital.com",
        "status": "sent",
        "sent_at": "2025-09-29T11:06:42.000000Z"
      }
    ],
    "total": 5
  }
}
```

---

### 2. Get a Single Email Log

* **Endpoint:** `GET /api/email-logs/{id}`
* **Description:** Retrieve detailed information of a single email log, including **full HTML body**.

**Example Response:**

```json
{
  "id": 2,
  "recipient_email": "admin@tenant.test",
  "cc_email": null,
  "bcc_email": null,
  "subject": "Observation Assigned to You - OBS-92-QMS",
  "body": "<!DOCTYPE html> ...",
  "sender_email": "qms@cityhospital.com",
  "status": "sent",
  "error_message": null,
  "sent_at": "2025-09-29T11:07:22.000000Z",
  "created_at": "2025-09-29T11:07:22.000000Z",
  "updated_at": "2025-09-29T11:07:22.000000Z"
}
```

**Not Found Example:**

```json
{
  "success": false,
  "status": 404,
  "message": "Email log not found",
  "data": null,
  "errors": null
}
```

---

## Summary Table

| Method | Endpoint             | Description              |
| ------ | -------------------- | ------------------------ |
| GET    | /api/email-logs      | List all email logs      |
| GET    | /api/email-logs/{id} | Get a specific email log |

---

## Notes

* **Authentication:** Required for all endpoints.
* **Read-Only:** Email logs cannot be created/updated manually (system-generated).
* **Filtering:** Supports query params like `?status=sent` or `?search=admin@tenant.test`.
* **Pagination:** Default page size = 10, configurable via `?per_page=20`.

