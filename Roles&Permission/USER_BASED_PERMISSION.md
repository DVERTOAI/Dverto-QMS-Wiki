

## ➕ Assign Permissions to User (Bulk)

### `POST /api/permissions/assign`

Assigns one or more permissions to a user by ID. All permissions in the array must exist.

---

### 🔸 Request Body

```json
{
  "userId": "3",
  "permission": [
    "view department",
    "create department"
  ]
}
```

> Note: `userId` should be an integer (string-to-int coercion may be tolerated depending on implementation), and `permission` is an array of permission names.

---

### 🎯 Validation Rules & Custom Messages

| Field         | Rule                                | Custom Message                                                                                                                                                   |
| ------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| userId        | required, integer, exists\:users,id | `'userId.required' => 'User ID is required.'`<br>`'userId.integer' => 'User ID must be an integer.'`<br>`'userId.exists' => 'The selected user does not exist.'` |
| permission    | required, array                     | `'permission.required' => 'At least one permission is required.'`<br>`'permission.array' => 'Permissions must be an array.'`                                     |
| permission.\* | string, exists\:permissions,name    | `'permission.*.string' => 'Each permission must be a string.'`<br>`'permission.*.exists' => 'One or more permissions do not exist.'`                             |

---

### 🧪 Example Request

```json
{
  "userId": 5,
  "permission": [
    "edit department",
    "view department"
  ]
}
```

---

### ✅ Success Response (200 OK)

```json
{
  "success": true,
  "status": 200,
  "message": "Permission assigned successfully",
  "data": true
}
```

---

### ❌ Validation Error Examples (422 Unprocessable Entity)

**Missing userId:**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "userId": ["User ID is required."]
  }
}
```

**Non-integer userId:**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "userId": ["User ID must be an integer."]
  }
}
```

**User does not exist:**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "userId": ["The selected user does not exist."]
  }
}
```

**Permission not provided / not an array:**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "permission": ["At least one permission is required."]
  }
}
```

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "permission": ["Permissions must be an array."]
  }
}
```

**Invalid permission entries:**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "permission.0": ["Each permission must be a string."],
    "permission.1": ["One or more permissions do not exist."]
  }
}
```

---

### ❌ User Not Found (404)

```json
{
  "message": "User not found."
}
```
