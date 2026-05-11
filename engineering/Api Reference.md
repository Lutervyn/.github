# API Reference Documentation

Last Updated: May 12, 2024

## 1. Base URL

```
https://api.lutervyn.com/v1
```

## 2. Authentication

### 2.1 API Key

```
Authorization: Bearer YOUR_API_KEY
```

### 2.2 OAuth 2.0

- Authorization code flow
- Token refresh
- Scopes defined

## 3. Endpoints

### 3.1 Authentication

- POST /auth/login
- POST /auth/logout
- POST /auth/refresh
- POST /auth/register

### 3.2 Users

- GET /users
- GET /users/{id}
- POST /users
- PUT /users/{id}
- DELETE /users/{id}

### 3.3 Projects

- GET /projects
- GET /projects/{id}
- POST /projects
- PUT /projects/{id}
- DELETE /projects/{id}

## 4. Request Format

- Content-Type: application/json
- UTF-8 encoding
- JSON body
- HTTP methods
- Query parameters

## 5. Response Format

```json
{
  "success": true,
  "data": {},
  "errors": [],
  "meta": {}
}
```

## 6. Error Handling

- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 500: Server Error

## 7. Rate Limiting

- 1000 requests/hour
- Per API key
- Headers indicate limits
- Exponential backoff

## 8. Contact

- API Support: api@lutervyn.com
- Technical: tech@lutervyn.com
