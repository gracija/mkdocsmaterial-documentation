# Errors & Status Codes

This section explains how the API communicates failures, what the most common errors mean, and how to troubleshoot them effectively.

---

## HTTP Status Codes Overview

The API uses standard HTTP status codes to indicate the outcome of a request.

### 2xx — Success

| Code | Meaning | Description |
|------|----------|------------|
| 200  | OK | Request succeeded and a response body is returned. |
| 201  | Created | Resource was successfully created. |
| 204  | No Content | Request succeeded but no response body is returned. |

---

### 4xx — Client Errors

These errors indicate that something is wrong with the request.

| Code | Meaning | Description |
|------|----------|------------|
| 400 | Bad Request | Invalid parameters or malformed request. |
| 401 | Unauthorized | Missing or invalid authentication credentials. |
| 403 | Forbidden | Authenticated but not allowed to access the resource. |
| 404 | Not Found | The requested resource does not exist. |
| 409 | Conflict | Request conflicts with the current state of the resource. |
| 422 | Unprocessable Entity | Validation failed. |

---

### 5xx — Server Errors

These errors indicate a problem on the server side.

| Code | Meaning | Description |
|------|----------|------------|
| 500 | Internal Server Error | Unexpected server error. |
| 502 | Bad Gateway | Upstream dependency failed. |
| 503 | Service Unavailable | Service temporarily unavailable. |
| 504 | Gateway Timeout | Upstream service timed out. |

---

# Error Response Format

All errors follow a consistent JSON structure.

```json
{
  "error": {
    "code": "validation_error",
    "message": "The 'email' field is required.",
    "details": [
      {
        "field": "email",
        "issue": "missing"
      }
    ],
    "request_id": "req_123456789"
  }
}
```

### Fields Explained

| Field | Description |
|-------|------------|
| code | Machine-readable error identifier. |
| message | Human-readable explanation of the error. |
| details | Optional list of field-level validation issues. |
| request_id | Unique request identifier for support/debugging. |

---

# Common Errors

Below are the most frequently encountered errors and how to resolve them.

---

## Authentication Errors

### 401 Unauthorized

**Cause:**

- Missing Authorization header
- Expired token
- Invalid API key

**Example Response:**

```json
{
  "error": {
    "code": "unauthorized",
    "message": "Invalid or missing authentication token."
  }
}
```

**Fix:**

- Ensure the Authorization header is present
- Verify the token is not expired
- Regenerate API credentials if needed

---

## Permission Errors

### 403 Forbidden

**Cause:**

- Insufficient permissions
- Accessing another user's resource

**Fix:**

- Verify your account role
- Check resource ownership
- Contact an administrator if access is required

---

## Validation Errors

### 422 Unprocessable Entity

**Cause:**

- Missing required fields
- Invalid field format
- Business rule violation

**Example:**

```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid input.",
    "details": [
        { 
            "field": "email",
            "issue": "invalid_format" 
        }
    ]
  }
}
```

**Fix:**

- Review the request body
- Ensure required fields are included
- Validate data formats before sending

---

## Resource Not Found

### 404 Not Found

**Cause:**

- Incorrect ID
- Resource deleted
- Wrong endpoint

**Fix:**

- Verify the resource ID
- Confirm the endpoint URL
- Ensure the resource exists

---

## Conflict Errors

### 409 Conflict

**Cause:**

- Duplicate resource
- Version mismatch
- Concurrent modification

**Fix:**

- Check if the resource already exists
- Retry with updated version
- Implement idempotency where supported

---

# How to Troubleshoot Errors

Follow this systematic approach when debugging API issues.

---

## 1. Check the Status Code

Start with the HTTP status code:

- 4xx → Review your request
- 5xx → Retry or contact support

---

## 2. Read the Error Code

Use the machine-readable `error.code` field to determine the exact failure type.

---

## 3. Inspect Validation Details

If `details` are present:
- Identify the problematic field
- Check formatting and required constraints
- Correct and retry

---

## 4. Use the Request ID

Each error includes a `request_id`.

If contacting support, include:

- The full error response
- The request ID
- Timestamp of the request
- Endpoint used

---

## 5. Enable Logging

When integrating the API:

- Log full request and response bodies (excluding sensitive data)
- Log status codes
- Log request IDs
- Log timestamps

This dramatically reduces debugging time.

---

## 6. Retry Strategy

For temporary failures (502, 503, 504):

Recommended retry pattern:

- Use exponential backoff
- Retry up to 3 times
- Avoid immediate repeated retries

---

# Error Handling Best Practices

- Always check HTTP status codes
- Do not rely solely on response body
- Handle 401 by refreshing tokens
- Implement idempotency for write operations
- Surface meaningful error messages to end users
- Never expose raw server errors directly in production UI

---

# Summary

The API uses:

- Standard HTTP status codes
- Consistent structured error responses
- Machine-readable error identifiers
- Request IDs for traceability

Proper logging and structured error handling will make integrations stable, predictable, and easier to maintain.