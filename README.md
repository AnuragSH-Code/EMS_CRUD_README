# Employee Management System (EMS) API

A robust Employee Management System API built with Go and MongoDB, deployed on Kubernetes in the BREVO environment. This RESTful API provides complete CRUD operations for employee data management with advanced filtering and pagination capabilities.

## 🚀 Features

- **Complete CRUD Operations** - Create, Read, Update, and Delete employees
- **Advanced Filtering** - Search by firstname, lastname, role, department, email, and manager
- **Pagination Support** - Efficient data retrieval with customizable page size
- **MongoDB Integration** - Scalable NoSQL database backend
- **Kubernetes Deployment** - Cloud-native deployment on K8s
- **High Code Quality** - 83.7% test coverage on new code (SonarQube)
- **RESTful Design** - Clean and intuitive API endpoints

## 📋 Table of Contents

- [Live Demo](#live-demo)
- [API Endpoints](#api-endpoints)
- [API Documentation](#api-documentation)
- [Data Models](#data-models)
- [Error Handling](#error-handling)
- [Testing](#testing)
- [Deployment](#deployment)

## 🌐 Live Demo

**Default Live Link:** [https://ems-anurag-sh.brevo.dev/employees](https://ems-anurag-sh.brevo.dev/employees)

## 🔗 API Endpoints

### Base URL
```
https://ems-anurag-sh.brevo.dev
```

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/employees` | Retrieve all employees with filtering and pagination |
| POST | `/employee` | Create a new employee |
| PUT | `/employee/{id}` | Update an existing employee |
| DELETE | `/employee/{id}` | Delete an employee |

## 📖 API Documentation

### 1. Get All Employees

Retrieve employees with optional filtering and pagination.

**Endpoint:** `GET /employees`

**Query Parameters:**
- `page` (optional) - Page number (default: 1)
- `limit` (optional) - Items per page (default: 50)
- `firstname` (optional) - Filter by first name
- `lastname` (optional) - Filter by last name
- `role` (optional) - Filter by role
- `department` (optional) - Filter by department
- `email` (optional) - Filter by email (supports partial matching)
- `manager` (optional) - Filter by manager name

**Examples:**

```bash
# Get all employees
curl https://ems-anurag-sh.brevo.dev/employees

# Pagination
curl https://ems-anurag-sh.brevo.dev/employees?page=1&limit=1

# Filter by firstname
curl https://ems-anurag-sh.brevo.dev/employees?firstname=Sachin

# Multiple filters
curl "https://ems-anurag-sh.brevo.dev/employees?role=Tech%20Lead&department=Engineering"

# Email filter (partial matching)
curl https://ems-anurag-sh.brevo.dev/employees?email=@gmail.com

# Complex filtering with pagination
curl "https://ems-anurag-sh.brevo.dev/employees?firstname=Anurag&role=Software%20Engineer&page=1&limit=1"
```

**Response:**
```json
{
  "employees": [
    {
      "id": "6839a096341e67e493e1b986",
      "firstname": "Sachin",
      "lastname": "Raghav",
      "role": "Tech Lead",
      "department": "Engineering",
      "email": "sachin0.raghav@brevo.com",
      "contact_no": "1234567890",
      "manager": "Vivek"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total": 1,
    "has_next": false,
    "has_prev": false
  }
}
```

### 2. Create Employee

Create a new employee record.

**Endpoint:** `POST /employee`

**Request Body:**
```json
{
  "firstname": "Sachin",
  "lastname": "Raghav",
  "role": "Tech Lead",
  "department": "Engineering",
  "email": "sachin0.raghav@brevo.com",
  "contact_no": "1234567890",
  "manager": "Vivek"
}
```

**Example:**
```bash
curl -X POST https://ems-anurag-sh.brevo.dev/employee \
  -H "Content-Type: application/json" \
  -d '{
    "firstname": "Sachin",
    "lastname": "Raghav",
    "role": "Tech Lead",
    "department": "Engineering",
    "email": "sachin0.raghav@brevo.com",
    "contact_no": "1234567890",
    "manager": "Vivek"
  }'
```

### 3. Update Employee

Update an existing employee record.

**Endpoint:** `PUT /employee/{id}`

**Request Body:** (partial updates supported)
```json
{
  "firstname": "Shubam",
  "lastname": "Baheti"
}
```

**Example:**
```bash
curl -X PUT https://ems-anurag-sh.brevo.dev/employee/{id} \
  -H "Content-Type: application/json" \
  -d '{
    "firstname": "Shubam",
    "lastname": "Baheti"
  }'
```

**Response:**
```json
{
  "message": "Employee updated successfully"
}
```

### 4. Delete Employee

Delete an employee record.

**Endpoint:** `DELETE /employee/{id}`

**Example:**
```bash
curl -X DELETE https://ems-anurag-sh.brevo.dev/employee/{id}
```

**Response:**
```json
{
  "message": "Employee deleted successfully"
}
```

## 📊 Data Models

### Employee

```go
type Employee struct {
    ID          string `json:"id" bson:"_id,omitempty"`
    FirstName   string `json:"firstname" bson:"firstname"`
    LastName    string `json:"lastname" bson:"lastname"`
    Role        string `json:"role" bson:"role"`
    Department  string `json:"department" bson:"department"`
    Email       string `json:"email" bson:"email"`
    ContactNo   string `json:"contact_no" bson:"contact_no"`
    Manager     string `json:"manager" bson:"manager"`
}
```

### Pagination Response

```go
type PaginationResponse struct {
    Page    int  `json:"page"`
    Limit   int  `json:"limit"`
    Total   int  `json:"total"`
    HasNext bool `json:"has_next"`
    HasPrev bool `json:"has_prev"`
}
```

## ⚠️ Error Handling

The API returns appropriate HTTP status codes and error messages:

- `200 OK` - Successful operation
- `201 Created` - Resource created successfully
- `400 Bad Request` - Invalid request data
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

Error Response Format:
```json
{
  "error": "Error message description",
  "code": "ERROR_CODE"
}
```

## 🧪 Testing

The project maintains high code quality with **83.7% test coverage** on new code as verified by SonarQube.

### Running Tests

```bash
# Run all tests
go test ./...

# Run tests with coverage
go test -cover ./...

# Generate coverage report
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```

## 🚢 Deployment

### CI/CD Pipeline

- **CI**: GitHub Actions for continuous integration
- **CD**: ArgoCD for continuous deployment and GitOps workflow

### Kubernetes Deployment

The application is deployed on Kubernetes in the BREVO environment using:

- **Deployment**: Manages application pods
- **Service**: Exposes the application
- **Ingress**: Handles external traffic routing
- **ConfigMap**: Environment configuration
- **Secret**: Sensitive data management

## 🏗️ Technology Stack

- **Backend**: Go (Golang)
- **Database**: MongoDB
- **Container**: Docker
- **Orchestration**: Kubernetes
- **CI**: GitHub Actions
- **CD**: ArgoCD
- **Code Quality**: SonarQube
- **Cloud**: BREVO Cloud Infrastructure

## 📈 Performance

- **Scalable Architecture**: Kubernetes-based horizontal scaling
- **Efficient Pagination**: Optimized database queries
- **Connection Pooling**: MongoDB connection optimization
