
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

## Base URL
`https://api.employeedir.com/v1`

All endpoints listed below are relative to this base URL.  
API versioning is handled via the URL path.

---

<a id="authentication"></a>

## Authentication

All endpoints require a Bearer token from the internal Auth Service.

**Header Format:**

### Example (cURL)
```bash
curl -X GET https://api.employeedir.com/v1/employees \
  -H "Authorization: Bearer <your_token>"

```
### Example (JavaScript)
```
const resp = await fetch('https://api.employeedir.com/v1/employees?page=1&limit=2', {
  headers: { 'Authorization': `Bearer ${token}` }
});
console.log(await resp.json());
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

### Query Parameters

| Parameter  | Type   | Required | Description                                       |
|------------|--------|----------|---------------------------------------------------|
| `page`       | int    | No       | Page number to retrieve                          |
| `limit`     | int    | No       | Number of employees per page                     |
| `department` | string | No       | Filter by department name (case-insensitive)     |
| `location`   | string | No       | Filter by office location                        |


## Example Request

```http
GET /employees?page=1&limit=2
Authorization: Bearer YOUR_ACCESS_TOKEN
```

## Example Response
```json
{
  "page": 1,
  "limit": 2,
  "total": 134,
  "data": [
    {
      "id": "emp_001",
      "first_name": "Jon",
      "last_name": "Doe",
      "email": "jon.doe@example.com",
      "title": "Developer",
      "department": "Documentation",
      "location": "Glasgow"
    },
    {
      "id": "emp_002",
      "first_name": "Jane",
      "last_name": "Bloggs",
      "email": "jane.bloggs@example.com",
      "title": "DevOps Engineer",
      "department": "Infrastructure",
      "location": "Stoke"
    }
  ]
}
```

### Pagination Info

- `page`: Current page number
- `limit`: Max results per page
- `total`: Total matching employees

---

<a id="get-employeesid"></a>

## GET `/employees/{id}`

Retrieve full details for one employee.


## Example Request
```http
GET /employees/emp_001
Authorization: Bearer YOUR_ACCESS_TOKEN
```


## Example Response
```json
{
  "id": "emp_001",
  "first_name": "Jon",
  "last_name": "Doe",
  "email": "jon.doe@example.com",
  "title": "Technical Writer",
  "phone": "+44 701 234 5678",
  "manager_id": "emp_099",
  "department": "Documentation",
  "location": "Glasgow",
  "start_date": "2021-08-23"
}
```

---

<a id="post-`employees`"></a>

## POST `/employees`

Create a new employee.


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

## Common Error Responses

| Code | Meaning           | Sample Response |
|------|-------------------|-----------------|
| 400  | Bad request       | `{ "error": "Missing field: email" }` |
| 401  | Unauthorised      | `{ "error": "Invalid or missing token" }` |
| 404  | Resource not found | `{ "error": "Employee not found" }` |
| 500  | Server error      | `{ "error": "Unexpected error occurred" }` |
