# API and Postman – Introduction (From an API Tester’s Perspective)

## 1. What is an API? (Tester’s View)

An **API (Application Programming Interface)** is a contract that allows two software systems to communicate with each other.

From an **API tester’s perspective**, an API is about:

- Request correctness
- Response accuracy
- Data integrity
- Security
- Performance
- Error handling

APIs are tested **before UI** because:

- APIs contain the core business logic
- UI is only a consumer of APIs
- Bugs found early are cheaper to fix

---

## 2. Why API Testing is Critical

API testing ensures:

- Business logic works independently of UI
- Multiple clients receive consistent data
- Security issues are caught early
- Backend systems scale reliably

### API vs UI Testing

| Aspect | API Testing | UI Testing |
|------|-------------|------------|
| Speed | Fast | Slow |
| Stability | High | Low |
| Scope | Business logic | User interaction |
| Maintenance | Low | High |

---

## 3. What is REST API?

**REST (Representational State Transfer)** is the most common API architecture.

### REST Principles

1. Client–Server separation
2. Stateless communication
3. Resource-based URLs
4. HTTP methods define actions
5. Uniform interface
6. Standard HTTP status codes

---

## 4. REST API Components

### 4.1 Endpoint

```url
https://api.example.com/api/books/10
```

- `/api/books` → resource
- `10` → resource ID

Tester validates:

- Correct routing
- Invalid IDs
- URL formatting

---

### 4.2 HTTP Methods

| Method | Purpose |
|------|---------|
| GET | Retrieve data |
| POST | Create data |
| PUT | Update full resource |
| PATCH | Partial update |
| DELETE | Remove resource |
| OPTIONS | Allowed methods |
| HEAD | Headers only |

---

## 5. HTTP Request Structure

### Request Line

```url
POST /api/books HTTP/1.1
```

### Headers

Common headers:

- Content-Type

- Accept
- Authorization
- Cache-Control

Example:

```header
Content-Type: application/json
Authorization: Bearer <token>
```

### Request Body

```json
{
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "price": 499
}
```

Tester checks:

- Required fields
- Data types
- Invalid JSON
- Boundary values

---

## 6. HTTP Response

### 6.1 Status Codes

| Code | Meaning |
|-----|--------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Validation Error |
| 500 | Server Error |
| 503 | Service Unavailable |

---

### 6.2 Response Headers

- Content-Type
- Date
- Correlation IDs
- Security headers

### 6.3 Response Body

```json
{
  "id": 10,
  "title": "Clean Code",
  "author": "Robert C. Martin"
}
```

Tester validates:

- Schema
- Field names
- Null handling
- Extra fields

---

## 7. What is Postman?

Postman is an API testing and development tool used to:

- Send HTTP requests
- Validate responses
- Automate test cases
- Test authentication flows

---

## 8. Why API Testers Use Postman

- No UI dependency
- Fast manual testing
- Automation support
- Environment management
- Collaboration

---

## 9. Core Postman Features

### Collections

- Group related APIs

### Environments

- Store base URLs, tokens, IDs

### Authorization

- Basic Auth
- Bearer Token
- OAuth 2.0
- API Keys

### Tests (Scripts)

```javascript

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

---

## 10. Types of API Testing

- Functional Testing
- Validation Testing
- Security Testing
- Negative Testing
- Performance Testing
- Regression Testing
- Integration Testing

---

## 11. Common API Testing Scenarios

- Missing mandatory fields
- Invalid HTTP method
- Expired tokens
- Duplicate requests
- Invalid content type
- Role-based access checks

---

## 12. API Tester Mindset

An API tester thinks like:

- A developer
- A security tester
- An end user
- A system architect

**Strong API testing ensures stability, security, and scalability before UI exists.**
