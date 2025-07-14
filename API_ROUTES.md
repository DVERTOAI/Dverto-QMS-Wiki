# API Routes Documentation

This document lists the available API endpoints for Department, Department Area, Designation, Observation management, and User management, as well as authentication. All endpoints are protected and require specific headers.

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

## Designation Endpoints

| Method    | Endpoint                            | Controller & Method                         | Description                  |
|-----------|-------------------------------------|---------------------------------------------|------------------------------|
| GET       | /api/designations                   | Tenant\DesignationController@index         | List designations            |
| POST      | /api/designations                   | Tenant\DesignationController@store         | Create designation           |
| GET       | /api/designations/{designation}     | Tenant\DesignationController@show          | Get designation              |
| PUT/PATCH | /api/designations/{designation}     | Tenant\DesignationController@update        | Update designation           |

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
| GET       | /api/users                          | Tenant\UserController@index                | List users                   |
| POST      | /api/users                          | Tenant\UserController@store                | Create user                  |
| GET       | /api/users/{users}                  | Tenant\UserController@show                 | Get user                     |
| PUT/PATCH | /api/users/{users}                  | Tenant\UserController@update               | Update user                  |

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

Based on your Laravel route list, here are all 22 available routes:

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

### Designations (4 routes)
- `GET /api/designations`
- `POST /api/designations`
- `GET /api/designations/{designation}`
- `PUT/PATCH /api/designations/{designation}`

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


---

## Notes

- **Authentication Required:** All API endpoints (except system routes) require authentication using the headers above.
- **Data Format:** All data should be sent and received in JSON format.
- **File Uploads:** For observation endpoints with file attachments, use `multipart/form-data` encoding.
- **Domain Header:** The `domain: psri.com` header is mandatory for all authenticated requests.
- **Route Parameters:** Use the actual IDs when accessing specific resources (e.g., `/api/designations/5`).
- **HTTP Methods:** Both `PUT` and `PATCH` are supported for update operations.
- **Unique Names:** For designations, names must be unique among active records only.

---

## Quick Reference

| Resource         | List All | Create | Get Single | Update | Delete |
|------------------|----------|--------|------------|--------|--------|
| Departments      | ✓        | ✓      | ✓          | ✓      | ✗      |
| Department Areas | ✓        | ✓      | ✓          | ✓      | ✗      |
| Designations     | ✓        | ✓      | ✓          | ✓      | ✗      |
| Observations     | ✓        | ✓      | ✓          | ✓      | ✗      |
| Users            | ✓        | ✓      | ✓          | ✓      | ✗      |

**Note:** Delete operations are not available for any of the main resources in this API version.

---

## Resource Features

| Resource         | Search | Sort | Pagination | Unique Constraint |
|------------------|--------|------|------------|-------------------|
| Departments      | ✓      | ✓    | ✓          | Active names only |
| Department Areas | ✓      | ✓    | ✓          | All names         |
| Designations     | ✓      | ✓    | ✓          | Active names only |
| Observations     | ✓      | ✓    | ✓          | No                |
| Users            | ✓      | ✓    | ✓          | Email/Username    |

---

For detailed request/response examples and validation rules, refer to the individual API documentation files for each resource type.