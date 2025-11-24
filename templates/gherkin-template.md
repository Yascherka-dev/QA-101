# Gherkin Template & Guide

## What is Gherkin?

**Gherkin** is a plain-text language used to write executable specifications in Behavior-Driven Development (BDD). It uses natural language to describe software behavior.

**Benefits:**
- Readable by non-technical stakeholders
- Executable as automated tests
- Documents behavior clearly
- Bridges communication gap between business and technical teams

---

## Gherkin Syntax

### Keywords

| Keyword | Purpose | Example |
|---------|---------|---------|
| `Feature:` | Describes the feature being tested | `Feature: User Login` |
| `Scenario:` | Describes a test scenario | `Scenario: Successful login` |
| `Given:` | Sets up initial context/precondition | `Given I am on the login page` |
| `When:` | Describes the action/trigger | `When I click the login button` |
| `Then:` | Describes the expected outcome | `Then I should see the dashboard` |
| `And:` | Continues previous step (any keyword) | `And I enter my password` |
| `But:` | Negative continuation | `But I should not see errors` |
| `Background:` | Steps that run before each scenario | Common setup steps |
| `Scenario Outline:` | Template for multiple scenarios | Data-driven testing |
| `Examples:` | Test data for Scenario Outline | Table of values |

---

## Basic Structure

```gherkin
Feature: [Feature Name]
  [Brief description of the feature]
  
  [Optional: Background with common steps]
  
  Scenario: [Scenario Name]
    Given [precondition]
    When [action]
    Then [expected outcome]
    
  Scenario: [Another Scenario]
    Given [precondition]
    When [action]
    Then [expected outcome]
```

---

## Template

```gherkin
Feature: [Feature Name]
  As a [user type]
  I want to [action]
  So that [benefit]
  
  Background:
    Given [common precondition that applies to all scenarios]
    
  Scenario: [Positive Scenario Name]
    Given [initial context]
    When [action is performed]
    Then [expected outcome]
    And [additional verification]
    
  Scenario: [Negative Scenario Name]
    Given [initial context]
    When [action is performed]
    Then [error/negative outcome]
    But [what should not happen]
    
  Scenario Outline: [Parameterized Scenario Name]
    Given [precondition with <parameter>]
    When [action with <parameter>]
    Then [outcome with <parameter>]
    
    Examples:
      | parameter | expected_result |
      | value1    | result1         |
      | value2    | result2         |
```

---

## Examples

### Example 1: User Login

```gherkin
Feature: User Login
  As a registered user
  I want to log into the application
  So that I can access my account and personalized content
  
  Background:
    Given the application is accessible
    And I am not currently logged in
    
  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter "user@example.com" in the email field
    And I enter "Password123!" in the password field
    And I click the "Login" button
    Then I should be redirected to the dashboard
    And I should see the welcome message "Welcome, User"
    And I should see my name in the user menu
    
  Scenario: Login fails with invalid email
    Given I am on the login page
    When I enter "invalid@example.com" in the email field
    And I enter "Password123!" in the password field
    And I click the "Login" button
    Then I should see an error message "Invalid email or password"
    And I should remain on the login page
    But I should not be redirected
    
  Scenario: Login fails with invalid password
    Given I am on the login page
    When I enter "user@example.com" in the email field
    And I enter "WrongPassword" in the password field
    And I click the "Login" button
    Then I should see an error message "Invalid email or password"
    And the password field should be cleared
    
  Scenario: Login validation - empty fields
    Given I am on the login page
    When I click the "Login" button without entering any credentials
    Then I should see "Email is required" below the email field
    And I should see "Password is required" below the password field
    And I should remain on the login page
    
  Scenario: Remember me functionality
    Given I am on the login page
    When I enter "user@example.com" in the email field
    And I enter "Password123!" in the password field
    And I check the "Remember Me" checkbox
    And I click the "Login" button
    Then I should be logged in successfully
    When I log out and return to the login page
    Then the email field should be pre-filled with "user@example.com"
    
  Scenario Outline: Login with various invalid credentials
    Given I am on the login page
    When I enter "<email>" in the email field
    And I enter "<password>" in the password field
    And I click the "Login" button
    Then I should see "<error_message>"
    
    Examples:
      | email              | password      | error_message                    |
      | ""                 | "Password123!" | "Email is required"              |
      | "user@example.com" | ""            | "Password is required"           |
      | "invalid-email"    | "Password123!" | "Please enter a valid email"     |
      | "user@example.com" | "short"       | "Password must be at least 8 characters" |
```

### Example 2: User Registration

```gherkin
Feature: User Registration
  As a new user
  I want to create an account
  So that I can access the application
  
  Background:
    Given I am on the registration page
    And I have not previously registered
    
  Scenario: Successful registration with all required fields
    Given I am on the registration page
    When I enter "John" in the first name field
    And I enter "Doe" in the last name field
    And I enter "john.doe@example.com" in the email field
    And I enter "SecurePass123!" in the password field
    And I enter "SecurePass123!" in the confirm password field
    And I check the "I agree to Terms and Conditions" checkbox
    And I click the "Register" button
    Then I should see a success message "Account created successfully"
    And I should receive a verification email at "john.doe@example.com"
    And I should be redirected to the login page
    
  Scenario: Registration fails with duplicate email
    Given a user with email "existing@example.com" already exists
    When I enter "existing@example.com" in the email field
    And I fill all other required fields with valid data
    And I click the "Register" button
    Then I should see an error message "An account with this email already exists"
    And I should remain on the registration page
    
  Scenario: Registration validation - weak password
    Given I am on the registration page
    When I enter "weak" in the password field
    And I fill all other required fields
    And I click the "Register" button
    Then I should see a password validation message
    And the message should indicate password requirements:
      | Requirement                    |
      | At least 8 characters          |
      | One uppercase letter           |
      | One lowercase letter           |
      | One number                     |
      | One special character          |
    
  Scenario Outline: Registration with invalid email formats
    Given I am on the registration page
    When I enter "<email>" in the email field
    And I fill all other required fields with valid data
    And I click the "Register" button
    Then I should see "Please enter a valid email address"
    
    Examples:
      | email                  |
      | "notanemail"           |
      | "missing@domain"       |
      | "@nodomain.com"        |
      | "spaces in@email.com"  |
      | "missingdot@com"       |
```

