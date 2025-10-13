# Profile API Documentation

This API provides endpoints to manage **User Profiles** in your application. All endpoints are protected by authentication via Laravel Sanctum.

---

## Database Table Structure

The `users` table contains the following profile-related fields:

| # | Name         | Type         | Attributes  | Null | Default | Extra           |
| - | ------------ | ------------ | ----------- | ---- | ------- | --------------- |
| 1 | id           | bigint(20)   | Primary Key | No   | None    | AUTO_INCREMENT  |
| 2 | employee_id  | varchar(50)  |             | Yes  | NULL    |                 |
| 3 | name         | varchar(255) |             | No   | None    |                 |
| 4 | email        | varchar(255) | Unique      | No   | None    |                 |
| 5 | username     | varchar(255) | Unique      | No   | None    |                 |
| 6 | mobile       | varchar(15)  | Unique      | Yes  | NULL    |                 |
| 7 | designation  | varchar(255) |             | Yes  | NULL    |                 |
| 8 | gender       | enum         |             | Yes  | NULL    |                 |
| 9 | location     | varchar(255) |             | Yes  | NULL    |                 |
| 10| date_joined  | date         |             | Yes  | NULL    |                 |
| 11| password     | varchar(255) |             | No   | None    |                 |

**Sample Row:**

| id | employee_id | name     | email           | mobile     | designation | gender | location |
| -- | ----------- | -------- | --------------- | ---------- | ----------- | ------ | -------- |
| 1  | EMP001      | John Doe | john@psri.com   | 1234567890 | Engineer    | male   | Mumbai   |

---

## Base URL

```
https://your-api-domain.com/api/profile
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

### 1. Get Current User Profile

* **Endpoint:** `GET /api/profile`
* **Description:** Retrieve current authenticated user's profile information.

**Example Request:**

```bash
curl -X GET "https://your-api-domain.com/api/profile" \
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
  "message": "Profile fetched successfully",
  "data": {
    "id": 1,
    "employee_id": "EMP001",
    "name": "John Doe",
    "email": "john@psri.com",
    "username": "johndoe",
    "mobile": "1234567890",
    "designation": "Software Engineer",
    "gender": "male",
    "location": "Mumbai",
    "date_joined": "2024-01-15",
    "created_at": "2024-01-15T10:30:00.000000Z",
    "updated_at": "2024-01-15T10:30:00.000000Z",
    "department": {
      "id": 1,
      "name": "IT Department"
    },
    "roles": [
      {
        "id": 1,
        "name": "employee"
      }
    ]
  }
}
```

---

### 2. Update Current User Profile

* **Endpoint:** `PUT /api/profile`
* **Description:** Update current authenticated user's profile information(sent those feilds which you want to update).

**Example Request Body:**

```json
{
  "name": "John Smith",
  "email": "john.smith@psri.com",
  "mobile": "9876543210",
  "designation": "Senior Software Engineer",
  "gender": "male",
  "location": "Delhi",
  "password": "newpassword123"
}
```

**Example Response:**

```json
{
  "success": true,
  "status": 200,
  "message": "Profile updated successfully",
  "data": {
    "id": 1,
    "employee_id": "EMP001",
    "name": "John Smith",
    "email": "john.smith@psri.com",
    "mobile": "9876543210",
    "designation": "Senior Software Engineer",
    "gender": "male",
    "location": "Delhi",
    "updated_at": "2024-01-16T10:30:00.000000Z"
  }
}
```

**Validation Error Example (Duplicate Email):**

```json
{
  "message": "The email has already been taken.",
  "errors": {
    "email": [
      "The email has already been taken."
    ]
  }
}
```

---

## Validation Rules

| Field       | Rules                                           |
| ----------- | ----------------------------------------------- |
| name        | Required, string, max 255 characters           |
| email       | Required, valid email, unique (excluding self) |
| mobile      | Optional, string, max 15 chars, unique         |
| designation | Optional, string, max 255 characters           |
| gender      | Optional, enum (male, female, other)           |
| location    | Optional, string, max 255 characters           |
| password    | Optional, string, minimum 8 characters         |

---

## Summary Table

| Method | Endpoint     | Description                |
| ------ | ------------ | -------------------------- |
| GET    | /api/profile | Get current user profile   |
| PUT    | /api/profile | Update current user profile|

---

## Notes

* **Authentication:** All routes require Sanctum authentication.
* **Header Requirement:** `domain: psri.com` is mandatory.
* **Self-Update Only:** Users can only update their own profile.
* **Limited Fields:** Users cannot modify employee_id, username, roles, or department.
* **Password Hashing:** Passwords are automatically hashed when updated.
* **Unique Validation:** Email and mobile must be unique across all users.

---