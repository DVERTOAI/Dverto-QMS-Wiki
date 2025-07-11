# API Routes Documentation

This document lists the available API endpoints for Department, Department Area, and Observation management, as well as authentication. All endpoints are protected and require specific headers.

---

## Required Headers

For all authenticated requests, include the following headers:

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <your_token>
domain: psri.com
```
- Replace `<your_token>` with the token received from the login response.

---

## Authentication Endpoints

| Method | Endpoint                        | Controller & Method             | Description                        |
|--------|---------------------------------|---------------------------------|------------------------------------|
| POST   | /api/auth/tenant/login          | Tenant\AuthController@login     | Tenant login (returns token)       |
| POST   | /api/auth/tenant/logout         | Tenant\AuthController@logout    | Tenant logout                      |

---

## Department Endpoints

| Method   | Endpoint                      | Controller & Method                      | Description           |
|----------|-------------------------------|------------------------------------------|-----------------------|
| GET      | /api/departments              | Tenant\DepartmentController@index        | List departments      |
| POST     | /api/departments              | Tenant\DepartmentController@store        | Create department     |
| GET      | /api/departments/{department} | Tenant\DepartmentController@show         | Get department        |
| PUT/PATCH| /api/departments/{department} | Tenant\DepartmentController@update       | Update department     |

---

## Department Area Endpoints

| Method    | Endpoint                                | Controller & Method                                   | Description                  |
|-----------|-----------------------------------------|-------------------------------------------------------|------------------------------|
| GET       | /api/departments-area                   | Tenant\DepartmentAreaController@index                 | List department areas        |
| POST      | /api/departments-area                   | Tenant\DepartmentAreaController@store                 | Create department area       |
| GET       | /api/departments-area/{departments_area}| Tenant\DepartmentAreaController@show                  | Get department area          |
| PUT/PATCH | /api/departments-area/{departments_area}| Tenant\DepartmentAreaController@update                | Update department area       |

---

## Observation Endpoints

| Method    | Endpoint                            | Controller & Method                         | Description                  |
|-----------|-------------------------------------|---------------------------------------------|------------------------------|
| GET       | /api/observation                    | Tenant\ObservationController@index         | List observations            |
| POST      | /api/observation                    | Tenant\ObservationController@store         | Create observation           |
| GET       | /api/observation/{observation}      | Tenant\ObservationController@show          | Get observation              |
| PUT/PATCH | /api/observation/{observation}      | Tenant\ObservationController@update        | Update observation           |

---

## Users Endpoints

| Method    | Endpoint                            | Controller & Method                         | Description                  |
|-----------|-------------------------------------|---------------------------------------------|------------------------------|
| GET       | /api/users                    | Tenant\UserController@index         | List User            |
| POST      | /api/users                    | Tenant\UserController@store         | Create User           |
| GET       | /api/users/{users}      | Tenant\UserController@show          | Get User              |
| PUT/PATCH | /api/users/{users}      | Tenant\UserController@update        | Update User           |

---

## System Endpoints

| Method | Endpoint             | Controller & Method                             | Description            |
|--------|----------------------|-------------------------------------------------|------------------------|
| GET    | /sanctum/csrf-cookie | Laravel\Sanctum\CsrfCookieController@show       | Get CSRF cookie        |
| GET    | /storage/{path}      | storage.local                                   | Access storage files   |
| GET    | /                   | (root)                                          | Default home route     |
| GET    | /up                 | (health check)                                  | Laravel health check   |

---

## Complete Route List Summary

Based on your Laravel route list, here are all 18 available routes:

### Authentication (2 routes)
- `POST /api/auth/tenant/login`
- `POST /api/auth/tenant/logout`

### Departments (4 routes)
- `GET /api/departments`
- `POST /api/departments`
- `GET /api/departments/{department}`
- `PUT/PATCH /api/departments/{department}`

### Department Areas (4 routes)
- `GET /api/departments-area`
- `POST /api/departments-area`
- `GET /api/departments-area/{departments_area}`
- `PUT/PATCH /api/departments-area/{departments_area}`

### Observations (4 routes)
- `GET /api/observation`
- `POST /api/observation`
- `GET /api/observation/{observation}`
- `PUT/PATCH /api/observation/{observation}`

### Users (4 routes)
- `GET /api/users`
- `POST /api/users`
- `GET /api/users/{users}`
- `PUT/PATCH /api/users/{users}`

### System Routes (4 routes)
- `GET /`
- `GET /sanctum/csrf-cookie`
- `GET /storage/{path}`
- `GET /up`

---

## Notes

- **Authentication Required:** All API endpoints (except system routes) require authentication using the headers above.
- **Data Format:** All data should be sent and received in JSON format.
- **File Uploads:** For observation endpoints with file attachments, use `multipart/form-data` encoding.
- **Domain Header:** The `domain: psri.com` header is mandatory for all authenticated requests.
- **Route Parameters:** Use the actual IDs when accessing specific resources (e.g., `/api/observation/14`).
- **HTTP Methods:** Both `PUT` and `PATCH` are supported for update operations.

---

## Quick Reference

| Resource         | List All | Create | Get Single | Update | Delete |
|------------------|----------|--------|------------|--------|--------|
| Departments      | ✓        | ✓      | ✓          | ✓      | ✗      |
| Department Areas | ✓        | ✓      | ✓          | ✓      | ✗      |
| Observations     | ✓        | ✓      | ✓          | ✓      | ✗      |
| Users            | ✓        | ✓      | ✓          | ✓      | ✗      |

**Note:** Delete operations are not available for any of the main resources in this API version.

---

For detailed request/response examples and validation rules, refer to the individual API documentation files for each resource type.