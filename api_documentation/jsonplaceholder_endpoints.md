# JSONPlaceholder API Documentation

## API Security Analysis Reference

---

## Base URL

```
https://jsonplaceholder.typicode.com
```

---

## Available Resources

### Posts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /posts | List all posts |
| GET | /posts/1 | Get single post |
| GET | /posts/1/comments | Get post comments |
| POST | /posts | Create new post |
| PUT | /posts/1 | Update post |
| DELETE | /posts/1 | Delete post |

**Post Object Structure:**
```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident...",
  "body": "quia et suscipit\nsuscipit recusandae..."
}
```

---

### Users

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /users | List all users |
| GET | /users/1 | Get single user |
| GET | /users/1/posts | Get user's posts |
| GET | /users/1/todos | Get user's todos |

**User Object Structure:**
```json
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874",
    "geo": {
      "lat": "-37.3159",
      "lng": "81.1496"
    }
  },
  "phone": "1-770-736-8031 x56442",
  "website": "hildegard.org",
  "company": {
    "name": "Romaguera-Crona",
    "catchPhrase": "Multi-layered client-server neural-net",
    "bs": "harness real-time e-markets"
  }
}
```

**Security Note:** The user object exposes significant PII including email, phone, full address, and geolocation coordinates.

---

### Comments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /comments | List all comments |
| GET | /comments?postId=1 | Filter by post |

**Comment Object Structure:**
```json
{
  "postId": 1,
  "id": 1,
  "name": "id labore ex et quam laborum",
  "email": "Eliseo@gardner.biz",
  "body": "laudantium enim quasi est quidem magnam..."
}
```

**Security Note:** Comments expose email addresses. Endpoint returns 500 records (157KB) without pagination.

---

### Todos

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /todos | List all todos |
| GET | /todos?userId=1 | Filter by user |

**Todo Object Structure:**
```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

---

### Photos

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /photos | List all photos |
| GET | /photos?albumId=1 | Filter by album |

**Photo Object Structure:**
```json
{
  "albumId": 1,
  "id": 1,
  "title": "accusamus beatae ad facilis cum similique...",
  "url": "https://via.placeholder.com/600/92c952",
  "thumbnailUrl": "https://via.placeholder.com/150/92c952"
}
```

---

## Query Parameters

### Filtering

| Parameter | Example | Description |
|-----------|---------|-------------|
| userId | /posts?userId=1 | Filter by user ID |
| postId | /comments?postId=1 | Filter by post ID |
| albumId | /photos?albumId=1 | Filter by album ID |

---

## Response Headers

### Present Headers

| Header | Value | Security Purpose |
|--------|-------|------------------|
| Content-Type | application/json; charset=utf-8 | Data format |
| X-Content-Type-Options | nosniff | Prevents MIME sniffing |
| X-RateLimit-Limit | 1000 | Rate limit ceiling |
| X-RateLimit-Remaining | 999 | Remaining requests |

### Missing Security Headers

| Header | Security Purpose | Status |
|--------|------------------|--------|
| Strict-Transport-Security | Enforces HTTPS | ✗ Missing |
| X-Frame-Options | Clickjacking protection | ✗ Missing |
| Content-Security-Policy | XSS protection | ✗ Missing |
| X-XSS-Protection | Legacy XSS filter | ✗ Missing |
| Access-Control-Allow-Origin | CORS control | ✗ Missing |

---

## Security Analysis Summary

### Authentication
- **Status:** Not Required
- **Risk Level:** CRITICAL
- **Finding:** All endpoints accessible without authentication

### Authorization
- **Status:** Not Implemented
- **Risk Level:** HIGH
- **Finding:** No resource-level authorization checks

### Data Exposure
- **Status:** Excessive
- **Risk Level:** HIGH
- **Finding:** Complete user objects with PII exposed

### Rate Limiting
- **Status:** Partial
- **Risk Level:** MEDIUM
- **Finding:** Headers present but no enforcement on large responses

### Input Validation
- **Status:** None
- **Risk Level:** MEDIUM
- **Finding:** Arbitrary JSON accepted without validation

---

## Sample API Requests

### Get All Users
```bash
curl https://jsonplaceholder.typicode.com/users
```

### Get Single User
```bash
curl https://jsonplaceholder.typicode.com/users/1
```

### Create Post
```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "foo", "body": "bar", "userId": 1}'
```

### Update Post
```bash
curl -X PUT https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"id": 1, "title": "foo", "body": "bar", "userId": 1}'
```

### Delete Post
```bash
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

---

## Security Testing Commands

### Test Authentication
```bash
# Should fail in secure API, but succeeds here
curl https://jsonplaceholder.typicode.com/users
```

### Test Data Exposure
```bash
# Check what PII is exposed
curl https://jsonplaceholder.typicode.com/users/1 | jq
```

### Test Write Operations
```bash
# Create without auth (should fail in secure API)
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "test", "body": "test", "userId": 999}'
```

### Check Response Headers
```bash
curl -I https://jsonplaceholder.typicode.com/users
```

---

## Response Size Analysis

| Endpoint | Records | Size |
|----------|---------|------|
| /users | 10 | 5,645 bytes |
| /posts | 100 | 27,520 bytes |
| /comments | 500 | 157,745 bytes |
| /todos | 200 | 24,311 bytes |
| /photos | 5000 | ~1.2 MB |

---

## References

- [JSONPlaceholder Official](https://jsonplaceholder.typicode.com/)
- [JSONPlaceholder Guide](https://jsonplaceholder.typicode.com/guide/)
- [JSON Server](https://github.com/typicode/json-server)

---

**Document Version:** 1.0  
**Last Updated:** March 22, 2026  
**Classification:** API Security Reference
