# API Documentation Standards

Last Updated: May 12, 2026

## 1. Overview

To ensure our APIs are easy to use, integrate, and maintain, all Lutervyn APIs must adhere to these documentation standards.

## 2. Specification Format

- **OpenAPI 3.0+**: All RESTful APIs must be documented using the OpenAPI Specification (OAS).
- **AsyncAPI**: For event-driven or message-based APIs.

## 3. Required Content

Every API endpoint must document:
- **Endpoint URL**: The full path.
- **HTTP Method**: GET, POST, PUT, DELETE, etc.
- **Authentication**: Required scopes or roles.
- **Parameters**: 
  - Path, Query, and Header parameters.
  - Data type, required/optional status, and description.
- **Request Body**: (For POST/PUT) Schema definition and example.
- **Responses**:
  - All possible HTTP status codes (2xx, 4xx, 5xx).
  - Response body schema and example for each code.
- **Error Messages**: Detailed explanations of error codes and how to resolve them.

## 4. Style & Clarity

- **Naming**: Use clear, consistent, and pluralized resource names (e.g., `/users`, `/projects`).
- **Descriptions**: Use active voice and avoid jargon where possible.
- **Examples**: Provide realistic, copy-pasteable examples for every request and response.

## 5. Versioning

- APIs must be versioned in the URL (e.g., `/v1/users`).
- Document the deprecation schedule for older versions.

## 6. Tools

- **Swagger/Redoc**: For generating interactive API documentation from OAS files.
- **Postman**: To provide official Postman Collections for external developers.
- **Git**: OAS files must be stored in the repository alongside the code.

---

**Effective Date**: May 12, 2026
**Next Review**: May 12, 2027
