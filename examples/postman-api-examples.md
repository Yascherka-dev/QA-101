# Postman API Testing Examples

## Setup and Configuration

### Environment Variables

Create a Postman environment with these variables:

```json
{
  "base_url": "https://api.example.com/v1",
  "admin_token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user_token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "test_user_id": "user-123",
  "test_email": "testuser@example.com"
}
```

---

## Example 1: Authentication API

### Login Request

**Method:** `POST`  
**URL:** `{{base_url}}/auth/login`  
**Headers:**
```
Content-Type: application/json
```

**Body (raw JSON):**
```json
{
  "email": "user@example.com",
  "password": "Password123!"
}
```

**Tests (JavaScript):**
```javascript
// Test status code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Test response time
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// Test response structure
pm.test("Response has access_token", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('access_token');
    pm.expect(jsonData.access_token).to.be.a('string');
});

// Save token for future requests
pm.test("Save access token", function () {
    var jsonData = pm.response.json();
    pm.environment.set("user_token", "Bearer " + jsonData.access_token);
});

// Test token expiration
pm.test("Token has expiration", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('expires_in');
    pm.expect(jsonData.expires_in).to.be.a('number');
});
```

### Login with Invalid Credentials

**Method:** `POST`  
**URL:** `{{base_url}}/auth/login`  
**Body:**
```json
{
  "email": "wrong@example.com",
  "password": "WrongPassword"
}
```

**Tests:**
```javascript
pm.test("Status code is 401", function () {
    pm.response.to.have.status(401);
});

pm.test("Error message is correct", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
    pm.expect(jsonData.error).to.eql('Invalid credentials');
});

pm.test("No access token in response", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.not.have.property('access_token');
});
```

### Refresh Token

**Method:** `POST`  
**URL:** `{{base_url}}/auth/refresh`  
**Headers:**
```
Authorization: {{user_token}}
Content-Type: application/json
```

**Body:**
```json
{
  "refresh_token": "{{refresh_token}}"
}
```

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("New access token received", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('access_token');
    
    // Update token
    pm.environment.set("user_token", "Bearer " + jsonData.access_token);
});
```

---

## Example 2: User Management API

### Get All Users

**Method:** `GET`  
**URL:** `{{base_url}}/users`  
**Headers:**
```
Authorization: {{admin_token}}
```

**Query Parameters:**
- `page`: 1
- `limit`: 20
- `sort`: createdAt
- `order`: desc

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response has users array", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('users');
    pm.expect(jsonData.users).to.be.an('array');
});

pm.test("Pagination metadata is present", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('total');
    pm.expect(jsonData).to.have.property('page');
    pm.expect(jsonData).to.have.property('limit');
    pm.expect(jsonData).to.have.property('totalPages');
});

pm.test("Each user has required fields", function () {
    var jsonData = pm.response.json();
    jsonData.users.forEach(function(user) {
        pm.expect(user).to.have.property('id');
        pm.expect(user).to.have.property('email');
        pm.expect(user).to.have.property('firstName');
        pm.expect(user).to.have.property('lastName');
        pm.expect(user).to.not.have.property('password'); // Security check
    });
});
```

### Create User

**Method:** `POST`  
**URL:** `{{base_url}}/users`  
**Headers:**
```
Authorization: {{admin_token}}
Content-Type: application/json
```

**Body:**
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane.smith@example.com",
  "password": "SecurePass123!",
  "role": "user"
}
```

**Pre-request Script:**
```javascript
// Generate unique email to avoid duplicates
var timestamp = new Date().getTime();
pm.environment.set("test_email", "test" + timestamp + "@example.com");
```

**Body (with variable):**
```json
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "{{test_email}}",
  "password": "SecurePass123!",
  "role": "user"
}
```

**Tests:**
```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("User created successfully", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('user');
    pm.expect(jsonData.user.email).to.eql(pm.environment.get("test_email"));
    
    // Save user ID for future requests
    pm.environment.set("created_user_id", jsonData.user.id);
});

pm.test("Password is not returned", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.user).to.not.have.property('password');
});

