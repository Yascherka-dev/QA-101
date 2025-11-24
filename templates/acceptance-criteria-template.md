# Acceptance Criteria Template

## What are Acceptance Criteria?

**Definition:** Clear, testable conditions that a user story must meet to be considered "done" and ready for release.

**Purpose:**
- Define "done" clearly
- Guide development
- Guide testing
- Enable validation
- Prevent scope creep

---

## Template Formats

### Format 1: Given-When-Then (BDD Style)

**Structure:**
```
Given [initial context/precondition]
When [action/trigger]
Then [expected outcome]
```

**Example:**
```
Given: User is on the login page
When: User enters valid email and password and clicks "Login"
Then: User is redirected to dashboard and sees welcome message
```

### Format 2: Scenario-Based

**Structure:**
```
Scenario: [Scenario name]
  Given [precondition]
  When [action]
  Then [outcome]
  And [additional outcome]
```

**Example:**
```
Scenario: Successful login
  Given I am on the login page
  When I enter valid credentials and click "Login"
  Then I am redirected to the dashboard
  And I see a welcome message with my name
```

### Format 3: Checklist Format

**Structure:**
- [ ] [Criterion 1]
- [ ] [Criterion 2]
- [ ] [Criterion 3]

**Example:**
- [ ] User can log in with valid email and password
- [ ] User is redirected to dashboard after successful login
- [ ] Welcome message displays user's name
- [ ] Error message displays for invalid credentials
- [ ] Password field masks input

### Format 4: Detailed Narrative

**Structure:**
```
As a [user type]
I want to [action]
So that [benefit]

Acceptance Criteria:
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]
```

---

## Complete Template

### User Story Information

**User Story ID:** [e.g., US-101]  
**User Story Title:** [Brief title]  
**As a:** [User type]  
**I want to:** [Action/feature]  
**So that:** [Benefit/value]

### Acceptance Criteria

**AC-1:** [Criterion 1]  
**Given:** [Precondition]  
**When:** [Action]  
**Then:** [Expected outcome]

**AC-2:** [Criterion 2]  
**Given:** [Precondition]  
**When:** [Action]  
**Then:** [Expected outcome]

**AC-3:** [Criterion 3]  
**Given:** [Precondition]  
**When:** [Action]  
**Then:** [Expected outcome]

### Edge Cases

- [Edge case 1]
- [Edge case 2]

### Non-Functional Requirements

- [Performance requirement]
- [Security requirement]
- [Accessibility requirement]

### Definition of Done

- [ ] All acceptance criteria met
- [ ] Code reviewed
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Manual testing completed
- [ ] No critical/high bugs
- [ ] Documentation updated

---

## Examples

### Example 1: User Login

**User Story:**
As a registered user, I want to log into the application so that I can access my account and personalized content.

**Acceptance Criteria:**

**AC-1: Successful Login with Valid Credentials**
- **Given:** User is on the login page and has a valid account
- **When:** User enters valid email and password and clicks "Login"
- **Then:** User is authenticated and redirected to the dashboard
- **And:** Welcome message displays: "Welcome, [User Name]"
- **And:** User menu shows logged-in user's name

**AC-2: Login Failure with Invalid Credentials**
- **Given:** User is on the login page
- **When:** User enters invalid email or password and clicks "Login"
- **Then:** Error message displays: "Invalid email or password"
- **And:** User remains on the login page
- **And:** Password field is cleared (email remains)

**AC-3: Login with Empty Fields**
- **Given:** User is on the login page
- **When:** User clicks "Login" without entering email or password
- **Then:** Validation messages display:
  - "Email is required" below email field
  - "Password is required" below password field
- **And:** User remains on the login page

**AC-4: Remember Me Functionality**
- **Given:** User is on the login page
- **When:** User enters credentials, checks "Remember Me", and clicks "Login"
- **Then:** User is logged in
- **And:** Credentials are saved in browser (for next visit)
- **And:** Next time user visits, email is pre-filled

**Edge Cases:**
- Login with special characters in email
- Login with very long password
- Login after session timeout
- Login with account that is locked/suspended

**Non-Functional Requirements:**
- Login request completes within 2 seconds
- Password is transmitted over HTTPS only
- Failed login attempts are logged for security
- Login form is accessible via keyboard navigation

