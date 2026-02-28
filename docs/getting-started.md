# Getting Started

This guide walks you through everything you need to start using the **GitHub Repository Management API** subset documented in this project.

By the end of this section, you will:

- Generate a personal access token
- Authenticate requests
- Make your first API call
- Understand rate limits
- Know how to structure requests properly

---

## Prerequisites

Before you begin, make sure you have:

- A GitHub account
- Basic understanding of REST APIs
- A tool for making HTTP requests:

    - Swagger UI (recommended)
    - curl
    - Postman
    - Any HTTP client of your choice

---

# Step 1 — Generate a Personal Access Token

This API uses **Personal Access Tokens (PATs)** for authentication.

1. Go to **GitHub**
2. Open **Settings**
3. Navigate to **Developer settings**
4. Select **Personal access tokens**
5. Click **Generate new token**
6. Choose appropriate scopes (minimum: `repo`)
7. Copy and store the token securely

:exclamation: You will not be able to see the token again after closing the page.

---

# Step 2 — Authenticate Requests

All requests must include an `Authorization` header.

Authorization: `Bearer YOUR_PERSONAL_ACCESS_TOKEN`

Example using curl:

```curl
-H "Authorization: Bearer YOUR_PERSONAL_ACCESS_TOKEN" \
-H "Accept: application/vnd.github+json" \
https://api.github.com/user/repos
```

If authentication is successful, the API returns `200 OK`.

If authentication fails, you will receive: `401 Unauthorized`

See [**Errors & Status Codes**](./errors-and-status-codes.md) for details.

---

# Step 3 — Make Your First Request

Let’s retrieve a list of repositories for the authenticated user.

Endpoint:

```http
GET /user/repos
```

Full request:

```http
GET https://api.github.com/user/repos
```

Successful response: `200 OK`

```json
[
  {
    "id": 123456,
    "name": "example-repo",
    "private": false
  }
]
```

You have now successfully connected to the API.

---

# Understanding the Base URL

All endpoints use the following base URL:

``http
https://api.github.com
```

Requests follow this pattern:

```http
https://api.github.com/{resource}
```

Example:

```http
https://api.github.com/user/repos
```

---

# Request Structure

Each request typically includes:

- HTTP method (`GET`, `POST`, `PATCH`, `DELETE`)
- Endpoint path
- Headers
- Optional JSON body (for POST and PATCH)

Example POST request body:

```json
{
  "name": "new-repository",
  "private": false,
  "description": "Created via API"
}
```

---

# Rate Limiting

GitHub enforces rate limits on API usage.

Authenticated requests limit: _5,000 requests per hour per user_

If you exceed the limit, the API returns:

`403 Forbidden`

Response headers include:

`X-RateLimit-Limit`
`X-RateLimit-Remaining`
`X-RateLimit-Reset`  

To avoid rate limiting issues:

- Cache responses when possible
- Avoid unnecessary polling
- Monitor rate limit headers

---

# Common First Issues

### 401 Unauthorized

Cause:

- Missing or incorrect token

Fix:

- Verify Authorization header
- Regenerate token if necessary

---

### 403 Rate Limit Exceeded

Cause:

- Too many requests within one hour

Fix:

- Wait until reset time
- Implement request throttling

---

# Using Swagger UI (Recommended)

The easiest way to explore the API is through the interactive [Swagger UI](./swagger-ui.md).

Steps:

1. Generate a GitHub personal access token
2. Open the Swagger UI
3. Authorize using your token
4. Execute any endpoint

This allows you to:

- Experiment safely
- Inspect request/response structures
- Understand required parameters

---

# Next Steps

- [**API Reference**](./api-reference.md) for endpoint details
- [**Concepts Overview**](./concepts/overview.md) for architectural understanding
- [**Concepts Workflows**](./concepts/workflows.md) for practical usage patterns

You are now ready to start building with the GitHub Repository Management API.