# Test Case Template

## Basic Information

**Test Case ID:** `TC-XXX-XXX`  
**Test Case Name:** [Brief, descriptive name]  
**Module/Feature:** [e.g., Authentication, User Management]  
**Created By:** [Your name]  
**Created Date:** [Date]  
**Last Updated:** [Date]  
**Version:** [1.0]

## Test Details

**Priority:** [High / Medium / Low]  
**Test Type:** [Functional / Integration / E2E / Regression / Smoke]  
**Test Level:** [Unit / Integration / System / Acceptance]  
**Estimated Time:** [e.g., 5 minutes]

## Preconditions

[List conditions that must be met before test execution]

**Example:**
- User account exists with email: test@example.com
- Application is accessible at https://app.example.com
- User is not currently logged in
- Browser: Chrome (latest version)

## Test Data

[Specify test data needed]

**Example:**
- Valid Email: user@example.com
- Valid Password: Test123!
- Invalid Email: invalid@test.com
- Invalid Password: wrongpass

## Test Steps

| Step # | Action | Expected Result |
|--------|--------|-----------------|
| 1 | Navigate to login page | Login page is displayed |
| 2 | Enter email in email field | Email is entered correctly |
| 3 | Enter password in password field | Password is entered (masked) |
| 4 | Click "Login" button | System processes login request |
| 5 | Verify redirect | User is redirected to dashboard |
| 6 | Verify welcome message | "Welcome, [User Name]" is displayed |

## Expected Result

[Overall expected outcome]

**Example:**
User successfully logs in and is redirected to the dashboard with a personalized welcome message.

## Postconditions

[State after test execution]

**Example:**
- User is logged in
- Session is active
- User is on dashboard page

## Actual Result

[Fill during execution]

## Test Status

- [ ] Not Executed
- [ ] Pass
- [ ] Fail
- [ ] Blocked
- [ ] Skipped

## Defect ID

[If test fails, link to bug report]

**Defect ID:** [e.g., BUG-123]

## Notes/Comments

[Additional observations, edge cases, or follow-up actions]

---

## Example: Complete Test Case

**Test Case ID:** `TC-AUTH-001`  
**Test Case Name:** User Login with Valid Credentials  
**Module/Feature:** Authentication  
**Priority:** High  
**Test Type:** Functional  
**Test Level:** System

**Preconditions:**
- User account exists: testuser@example.com / Password123!
- Application is accessible
- User is logged out

**Test Steps:**

| Step # | Action | Expected Result |
|--------|--------|-----------------|
| 1 | Open browser and navigate to https://app.example.com/login | Login page loads successfully |
| 2 | Verify login form is displayed | Email and password fields are visible |
| 3 | Enter "testuser@example.com" in email field | Email is entered correctly |
| 4 | Enter "Password123!" in password field | Password is entered (shown as dots) |
| 5 | Click "Login" button | Loading indicator appears briefly |
| 6 | Wait for redirect | User is redirected to /dashboard |
| 7 | Verify URL contains "/dashboard" | URL is correct |
| 8 | Verify welcome message is displayed | Message "Welcome, Test User" is visible |
| 9 | Verify user menu shows logged-in user | User menu displays "Test User" |

**Expected Result:**  
User successfully logs in and is redirected to the dashboard with personalized welcome message and user menu updated.

**Actual Result:**  
[Fill during execution]

**Test Status:** [ ] Pass [ ] Fail [ ] Blocked

**Defect ID:** [If applicable]

**Notes:**  
- Test executed on Chrome 120.0
- Response time: 1.2 seconds
- No issues observed