pm.test("Response time is acceptable", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

### Get User by ID

**Method:** `GET`  
**URL:** `{{base_url}}/users/{{created_user_id}}`  
**Headers:**
```
Authorization: {{admin_token}}
```

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("User data is correct", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.user.id).to.eql(pm.environment.get("created_user_id"));
    pm.expect(jsonData.user.email).to.eql(pm.environment.get("test_email"));
});
```

### Update User

**Method:** `PATCH`  
**URL:** `{{base_url}}/users/{{created_user_id}}`  
**Headers:**
```
Authorization: {{admin_token}}
Content-Type: application/json
```

**Body:**
```json
{
  "firstName": "Jane Updated",
  "phone": "555-9999"
}
```

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("User updated successfully", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.user.firstName).to.eql("Jane Updated");
    pm.expect(jsonData.user.phone).to.eql("555-9999");
    pm.expect(jsonData.user).to.have.property('updatedAt');
});
```

### Delete User

**Method:** `DELETE`  
**URL:** `{{base_url}}/users/{{created_user_id}}`  
**Headers:**
```
Authorization: {{admin_token}}
```

**Tests:**
```javascript
pm.test("Status code is 204", function () {
    pm.response.to.have.status(204);
});

pm.test("User is deleted", function () {
    // Verify user no longer exists
    pm.sendRequest({
        url: pm.environment.get("base_url") + "/users/" + pm.environment.get("created_user_id"),
        method: 'GET',
        header: {
            'Authorization': pm.environment.get("admin_token")
        }
    }, function (err, res) {
        pm.expect(res.code).to.eql(404);
    });
});
```

---

## Example 3: Product API

### Search Products

**Method:** `GET`  
**URL:** `{{base_url}}/products/search`  
**Query Parameters:**
- `q`: laptop
- `page`: 1
- `limit`: 20
- `sort`: price
- `order`: asc

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Products array is returned", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('products');
    pm.expect(jsonData.products).to.be.an('array');
});

pm.test("Products are sorted by price ascending", function () {
    var jsonData = pm.response.json();
    var prices = jsonData.products.map(p => p.price);
    var sortedPrices = [...prices].sort((a, b) => a - b);
    pm.expect(prices).to.eql(sortedPrices);
});

pm.test("Each product has required fields", function () {
    var jsonData = pm.response.json();
    jsonData.products.forEach(function(product) {
        pm.expect(product).to.have.property('id');
        pm.expect(product).to.have.property('name');
        pm.expect(product).to.have.property('price');
        pm.expect(product.price).to.be.a('number');
    });
});
```

### Get Product by ID

**Method:** `GET`  
**URL:** `{{base_url}}/products/product-123`  

**Tests:**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Product details are complete", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.product).to.have.property('id');
    pm.expect(jsonData.product).to.have.property('name');
    pm.expect(jsonData.product).to.have.property('description');
    pm.expect(jsonData.product).to.have.property('price');
    pm.expect(jsonData.product).to.have.property('images');
    pm.expect(jsonData.product.images).to.be.an('array');
});
```

---

## Example 4: Error Handling Tests

### 400 Bad Request

**Method:** `POST`  
**URL:** `{{base_url}}/users`  
**Body (missing required field):**
```json
{
  "firstName": "John"
  // Missing email and password
}
```

**Tests:**
```javascript
pm.test("Status code is 400", function () {
    pm.response.to.have.status(400);
});

pm.test("Error message indicates missing fields", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
    pm.expect(jsonData.error).to.include('required');
});
```

### 401 Unauthorized

**Method:** `GET`  
**URL:** `{{base_url}}/users`  
**Headers:** (No Authorization header)

**Tests:**
```javascript
pm.test("Status code is 401", function () {
    pm.response.to.have.status(401);
});

pm.test("Error message indicates authentication required", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
    pm.expect(jsonData.error).to.include('Unauthorized');
});
```

### 404 Not Found

**Method:** `GET`  
**URL:** `{{base_url}}/users/nonexistent-id-12345`  
**Headers:**
```
Authorization: {{admin_token}}
```

