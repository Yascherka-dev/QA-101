# Chapter 3 Exercises: Designing Tests

## Exercise 1: Writing Test Cases

### Scenario

Feature: User Login  
User Story: "As a user, I want to log into the application so that I can access my account."

### Task

Write a test case for "Login with valid credentials" using the test case template.

**Test Case ID:** TC-AUTH-001  
**Test Case Name:** ________________________________  
**Priority:** ________________________________  
**Preconditions:** ________________________________  
**Test Steps:**
1. ________________________________
2. ________________________________
3. ________________________________
4. ________________________________

**Expected Result:** ________________________________

### Sample Answer

**Test Case Name:** User Login with Valid Credentials  
**Priority:** High  
**Preconditions:** User account exists (testuser@example.com / Password123!), app is accessible  
**Test Steps:**
1. Navigate to login page
2. Enter "testuser@example.com" in email field
3. Enter "Password123!" in password field
4. Click "Login" button

**Expected Result:** User is logged in and redirected to dashboard

---

## Exercise 2: Acceptance Criteria

### Scenario

User Story: "As a user, I want to reset my password so that I can regain access to my account."

### Task

Write 3 acceptance criteria in Given-When-Then format:

**AC-1:**
- Given: ________________________________
- When: ________________________________
- Then: ________________________________

**AC-2:**
- Given: ________________________________
- When: ________________________________
- Then: ________________________________

**AC-3:**
- Given: ________________________________
- When: ________________________________
- Then: ________________________________

### Sample Answer

**AC-1: Request Password Reset**
- Given: User is on the login page
- When: User clicks "Forgot Password?" and enters valid email
- Then: User receives password reset email within 30 seconds

**AC-2: Reset Password with Valid Token**
- Given: User has received password reset email
- When: User clicks reset link and enters new password
- Then: Password is updated and user can log in with new password

**AC-3: Reset Password with Invalid Token**
- Given: User has an expired or invalid reset token
- When: User attempts to reset password
- Then: Error message displays "Invalid or expired reset link"

---

## Exercise 3: Gherkin Scenarios

### Scenario

Feature: Add Product to Cart

### Task

Write a Gherkin scenario for "Adding a product to cart":

```gherkin
Feature: Add Product to Cart
  As a customer
  I want to add products to my cart
  So that I can purchase multiple items
  
  Scenario: ________________________________
    Given ________________________________
    When ________________________________
    Then ________________________________
    And ________________________________
```

### Sample Answer

```gherkin
Feature: Add Product to Cart
  As a customer
  I want to add products to my cart
  So that I can purchase multiple items
  
  Scenario: Add product to cart from product page
    Given I am viewing a product page
    And the product is in stock
    When I click the "Add to Cart" button
    Then I should see a success message "Product added to cart"
    And the cart icon should show "1" item
    And the product should appear in my cart
```

---

## Exercise 4: Equivalence Partitioning

### Scenario

Age validation: Users must be 18-65 years old to register.

### Task

Identify equivalence partitions:

**Valid Partitions:**
- ________________________________

**Invalid Partitions:**
- ________________________________

**Test Values (one from each partition):**
- ________________________________

### Sample Answer

**Valid Partitions:**
- Age 18-65 (valid range)

**Invalid Partitions:**
- Age < 18 (too young)
- Age > 65 (too old)

**Test Values:**
- 25 (valid - middle of range)
- 17 (invalid - too young)
- 66 (invalid - too old)

---

## Exercise 5: Boundary Value Analysis

### Scenario

Password must be 8-20 characters long.

### Task

Identify boundary values to test:

**Lower Boundary:**
- ________________________________

**Upper Boundary:**
- ________________________________

**Test Values:**
- ________________________________

### Sample Answer

**Lower Boundary:** 7, 8, 9 characters  
**Upper Boundary:** 19, 20, 21 characters

**Test Values:**
- 7 characters (invalid - just below minimum)
- 8 characters (valid - minimum)
- 9 characters (valid - just above minimum)
- 19 characters (valid - just below maximum)
- 20 characters (valid - maximum)
- 21 characters (invalid - just above maximum)

---

## Exercise 6: Decision Table

### Scenario

Login system with:
- Email valid/invalid
- Password valid/invalid
- Remember Me checked/unchecked

### Task

Create a decision table:

| Email Valid | Password Valid | Remember Me | Expected Result |
|-------------|----------------|--------------|-----------------|
| Yes         | Yes            | Yes          | ?               |
| Yes         | Yes            | No           | ?               |
| Yes         | No             | Yes          | ?               |
| No          | Yes            | Yes          | ?               |
| No          | No             | Yes          | ?               |

### Sample Answer

