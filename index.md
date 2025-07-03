
<div style="position: fixed; top: 100px; right: 20px; background: #f9f9f9; padding: 10px; border: 1px solid #ccc; width: 260px; z-index: 1000;">
  <strong>On this page</strong>
  <ul>
    <li><a href="#authentication">Authentication</a></li>
    <li><a href="#endpoints-summary">Endpoints Summary</a></li>
    <li><a href="#get-employees">GET /employees</a></li>
    <li><a href="#get-employeesid">GET /employees/{id}</a></li>
    <li><a href="#post-employees">POST /employees</a></li>
    <li><a href="#put-employeesid">PUT /employees/{id}</a></li>
    <li><a href="#patch-employeesid">PATCH /employees/{id}</a></li>
    <li><a href="#get-departments">GET /departments</a></li>
    <li><a href="#get-locations">GET /locations</a></li>
    <li><a href="#common-error-responses">Error Responses</a></li>
  </ul>
</div>


# Employee Directory API

The **Employee Directory API** enables internal teams to retrieve, create, and manage employee records. This is intended for internal use only.

---

<a id="authentication"></a>

## Authentication

All endpoints require a Bearer token from the internal Auth Service.

**Header Format:**
```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

---

<a id="endpoints-summary"></a>

## Endpoints Summary

| Method | Endpoint               | Description                          |
|--------|------------------------|--------------------------------------|
| GET    | `/employees`           | List employees                       |
| GET    | `/employees/{id}`      | Get a single employee by ID          |
| POST   | `/employees`           | Create a new employee                |
| PUT    | `/employees/{id}`      | Replace entire employee record       |
| PATCH  | `/employees/{id}`      | Update specific fields               |
| GET    | `/departments`         | List all departments                 |
| GET    | `/locations`           | List all office locations            |

---

<a id="get-employees"></a>

[<a name="get-`employees`"></a>]: # 

## GET `/employees`

Retrieve a paginated list of employees.

<a id="query-parameters"></a>

## Query Parameters

| Name       | Type   | Description                    |
|------------|--------|--------------------------------|
| `page`     | int    | Page number (default: 1)        |
| `limit`    | int    | Results per page (default: 20) |
| `department` | string | Filter by department          |
| `location` | string | Filter by location             |

#<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

## Example Request

```http
GET /employees?page=1&limit=2
Authorization: Bearer YOUR_ACCESS_TOKEN
```

#<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

## Example Response
```json
{
  "page": 1,
  "limit": 2,
  "total": 134,
  "data": [
    {
      "id": "emp_001",
      "first_name": "Victor",
      "last_name": "Adedokun",
      "email": "victor.adedokun@example.com",
      "title": "Senior Technical Writer",
      "department": "Documentation",
      "location": "Glasgow"
    },
    {
      "id": "emp_002",
      "first_name": "Amina",
      "last_name": "Yusuf",
      "email": "amina.yusuf@example.com",
      "title": "DevOps Engineer",
      "department": "Infrastructure",
      "location": "Stoke"
    }
  ]
}
```

---

<a id="get-employeesid"></a>

## GET `/employees/{id}`

Retrieve full details for one employee.

#<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

## Example Request
```http
GET /employees/emp_001
Authorization: Bearer YOUR_ACCESS_TOKEN
```

#<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

<a name="✅-example-response"></a>

## Example Response
```json
{
  "id": "emp_001",
  "first_name": "Victor",
  "last_name": "Adedokun",
  "email": "victor.adedokun@example.com",
  "title": "Senior Technical Writer",
  "phone": "+44 701 234 5678",
  "manager_id": "emp_099",
  "department": "Documentation",
  "location": "Glasgow",
  "start_date": "2021-08-23"
}
```

---

<a name="➕-post-`employees`"></a>

## POST `/employees`

Create a new employee.

#<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="✅-example-request"></a>

<a name="example-request"></a>

## Example Request
```http
POST /employees
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "title": "Technical Writer",
  "phone": "+44 123 456 7890",
  "manager_id": "emp_050",
  "department": "Documentation",
  "location": "OnSite",
  "start_date": "2025-07-01"
}
```

#<a name="example-response"></a>

<a name="example-response"></a>

<a name="example-response-(201-created)"></a>

<a name="example-response"></a>

<a name="example-response"></a>

## Example Response (201 Created)
```json
{
  "id": "emp_135",
  "message": "Employee successfully created."
}
```

---

<a id="put-employeesid"></a>

## PUT `/employees/{id}`

Replace all fields for an employee (overwrite mode).

#<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

## Example Request
```http
PUT /employees/emp_135
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "first_name": "Victor",
  "last_name": "Adedokun",
  "email": "v.adedokun@company.com",
  "title": "Lead Technical Writer",
  "phone": "+44 123 456 7890",
  "manager_id": "emp_099",
  "department": "Documentation",
  "location": "Remote",
  "start_date": "2025-07-01"
}
```

#<a name="example-response"></a>

<a name="example-response"></a>

<a name="example-response"></a>

<a name="example-response"></a>

## Example Response
```json
{
  "message": "Employee record replaced."
}
```

---

<a id="patch-employeesid"></a>

## PATCH `/employees/{id}`

Update one or more fields for an existing employee.

#<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

<a name="example-request"></a>

## Example Request
```http
PATCH /employees/emp_135
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "title": "Principal Technical Writer",
  "location": "London"
}
```

#<a name="example-response"></a>

<a name="example-response"></a>

<a name="example-response"></a>

<a name="example-response"></a>

## Example Response
```json
{
  "message": "Employee record updated."
}
```

---

<a id="get-departments"></a>

## GET `/departments`

Retrieve a list of departments.

#<a name="response"></a>

<a name="response"></a>

## Response
```json
[
  "Engineering",
  "Product",
  "Documentation",
  "People Ops",
  "Finance"
]
```

---

<a id="get-locations"></a>

## GET `/locations`

Retrieve a list of office locations.

#<a name="response"></a>

<a name="response"></a>

## Response
```json
[
  "London",
  "Lagos",
  "Glasgow",
  "Remote"
]
```

---

<a id="common-error-responses"></a> 

[<a name="❗-common-error-responses"></a>]: # 

## ❗ Common Error Responses

| Code | Meaning           | Sample Response |
|------|-------------------|-----------------|
| 400  | Bad request       | `{ "error": "Missing field: email" }` |
| 401  | Unauthorised      | `{ "error": "Invalid or missing token" }` |
| 404  | Resource not found | `{ "error": "Employee not found" }` |
| 500  | Server error      | `{ "error": "Unexpected error occurred" }` |