### Example 3: Product Search (API Testing)

```gherkin
Feature: Product Search API
  As an application user
  I want to search for products via API
  So that I can find products programmatically
  
  Background:
    Given the API is accessible at "https://api.example.com"
    And I have a valid API token
    
  Scenario: Search products with valid query
    Given I send a GET request to "/api/products/search"
    And I include query parameter "q" with value "laptop"
    When the request is sent
    Then the response status code should be 200
    And the response should contain a "products" array
    And each product should have "id", "name", "price", and "image" fields
    And the response should include a "total" field with the count
    
  Scenario: Search returns empty results
    Given I send a GET request to "/api/products/search"
    And I include query parameter "q" with value "nonexistentproductxyz"
    When the request is sent
    Then the response status code should be 200
    And the "products" array should be empty
    And the "total" field should be 0
    
  Scenario: Search with pagination
    Given I send a GET request to "/api/products/search"
    And I include query parameter "q" with value "laptop"
    And I include query parameter "page" with value "2"
    And I include query parameter "limit" with value "10"
    When the request is sent
    Then the response status code should be 200
    And the response should contain "page" field with value 2
    And the response should contain "limit" field with value 10
    And the "products" array should have at most 10 items
    
  Scenario: Search API requires authentication
    Given I send a GET request to "/api/products/search" without authentication token
    When the request is sent
    Then the response status code should be 401
    And the response should contain error message "Unauthorized"
    
  Scenario Outline: Search with special characters
    Given I send a GET request to "/api/products/search"
    And I include query parameter "q" with value "<search_term>"
    When the request is sent
    Then the response status code should be 200
    And the API should handle the search safely without errors
    
    Examples:
      | search_term        |
      | "laptop & tablet"  |
      | "product's name"   |
      | "item@special"     |
      | "test<script>"     |
```

### Example 4: Shopping Cart

```gherkin
Feature: Shopping Cart
  As a customer
  I want to manage items in my shopping cart
  So that I can purchase multiple products together
  
  Background:
    Given I am logged in as a user
    And I am on the products page
    
  Scenario: Add product to cart
    Given I am viewing a product with name "Laptop" and price "$999"
    When I click the "Add to Cart" button
    Then I should see a confirmation message "Laptop added to cart"
    And the cart icon should show "1" item
    When I navigate to the cart page
    Then I should see "Laptop" in the cart
    And the total price should be "$999"
    
  Scenario: Remove product from cart
    Given I have "Laptop" in my cart
    When I navigate to the cart page
    And I click the "Remove" button for "Laptop"
    Then I should see a confirmation message "Laptop removed from cart"
    And the cart should be empty
    And the cart icon should show "0" items
    
  Scenario: Update product quantity in cart
    Given I have "Laptop" in my cart with quantity 1
    When I navigate to the cart page
    And I change the quantity to "3"
    Then the quantity should update to 3
    And the total price should be "$2,997" (3 × $999)
    
  Scenario: Cart persists across sessions
    Given I have added "Laptop" to my cart
    When I log out
    And I log back in
    Then "Laptop" should still be in my cart
```

---

## Best Practices

### ✅ DO:

- Write scenarios from user's perspective
- Use clear, natural language
- Keep scenarios focused (one scenario = one behavior)
- Use Background for common setup
- Use Scenario Outline for data-driven tests
- Make steps reusable
- Use specific values when it matters, variables when it doesn't
- Keep feature files organized by feature

### ❌ DON'T:

- Write technical implementation details
- Write scenarios that are too long (keep under 10 steps)
- Use vague language ("it should work")
- Mix multiple behaviors in one scenario
- Write scenarios that depend on each other
- Use technical jargon users don't understand

---

## Step Definitions (For Automation)

When automating Gherkin scenarios, you need step definitions. Here's how they map:

**Gherkin Step:**
```gherkin
Given I am on the login page
```

**Cypress Step Definition:**
```javascript
Given('I am on the login page', () => {
  cy.visit('/login');
});
```

**Playwright Step Definition:**
```javascript
Given('I am on the login page', async ({ page }) => {
  await page.goto('/login');
});
```

---

## Checklist for Writing Gherkin

- [ ] Feature description is clear
- [ ] Scenarios are written from user perspective
- [ ] Each scenario tests one behavior
- [ ] Steps are clear and actionable
- [ ] Positive and negative scenarios included
- [ ] Edge cases covered
- [ ] Background used for common setup
- [ ] Scenario Outline used for data-driven tests
- [ ] Language is natural and readable
- [ ] Scenarios are independent (no dependencies)