| Email Valid | Password Valid | Remember Me | Expected Result |
|-------------|----------------|--------------|-----------------|
| Yes         | Yes            | Yes          | Login success, remember |
| Yes         | Yes            | No           | Login success, no remember |
| Yes         | No             | Yes          | Error: invalid password |
| No          | Yes            | Yes          | Error: invalid email |
| No          | No             | Yes          | Error: invalid credentials |

---

## Exercise 7: Complete Test Design

### Scenario

Feature: Product Search  
Requirements:
- User can search by product name
- Search is case-insensitive
- Results show product name, price, image
- If no results, show "No products found"
- Search supports pagination (20 per page)

### Task

Design test cases covering:
1. Positive scenario
2. Negative scenario
3. Edge cases
4. Boundary values

**Test Case 1 (Positive):**
- Name: ________________________________
- Steps: ________________________________
- Expected: ________________________________

**Test Case 2 (Negative):**
- Name: ________________________________
- Steps: ________________________________
- Expected: ________________________________

**Test Case 3 (Edge Case):**
- Name: ________________________________
- Steps: ________________________________
- Expected: ________________________________

### Sample Answer

**Test Case 1 (Positive):**
- Name: Search with valid product name
- Steps: Enter "laptop", click search
- Expected: Results display with laptops, showing name, price, image

**Test Case 2 (Negative):**
- Name: Search with no results
- Steps: Enter "nonexistentxyz", click search
- Expected: Message "No products found" displays

**Test Case 3 (Edge Case):**
- Name: Search with special characters
- Steps: Enter "laptop & tablet", click search
- Expected: Results display (special chars handled safely)

---

## Exercise 8: Gherkin Scenario Outline

### Scenario

Testing login with different invalid credentials.

### Task

Write a Scenario Outline:

```gherkin
Scenario Outline: Login with invalid credentials
  Given I am on the login page
  When I enter "<email>" in the email field
  And I enter "<password>" in the password field
  And I click the "Login" button
  Then I should see "<error_message>"
  
  Examples:
    | email              | password      | error_message                    |
    | ?                  | ?             | ?                                 |
    | ?                  | ?             | ?                                 |
    | ?                  | ?             | ?                                 |
```

### Sample Answer

```gherkin
Scenario Outline: Login with invalid credentials
  Given I am on the login page
  When I enter "<email>" in the email field
  And I enter "<password>" in the password field
  And I click the "Login" button
  Then I should see "<error_message>"
  
  Examples:
    | email              | password      | error_message                    |
    | ""                 | "Password123!" | "Email is required"              |
    | "user@example.com" | ""            | "Password is required"           |
    | "wrong@test.com"   | "WrongPass"   | "Invalid email or password"      |
```

---

## Exercise 9: Test Case Review

### Scenario

Here's a test case someone wrote. Review it and identify issues:

**Test Case:**
- **Name:** Test login
- **Steps:** Login with email and password
- **Expected:** It works

### Issues Found:

1. ________________________________
2. ________________________________
3. ________________________________
4. ________________________________

### Improved Version:

**Test Case Name:** ________________________________  
**Preconditions:** ________________________________  
**Test Steps:**
1. ________________________________
2. ________________________________
3. ________________________________

**Expected Result:** ________________________________

### Sample Answer

**Issues:**
1. Name is too vague ("Test login")
2. Steps are not detailed
3. Expected result is vague ("It works")
4. No preconditions
5. No test data specified

**Improved Version:**
- **Name:** User Login with Valid Credentials
- **Preconditions:** User account exists (testuser@example.com / Password123!)
- **Steps:**
  1. Navigate to /login
  2. Enter "testuser@example.com" in email field
  3. Enter "Password123!" in password field
  4. Click "Login" button
- **Expected:** User is redirected to /dashboard and sees welcome message

---

## Exercise 10: Self-Assessment

### Rate Your Understanding

- [ ] Writing test cases: _____
- [ ] Writing acceptance criteria: _____
- [ ] Writing Gherkin scenarios: _____
- [ ] Equivalence partitioning: _____
- [ ] Boundary value analysis: _____
- [ ] Decision tables: _____

### Practice Tasks

1. **Write 3 test cases** for a "Contact Form" feature
2. **Write acceptance criteria** for a "Newsletter Signup" feature
3. **Write a Gherkin scenario** for "User Registration"
4. **Create a decision table** for "File Upload" (valid file, invalid file, file too large, etc.)

---

## Reflection

1. **Which design technique do you find most useful?**
   ________________________________

2. **What's challenging about writing test cases?**
   ________________________________

3. **What questions do you have?**
   ________________________________

---

## Ready for Chapter 4?

Before moving to "Executing Tests," make sure you:
- [ ] Can write a complete test case
- [ ] Can write acceptance criteria
- [ ] Can write Gherkin scenarios
- [ ] Understand test design techniques

**If yes, proceed to Chapter 4!**

