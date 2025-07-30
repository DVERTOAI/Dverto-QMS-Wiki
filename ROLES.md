
# 📘 Roles API Documentation

This API manages user roles and their permissions. It allows full CRUD (Create, Read, Update, Delete) operations.

All endpoints are protected by Sanctum authentication and require a valid `Bearer` token.

---

## 🔐 Authorization Header

| Key           | Value                  |
|---------------|------------------------|
| Authorization | Bearer {access_token}  |
| Accept        | application/json       |

---

## 📋 Endpoints Summary

| Method | Endpoint             | Description               |
|--------|----------------------|---------------------------|
| GET    | /api/roles           | List all roles            |
| GET    | /api/roles/{id}      | Get a role by ID          |
| POST   | /api/permissions     | Create a new role         |
| PUT    | /api/roles/{id}      | Update an existing role   |
| DELETE | /api/roles/{id}      | Delete a role             |

---

## 🆕 Create Role

### `POST /api/permissions`

Creates a new role with permissions.

### Request Body

```json
{
  "name": "HR Manager",
  "permissions": ["manage department", "view employee"]
}
````

### Response (201 Created)

```json
{
  "guard_name": "api",
  "name": "HR Manager",
  "is_parent": false,
  "parent_id": null,
  "updated_at": "2025-07-30T10:38:38.000000Z"
}
```

---

## 📖 Get All Roles

### `GET /api/roles`

Fetch a list of all roles.

### Sample Response

```json
[
  {
    "id": 1,
    "name": "Admin",
    "guard_name": "api",
    "permissions": ["manage users", "manage roles"]
  },
  {
    "id": 2,
    "name": "Staff",
    "guard_name": "api",
    "permissions": ["view dashboard"]
  }
]
```

---

## 🔍 Get Role by ID

### `GET /api/roles/{id}`

Fetch details of a single role using its ID.

### URL Parameters

| Parameter | Type | Required | Description      |
| --------- | ---- | -------- | ---------------- |
| id        | int  | Yes      | Role ID to fetch |

### Sample Response

```json
{
  "id": 2,
  "name": "Staff",
  "guard_name": "api",
  "permissions": ["view dashboard"]
}
```

---

## 📝 Update Role

### `PUT /api/roles/{id}`

Update a role's name and permissions.

### Request Body

```json
{
  "name": "Updated Role Name",
  "permissions": ["permission1", "permission2"]
}
```

### Sample Success Response

```json
{
  "success": true,
  "message": "Role updated successfully",
  "data": {
    "id": 2,
    "name": "Updated Role Name",
    "permissions": ["permission1", "permission2"]
  }
}
```

---

## 🗑️ Delete Role

### `DELETE /api/roles/{id}`

Delete a role by ID.

### URL Parameters

| Parameter | Type | Required | Description       |
| --------- | ---- | -------- | ----------------- |
| id        | int  | Yes      | Role ID to delete |

### Response

```json
{
  "success": true,
  "status": 200,
  "message": "Role deleted successfully",
  "data": null
}
```

---

## ❌ Error Responses

| Code | Example Message               | Reason                          |
| ---- | ----------------------------- | ------------------------------- |
| 401  | `Unauthorized`                | Missing or invalid bearer token |
| 403  | `Forbidden`                   | Insufficient permission         |
| 404  | `Role not found`              | Invalid role ID                 |
| 422  | `The name field is required.` | Validation failed               |

---

## 📎 Notes

* `permissions` array must contain valid **permission names**, not IDs.
* Role creation uses `/api/permissions` endpoint.
* Make sure roles and permissions sync logic is handled properly on backend.
* Returns are usually wrapped with success + message structure for update/delete.

---
