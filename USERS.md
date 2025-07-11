
# User API Documentation

This API provides endpoints to manage users in the system. All routes are protected using Laravel Sanctum authentication.

## Base URL

- **Base URL**: `https://your-api-domain.com/api/users`  
  (Replace `your-api-domain.com` with your actual backend domain.)

## Authentication

- All requests must include the following headers:
  - `Content-Type: application/json`
  - `Accept: application/json`
  - `Authorization: Bearer <your_token>`
  - `domain: psri.com`

## Database Fields

| Field              | Type                | Collation                | Null | Default | Extra          |
|--------------------|---------------------|--------------------------|------|---------|-----------------|
| id                 | bigint(20)          | UNSIGNED                 | No   | None    | AUTO_INCREMENT   |
| employee_id        | varchar(255)        | utf8mb4_unicode_ci       | Yes  | NULL    |                 |
| name               | varchar(255)        | utf8mb4_unicode_ci       | No   | None    |                 |
| email              | varchar(255)        | utf8mb4_unicode_ci       | No   | None    | INDEX           |
| username           | varchar(255)        | utf8mb4_unicode_ci       | No   | None    | INDEX           |
| mobile             | varchar(15)         | utf8mb4_unicode_ci       | Yes  | NULL    | INDEX           |
| gender             | enum('male', 'female', 'other') | utf8mb4_unicode_ci | Yes  | NULL    | INDEX           |
| location           | varchar(255)        | utf8mb4_unicode_ci       | Yes  | NULL    | INDEX           |
| date_joined        | date                |                          | Yes  | NULL    | INDEX           |
| status             | enum('active', 'inactive') | utf8mb4_unicode_ci | No   | active  |                 |
| designation        | varchar(255)        | utf8mb4_unicode_ci       | Yes  | NULL    |                 |
| department_id      | bigint(20)          | UNSIGNED                 | Yes  | NULL    | INDEX           |
| department_head     | tinyint(4)         |                          | No   | 0       |                 |
| properties         | longtext            | utf8mb4_bin              | Yes  | NULL    |                 |
| email_verified_at  | timestamp           |                          | Yes  | NULL    |                 |
| password           | varchar(255)        | utf8mb4_unicode_ci       | No   | None    |                 |
| deleted_at         | timestamp           |                          | Yes  | NULL    |                 |
| remember_token     | varchar(100)        | utf8mb4_unicode_ci       | Yes  | NULL    |                 |
| created_at         | timestamp           |                          | Yes  | NULL    |                 |
| updated_at         | timestamp           |                          | Yes  | NULL    |                 |

## Endpoints

1. **List All Users**
   - **Method**: `GET /api/users`
   - **Description**: Returns a paginated list of all users.
   - **Sample Response**:
     ```json
     {
       "success": true,
       "status": 200,
       "message": "Users fetched successfully",
       "data": [...]
     }
     ```

2. **Get a Single User**
   - **Method**: `GET /api/users/{id}`
   - **Description**: Fetch user by ID.
   - **Sample Response**:
     ```json
     {
       "id": 1,
       "name": "John Doe",
       "email": "john@example.com",
       "department": {
         "id": 2,
         "name": "Production"
       },
       "properties": {
         "is_internal_auditor": true
       }
     }
     ```

3. **Update a User**
   - **Method**: `PUT /api/users/{id}`
   - **Description**: Update an existing user's details.
   - **Sample Request**:
     ```json
     {
       "name": "John Updated",
       "status": "inactive",
       "department_id": 3
     }
     ```
   - **Sample Response**:
     ```json
     {
       "message": "Updated",
       "data": {
         "id": 1,
         "name": "John Updated"
       }
     }
     ```

## Model Utility Methods

- `isActive()`
- `isInternalAuditor()`
- `isIncidentAssignee()`
- `isObservationAssignee()`
- `getPermissionGroups()`
- `getPermissionsByGroupName($group)`
- `roleHasPermissions($role, $permissions)`

## Sample Input for User Creation

```json
{
  "name": "John Doe",
  "email": "john.doe@company.com",
  "username": "john.doe",
  "mobile": "9876543210",
  "password": "password123",
  "status": "active",
  "designation": "Software Engineer",
  "department_id": 1,
  "gender": "male",
  "location": "New Delhi",
  "date_joined": "2024-01-15",
  "is_internal_auditor": true,
  "is_incident_assignee": false,
  "is_observation_assignee": true,
  "is_indicator_assignee": false
}
```

## Notes

- Password is required during user creation.
- Uses soft deletes (tracked by `deleted_at`).
- Roles & permissions are managed using Spatie Laravel Permission.

## Contact

- For any issues or enhancements, please contact the backend team.

