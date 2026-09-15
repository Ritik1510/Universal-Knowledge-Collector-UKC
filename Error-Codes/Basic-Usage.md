Definition: HTTP status codes are standard responses from a server indicating whether a specific web or API request has been successfully completed. Codes in the 4xx range represent client-side errors (issues with what was sent), while codes in the 5xx range represent server-side errors (issues with the server itself).

## Quick Reference Table

| Code | Name | Primary Use Case | Simple Analogy  |
| --- | --- | --- | --- |
| 400 | Bad Request | Malformed syntax, unreadable request body, or generic client errors. | "I don't understand what you are saying."  |
| 401 | Unauthorized | Missing or invalid authentication credentials. | "Who are you? Please show your ID."  |
| 403 | Forbidden | Client is recognized but does not have permission. | "I know who you are, but you aren't allowed in here."  |
| 404 | Not Found | Resource does not exist, or you want to hide its existence. | "What you are looking for isn't here."  |
| 409 | Conflict | Request conflicts with the current state of the database. | "You can't use that name; it's already taken."  |
| 422 | Unprocessable Entity | Syntax is correct, but data fails business rules or validation. | "I understand your request, but the data makes no sense."  |
| 500 | Internal Server Error | A generic crash or unhandled exception on the server. | "Something broke on our end, we are looking into it."  |

## 400 — Bad Request

**Definition:** The server cannot process the request because it's malformed, syntactically incorrect, or contains invalid data that the server can't understand at a structural level.

**Why to use:** To tell the client "your request itself is broken" — before any business logic even runs.

**When to use:**
- Malformed JSON/XML syntax
- Missing required fields in the request body
- Invalid data types (sending a string where a number is expected)
- Invalid query parameters
- Request body exceeds expected format

**Where to use:** At the input parsing/validation layer, before touching the database or business logic.

**Purpose:** Client-side error — signals "fix your request format and try again."

**Example:**
```json
POST /users
{ "email": "not-an-email", "age": "twenty" }

Response: 400 Bad Request
{ "error": "Invalid email format; age must be a number" }
```

---

## 401 — Unauthorized

**Definition:** The request lacks valid authentication credentials, or the credentials provided are invalid/expired. (Despite the name, this is about **authentication**, not authorization.)

**Why to use:** To tell the client "we don't know who you are" — you must authenticate first.

**When to use:**
- Missing auth token/API key
- Expired JWT/session token
- Invalid username/password
- Malformed Authorization header

**Where to use:** At the authentication middleware layer, before any route logic executes.

**Purpose:** Prompts the client to log in, refresh a token, or resend valid credentials. Should be paired with a `WWW-Authenticate` header per spec.

**Example:**
```
GET /account
Authorization: Bearer expired_token

Response: 401 Unauthorized
{ "error": "Token expired, please log in again" }
```

---

## 403 — Forbidden

**Definition:** The server understood the request and the client is authenticated, but the client does not have permission to access the resource. Authentication won't help — access is deliberately denied.

**Why to use:** To distinguish "you're logged in, but you're not allowed" from "we don't know you" (401).

**When to use:**
- A regular user trying to access admin-only endpoints
- Accessing another user's private resource
- Feature-flagged or role-restricted actions
- IP-based blocking, banned accounts

**Where to use:** At the authorization/permission-check layer, after authentication succeeds.

**Purpose:** Tells the client the request was valid and identity was verified, but the action is blocked by policy — retrying won't help without a permission change.

**Example:**
```
DELETE /admin/users/5
(Authenticated as a normal user)

Response: 403 Forbidden
{ "error": "You do not have permission to perform this action" }
```

---

## 404 — Not Found

**Definition:** The server cannot find the requested resource. Either it doesn't exist, was deleted, or the URL is wrong.

**Why to use:** To indicate the endpoint/resource itself doesn't exist — distinct from "you can't access it" (403).

**When to use:**
- Requesting a resource by an ID that doesn't exist
- Wrong or mistyped endpoint URL
- Resource was deleted
- Sometimes used deliberately **instead of 403** to hide the existence of a resource from unauthorized users (security through obscurity)

**Where to use:** At the routing layer or after a database lookup returns no result.

**Purpose:** Clear signal that there's nothing at that URI/resource identifier.

**Example:**
```
GET /users/99999

Response: 404 Not Found
{ "error": "User not found" }
```

---

## 409 — Conflict

**Definition:** The request conflicts with the current state of the server/resource. The request is valid, but can't be completed due to a conflict.

**Why to use:** To flag a state-based conflict, not a validation or auth issue.

**When to use:**
- Trying to create a resource that already exists (duplicate email, username)
- Concurrent edit conflicts (optimistic locking/version mismatch)
- Trying to delete a resource that has dependent records
- State machine violations (e.g., trying to "ship" an order that's already "cancelled")

**Where to use:** At the business logic layer, after checking current resource state against the requested change.

**Purpose:** Tells the client the conflict must be resolved (e.g., refresh data, choose a different value) before retrying.

**Example:**
```json
POST /users
{ "email": "existing@example.com" }

Response: 409 Conflict
{ "error": "A user with this email already exists" }
```

---

## 422 — Unprocessable Entity

**Definition:** The request is syntactically correct (well-formed JSON, right structure) but contains semantic errors — it fails validation rules even though the format is fine.

**Why to use:** To separate "your JSON syntax is broken" (400) from "your JSON is valid but the values don't make sense/pass business rules" (422).

**When to use:**
- Field values fail validation rules (e.g., password too short, invalid date range, value out of allowed enum)
- Business rule violations that aren't about server state (unlike 409)
- Cross-field validation failures (e.g., end date before start date)

**Where to use:** At the validation layer, after parsing succeeds but before deeper processing.

**Purpose:** Distinguishes structural errors from semantic/validation errors, useful for form validation feedback in APIs (heavily used in REST APIs like Rails, Laravel).

**Example:**
```json
POST /signup
{ "password": "123", "confirmPassword": "456" }

Response: 422 Unprocessable Entity
{ "errors": { "password": "must be at least 8 characters", "confirmPassword": "does not match password" } }
```

**Note:** 400 vs 422 is often debated/blurred in practice — many APIs use 400 for everything. But strictly: 400 = malformed request, 422 = well-formed but semantically invalid.

---

## 500 — Internal Server Error

**Definition:** A generic catch-all error indicating something went wrong on the server side, and the server doesn't know how to be more specific — it's not the client's fault.

**Why to use:** To signal an unexpected failure in server code, not something the client can fix by changing their request.

**When to use:**
- Unhandled exceptions/crashes in server code
- Database connection failures
- Null pointer/reference errors
- Third-party service failures not properly caught
- Any unexpected bug

**Where to use:** As the final fallback/catch-all in error-handling middleware, when no more specific error applies.

**Purpose:** Alerts developers (via logs/monitoring) that something needs fixing on the backend. Should never leak stack traces or sensitive info to the client in production.

**Example:**
```
GET /orders/123
(Database connection drops mid-query)

Response: 500 Internal Server Error
{ "error": "Something went wrong. Please try again later." }
```

---

## Key distinctions worth remembering
- **400 vs 422:** syntax broken vs syntax fine but data/values wrong.
- **401 vs 403:** don't know who you are vs know who you are but you can't do this.
- **403 vs 404:** sometimes 404 is used deliberately instead of 403 to avoid revealing a resource exists.
- **409 vs 422:** 409 is about conflicting with existing *state* (e.g., duplicate, version mismatch); 422 is about the data itself failing validation rules, independent of current state.

