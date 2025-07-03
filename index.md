# 📘 Employee Directory API

The **Employee Directory API** enables internal teams and systems to retrieve, create, and manage employee records. This is intended for internal use only.

---

## 🔐 Authentication

All endpoints require a Bearer token from the internal Auth Service.

**Header Format:**
```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

---

## 📇 Endpoints Summary

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

## 📥 GET `/employees`

Retrieve a paginated list of employees.

### Query Parameters

| Name       | Type   | Description                    |
|------------|--------|--------------------------------|
| `page`     | int    | Page number (default: 1)        |
| `limit`    | int    | Results per page (default: 20) |
| `department` | string | Filter by department          |
| `location` | string | Filter by location             |

### ✅ Example Request

```http
GET /employees?page=1&limit=2
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### ✅ Example Response
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
      "location": "Lagos"
    }
  ]
}
```

---

## 🔍 GET `/employees/{id}`

Retrieve full details for one employee.

### ✅ Example Request
```http
GET /employees/emp_001
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### ✅ Example Response
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

## ➕ POST `/employees`

Create a new employee.

### ✅ Example Request
```http
POST /employees
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "first_name": "Victor",
  "last_name": "Adedokun",
  "email": "victor.adedokun@example.com",
  "title": "Technical Writer",
  "phone": "+44 123 456 7890",
  "manager_id": "emp_050",
  "department": "Documentation",
  "location": "Remote",
  "start_date": "2025-07-01"
}
```

### ✅ Example Response (201 Created)
```json
{
  "id": "emp_135",
  "message": "Employee successfully created."
}
```

---

## 🔁 PUT `/employees/{id}`

Replace all fields for an employee (overwrite mode).

### ✅ Example Request
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

### ✅ Example Response
```json
{
  "message": "Employee record replaced."
}
```

---

## ✏️ PATCH `/employees/{id}`

Update one or more fields for an existing employee.

### ✅ Example Request
```http
PATCH /employees/emp_135
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "title": "Principal Technical Writer",
  "location": "London"
}
```

### ✅ Example Response
```json
{
  "message": "Employee record updated."
}
```

---

## 🏢 GET `/departments`

Retrieve a list of departments.

### ✅ Response
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

## 🌍 GET `/locations`

Retrieve a list of office locations.

### ✅ Response
```json
[
  "London",
  "Lagos",
  "Glasgow",
  "Remote"
]
```

---

## ❗ Common Error Responses

| Code | Meaning           | Sample Response |
|------|-------------------|-----------------|
| 400  | Bad request       | `{ "error": "Missing field: email" }` |
| 401  | Unauthorized      | `{ "error": "Invalid or missing token" }` |
| 404  | Not found         | `{ "error": "Employee not found" }` |
| 500  | Server error      | `{ "error": "Unexpected error occurred" }` |
