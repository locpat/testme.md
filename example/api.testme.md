# TESTME: REST API

Tests for the REST API endpoints.

## Prerequisites

- API server is running
- Database is seeded with test data
- API authentication token is available

## Setup

1. Start the API server: `npm run start:api`
2. Wait for `http://localhost:3000/api/health` to return 200
3. Obtain auth token by calling POST `/api/auth/login` with test credentials

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| API_URL | http://localhost:3000/api | API base URL |
| AUTH_TOKEN | - | Bearer token for authenticated requests |
| TEST_USER_ID | 1 | ID of test user |

## Tests

### Health Check

### Test: Health endpoint returns 200

1. Send GET request to `/health`

**Expect:**
- Status code: 200
- Response body contains `{ "status": "ok" }`

---

### Authentication

### Test: Login returns token

1. Send POST request to `/auth/login`
   ```json
   {
     "email": "user@example.com",
     "password": "password123"
   }
   ```

**Expect:**
- Status code: 200
- Response contains `token` field
- Response contains `user` object with id, email, name

---

### Test: Login rejects invalid credentials

1. Send POST request to `/auth/login`
   ```json
   {
     "email": "user@example.com",
     "password": "wrongpassword"
   }
   ```

**Expect:**
- Status code: 401
- Response contains error message

---

### Test: Refresh token extends session

1. Send POST request to `/auth/refresh` with current token in header

**Expect:**
- Status code: 200
- New token returned
- New token has later expiration

---

### Users Endpoint

### Test: Get current user

1. Send GET request to `/users/me` with auth token

**Expect:**
- Status code: 200
- Response contains current user's data
- No password field in response

---

### Test: Get user by ID

1. Send GET request to `/users/1` with auth token

**Expect:**
- Status code: 200
- Response contains user object
- User ID matches request

---

### Test: Get user 404 for non-existent

1. Send GET request to `/users/99999` with auth token

**Expect:**
- Status code: 404
- Response contains error message "User not found"

---

### Test: Update user profile

1. Send PATCH request to `/users/me` with auth token
   ```json
   {
     "name": "Updated Name"
   }
   ```

**Expect:**
- Status code: 200
- Response shows updated name
- Subsequent GET reflects change

---

### Test: Cannot update other user's profile

1. Send PATCH request to `/users/2` with non-admin auth token
   ```json
   {
     "name": "Hacked Name"
   }
   ```

**Expect:**
- Status code: 403
- Profile unchanged

---

### Items Endpoint (CRUD)

### Test: List items with pagination

1. Send GET request to `/items?page=1&limit=10`

**Expect:**
- Status code: 200
- Response contains `items` array
- Response contains `total` count
- Response contains `page` and `limit`
- Items array has at most 10 items

---

### Test: Create new item

1. Send POST request to `/items` with auth token
   ```json
   {
     "name": "API Test Item",
     "description": "Created via API",
     "price": 29.99
   }
   ```

**Expect:**
- Status code: 201
- Response contains created item with ID
- Item has `createdAt` timestamp

---

### Test: Create item validates required fields

1. Send POST request to `/items` without name field
   ```json
   {
     "description": "Missing name"
   }
   ```

**Expect:**
- Status code: 400
- Response contains validation error
- Error mentions "name" field

---

### Test: Get single item

1. Create an item (as above) and note the ID
2. Send GET request to `/items/{id}`

**Expect:**
- Status code: 200
- Response contains item data matching ID

---

### Test: Update item

1. Send PUT request to `/items/{id}` with auth token
   ```json
   {
     "name": "Updated Item Name",
     "price": 39.99
   }
   ```

**Expect:**
- Status code: 200
- Response shows updated values
- `updatedAt` timestamp changed

---

### Test: Partial update with PATCH

1. Send PATCH request to `/items/{id}` with auth token
   ```json
   {
     "price": 19.99
   }
   ```

**Expect:**
- Status code: 200
- Only price is updated
- Other fields unchanged

---

### Test: Delete item

1. Create an item and note the ID
2. Send DELETE request to `/items/{id}` with auth token

**Expect:**
- Status code: 204 (or 200)
- Subsequent GET returns 404

---

### Search and Filtering

### Test: Search items by name

1. Send GET request to `/items?search=test`

**Expect:**
- Status code: 200
- All returned items contain "test" in name or description

---

### Test: Filter items by category

1. Send GET request to `/items?category=electronics`

**Expect:**
- Status code: 200
- All returned items have category "electronics"

---

### Test: Sort items by field

1. Send GET request to `/items?sort=price&order=asc`

**Expect:**
- Status code: 200
- Items sorted by price ascending
- First item has lowest price

---

### Error Handling

### Test: 401 for missing auth token

1. Send GET request to `/users/me` without auth header

**Expect:**
- Status code: 401
- Response contains "Unauthorized" or similar

---

### Test: 401 for expired token

1. Send request with expired auth token

**Expect:**
- Status code: 401
- Response indicates token expired

---

### Test: 400 for malformed JSON

1. Send POST request to `/items` with invalid JSON body

**Expect:**
- Status code: 400
- Error message about invalid JSON

---

### Test: 405 for unsupported method

1. Send PUT request to `/items` (collection, not single item)

**Expect:**
- Status code: 405
- Error indicates method not allowed

---

### Test: Rate limiting

> **Note:** Requires sending many requests quickly.

1. Send 100+ requests to any endpoint within 1 minute

**Expect:**
- After threshold, status code: 429
- Response includes retry-after header

---

### Batch Operations

### Test: Batch create items

1. Send POST request to `/items/batch`
   ```json
   {
     "items": [
       { "name": "Batch Item 1", "price": 10 },
       { "name": "Batch Item 2", "price": 20 },
       { "name": "Batch Item 3", "price": 30 }
     ]
   }
   ```

**Expect:**
- Status code: 201
- Response contains array of created items
- All 3 items have IDs

---

### Test: Batch delete items

1. Send DELETE request to `/items/batch`
   ```json
   {
     "ids": [1, 2, 3]
   }
   ```

**Expect:**
- Status code: 200
- Response confirms deleted count
- Items no longer retrievable

## Teardown

1. Delete all items created during tests
2. Reset test user profile to original values
