
# 📘 Permissions API Documentation

This API allows managing permissions in your application. A permission can be a parent (e.g., `user`) or a child (e.g., `create user`), with the ability to nest using `parent_id`.

All routes are protected by Laravel Sanctum or Passport authentication.

---

## 🔐 Headers (All Requests)

| Key           | Value                |
|---------------|----------------------|
| Authorization | Bearer {access_token} |
| Accept        | application/json     |
| Content-Type  | application/json     |

---

## 📥 Create Permission

### `POST /api/permissions`

Creates a new permission, either as a parent or child.

### 🔸 Request Body

| Field       | Type     | Required | Description                                 |
|-------------|----------|----------|---------------------------------------------|
| name        | string   | Yes      | Name of the permission (must be unique)     |
| is_parent   | boolean  | No       | `true` if it's a parent permission          |
| parent_id   | integer  | Required if `is_parent` is false | ID of parent permission (must exist) |

### 🧪 Example Request

```json
{
  "name": "edit department",
  "is_parent": false,
  "parent_id": 1
}
````

### ✅ Success Response (201 Created)

```json
{
  "success": true,
  "status": 201,
  "message": "Permission created successfully",
  "data": {
    "id": 37,
    "name": "edit department",
    "guard_name": "api",
    "is_parent": false,
    "parent_id": 1,
    "created_at": "2025-07-30T12:00:00.000000Z",
    "updated_at": "2025-07-30T12:00:00.000000Z"
  }
}
```

### ❌ Validation Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The permission name has already been taken."],
    "parent_id": ["The parent_id field is required when is_parent is false."]
  }
}
```

---

## 📄 List All Permissions

### `GET /api/permissions`

Returns all parent permissions with their children.

### ✅ Response

```json
{
  "data": [
    {
      "id": 1,
      "name": "department",
      "children": [
        { "id": 2, "name": "manage department" },
        { "id": 3, "name": "create department" },
        { "id": 4, "name": "view department" },
        ...
      ]
    },
    {
      "id": 13,
      "name": "user",
      "children": [
        { "id": 14, "name": "manage user" },
        { "id": 15, "name": "create user" },
        ...
      ]
    }
  ],
  "current_page": 1,
  "per_page": 15,
  "total": 5,
  "last_page": 1
}
```

---

## 🔍 Show Permission by ID

### `GET /api/permissions/{id}`

Returns a single permission (no children).

### 🔁 URL Parameter

| Parameter | Type | Required | Description          |
| --------- | ---- | -------- | -------------------- |
| id        | int  | Yes      | ID of the permission |

### ✅ Example Response

```json
{
  "id": 1,
  "name": "department",
  "guard_name": "api",
  "is_parent": true,
  "created_at": "2025-07-10T11:26:35.000000Z",
  "updated_at": "2025-07-10T11:26:35.000000Z",
  "parent_id": null
}
```

### ❌ Not Found

```json
{
  "message": "Permission not found"
}
```


---

## 🧠 Validation Rules Summary

Handled by `PermissionRequest`:

| Field      | Rule                                                    |
| ---------- | ------------------------------------------------------- |
| name       | required, max:100, unique\:tenant.permissions,name,{id} |
| is\_parent | nullable, boolean                                       |
| parent\_id | required\_if\:is\_parent,false + exists\:permissions,id |

---

