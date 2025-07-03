# API Routes Documentation

This document lists the available API endpoints for Department and Department Area management, as well as authentication. All endpoints are protected and require specific headers.

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
| DELETE   | /api/departments/{department} | Tenant\DepartmentController@destroy      | Delete department     |

---

## Department Area Endpoints

| Method    | Endpoint                                | Controller & Method                                   | Description                  |
|-----------|-----------------------------------------|-------------------------------------------------------|------------------------------|
| GET       | /api/departments-area                   | Tenant\DepartmentAreaController@index                 | List department areas        |
| POST      | /api/departments-area                   | Tenant\DepartmentAreaController@store                 | Create department area       |
| GET       | /api/departments-area/{departments_area}| Tenant\DepartmentAreaController@show                  | Get department area          |
| PUT/PATCH | /api/departments-area/{departments_area}| Tenant\DepartmentAreaController@update                | Update department area       |
| DELETE    | /api/departments-area/{departments_area}| Tenant\DepartmentAreaController@destroy               | Delete department area       |

---

## Other Endpoints

| Method | Endpoint             | Controller & Method                             | Description            |
|--------|----------------------|-------------------------------------------------|------------------------|
| GET    | /sanctum/csrf-cookie | Laravel\Sanctum\CsrfCookieController@show       | Get CSRF cookie        |
| GET    | /storage/{path}      | storage.local                                   | Access storage files   |
| GET    | /                   | (root)                                          | (default home route)   |
| GET    | /up                 | (health check)                                  | Laravel health check   |

---

## Notes

- All endpoints (except `/`, `/up`, `/sanctum/csrf-cookie`, and `/storage/{path}`) require authentication using the headers above.
- All data should be sent and received in JSON format.
- For more details on request/response bodies, see the main `README.md`.

---