---

### Example 2: User Registration

**User Story:**
As a new user, I want to create an account so that I can access the application and its features.

**Acceptance Criteria:**

**AC-1: Successful Registration**
- **Given:** User is on the registration page
- **When:** User fills all required fields with valid data and clicks "Register"
- **Then:** Account is created successfully
- **And:** Confirmation message displays: "Account created successfully. Please check your email to verify your account."
- **And:** Verification email is sent to user's email address
- **And:** User is redirected to login page

**AC-2: Registration Form Validation**
- **Given:** User is on the registration page
- **When:** User submits form with invalid data
- **Then:** Appropriate validation messages display:
  - Invalid email format: "Please enter a valid email address"
  - Weak password: "Password must be at least 8 characters and contain uppercase, lowercase, number, and special character"
  - Password mismatch: "Passwords do not match"
  - Missing required field: "[Field name] is required"

**AC-3: Duplicate Email Registration**
- **Given:** User is on the registration page
- **When:** User enters an email that already exists and submits form
- **Then:** Error message displays: "An account with this email already exists. Please log in or use a different email."
- **And:** User remains on registration page
- **And:** Other fields retain their values

**Required Fields:**
- First Name
- Last Name
- Email
- Password
- Confirm Password
- Terms and Conditions (checkbox)

**Password Requirements:**
- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character (!@#$%^&*)

**Edge Cases:**
- Registration with email containing special characters
- Registration with very long name fields
- Registration with international characters
- Registration attempt during system maintenance

**Non-Functional Requirements:**
- Registration completes within 3 seconds
- Email is sent within 30 seconds
- Form is accessible and keyboard navigable
- All user data is encrypted in transit and at rest

---

### Example 3: Product Search (API)

**User Story:**
As a user, I want to search for products so that I can find items I'm looking for quickly.

**Acceptance Criteria:**

**AC-1: Successful Product Search**
- **Given:** User is on the products page
- **When:** User enters a search term (e.g., "laptop") and clicks "Search" or presses Enter
- **Then:** API returns products matching the search term
- **And:** Results are displayed in a grid/list view
- **And:** Results show product name, price, and image
- **And:** Results count displays: "X products found"

**AC-2: Empty Search Results**
- **Given:** User is on the products page
- **When:** User searches for a term with no matches (e.g., "xyzabc123")
- **Then:** Message displays: "No products found matching your search"
- **And:** Suggestions display: "Try different keywords" or "Browse all products"

**AC-3: Search with Filters**
- **Given:** User has performed a search
- **When:** User applies filters (price range, category, brand)
- **Then:** Search results are filtered accordingly
- **And:** Active filters are displayed
- **And:** User can remove individual filters

**API Requirements:**
- Endpoint: `GET /api/products/search?q={query}`
- Response time: < 500ms
- Returns: JSON with products array
- Supports pagination: `?page=1&limit=20`

**Edge Cases:**
- Search with special characters
- Search with very long query
- Search with empty string
- Search with SQL injection attempt (should be sanitized)

**Non-Functional Requirements:**
- Search results load within 1 second
- API handles 100 concurrent search requests
- Search is case-insensitive
- Special characters are handled safely (no SQL injection)

---

## Best Practices

### ✅ DO:

- Write clear, specific, testable criteria
- Use Given-When-Then format for clarity
- Include both positive and negative scenarios
- Specify exact error messages
- Include edge cases
- Consider non-functional requirements
- Make criteria measurable (e.g., "within 2 seconds")
- Review with team before development starts

### ❌ DON'T:

- Write vague criteria ("it should work")
- Use technical jargon users don't understand
- Forget negative test cases
- Skip edge cases
- Make assumptions
- Write criteria that can't be tested
- Change criteria mid-sprint without discussion

---

## Checklist for Writing Acceptance Criteria

- [ ] Criteria are clear and unambiguous
- [ ] Criteria are testable (can write test cases)
- [ ] Criteria include positive scenarios
- [ ] Criteria include negative scenarios
- [ ] Criteria include edge cases
- [ ] Criteria specify exact messages/behaviors
- [ ] Criteria are written from user perspective
- [ ] Criteria are reviewed and agreed upon by team
- [ ] Criteria align with user story value
- [ ] Non-functional requirements are considered