**Tests:**
```javascript
pm.test("Status code is 404", function () {
    pm.response.to.have.status(404);
});

pm.test("Error message indicates resource not found", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
    pm.expect(jsonData.error).to.include('not found');
});
```

### 409 Conflict (Duplicate)

**Method:** `POST`  
**URL:** `{{base_url}}/users`  
**Body:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "existing@example.com",
  "password": "Password123!"
}
```

**Tests:**
```javascript
pm.test("Status code is 409", function () {
    pm.response.to.have.status(409);
});

pm.test("Error indicates duplicate", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('error');
    pm.expect(jsonData.error).to.include('already exists');
});
```

---

## Example 5: Collection Runner

### Collection Structure

```
User Management API
├── Authentication
│   ├── Login
│   ├── Login Invalid
│   └── Refresh Token
├── Users
│   ├── Get All Users
│   ├── Create User
│   ├── Get User by ID
│   ├── Update User
│   └── Delete User
└── Products
    ├── Search Products
    └── Get Product by ID
```

### Collection-Level Tests

Add to collection's "Tests" tab:

```javascript
// Global test - runs after each request
pm.test("Response time is acceptable", function () {
    pm.expect(pm.response.responseTime).to.be.below(5000);
});

pm.test("No server errors", function () {
    pm.expect(pm.response.code).to.not.be.oneOf([500, 502, 503, 504]);
});
```

### Collection Variables

```json
{
  "base_url": "https://api.example.com/v1",
  "timeout": 5000
}
```

---

## Example 6: Advanced Testing

### Data-Driven Testing with CSV

**CSV File (users.csv):**
```csv
firstName,lastName,email,expectedStatus
John,Doe,john.doe@example.com,201
Jane,Smith,jane.smith@example.com,201
Invalid,User,invalid-email,400
```

**Collection Runner:**
- Select collection
- Data: Upload CSV file
- Iterations: Use data file

**Request Body (with variables):**
```json
{
  "firstName": "{{firstName}}",
  "lastName": "{{lastName}}",
  "email": "{{email}}",
  "password": "Test123!"
}
```

**Tests:**
```javascript
pm.test("Status code matches expected", function () {
    var expectedStatus = parseInt(pm.iterationData.get("expectedStatus"));
    pm.response.to.have.status(expectedStatus);
});
```

### Chaining Requests

**Request 1: Create User**
```javascript
// Save user ID
var jsonData = pm.response.json();
pm.environment.set("user_id", jsonData.user.id);
```

**Request 2: Get Created User** (uses `{{user_id}}`)

**Request 3: Update User** (uses `{{user_id}}`)

**Request 4: Delete User** (uses `{{user_id}}`)

### Environment Switching

Create multiple environments:
- `Development`: `https://dev-api.example.com`
- `Staging`: `https://staging-api.example.com`
- `Production`: `https://api.example.com`

Switch environments in Postman to test different stages.

---

## Best Practices

### ✅ DO:

- Use environment variables for URLs and tokens
- Write comprehensive tests for each request
- Test both success and error scenarios
- Use collection runner for regression testing
- Save tokens/IDs from responses for chaining
- Test response times
- Validate response schemas
- Test edge cases (empty, null, special characters)

### ❌ DON'T:

- Hardcode values (use variables)
- Skip error scenario testing
- Ignore response times
- Test only happy paths
- Forget to clean up test data

---

## Quick Reference

**Common Assertions:**
```javascript
// Status code
pm.response.to.have.status(200);

// Response time
pm.expect(pm.response.responseTime).to.be.below(500);

// JSON property exists
pm.expect(jsonData).to.have.property('key');

// Value equals
pm.expect(jsonData.value).to.eql('expected');

// Array length
pm.expect(jsonData.items).to.have.lengthOf(10);

// Type check
pm.expect(jsonData.id).to.be.a('string');
pm.expect(jsonData.price).to.be.a('number');
```

**Common Variables:**
- `{{base_url}}` - API base URL
- `{{token}}` - Authentication token
- `{{user_id}}` - User ID from previous request
- `pm.environment.get("var")` - Get environment variable
- `pm.environment.set("var", "value")` - Set environment variable

