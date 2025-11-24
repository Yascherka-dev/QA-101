# Gherkin Examples - Real-World Scenarios

## Example 1: Complete E-Commerce Checkout Flow

```gherkin
Feature: Checkout Process
  As a customer
  I want to complete a purchase
  So that I can receive the products I ordered
  
  Background:
    Given I am logged in as a registered user
    And I have items in my shopping cart
    And I am on the cart page
    
  Scenario: Successful checkout with credit card
    Given I have "Laptop" ($999) and "Mouse" ($29) in my cart
    When I click the "Checkout" button
    Then I should be redirected to the checkout page
    And I should see a summary of my order:
      | Item    | Quantity | Price  | Subtotal |
      | Laptop  | 1        | $999   | $999     |
      | Mouse   | 1        | $29    | $29      |
      | Total   |          |        | $1,028   |
    When I enter my shipping address:
      | Field           | Value                    |
      | Street Address  | 123 Main St              |
      | City            | New York                 |
      | State           | NY                       |
      | ZIP Code        | 10001                    |
    And I enter my payment information:
      | Field           | Value                    |
      | Card Number     | 4111111111111111         |
      | Expiry Date     | 12/25                    |
      | CVV             | 123                      |
      | Cardholder Name | John Doe                 |
    And I click the "Place Order" button
    Then I should see an order confirmation page
    And I should receive an order confirmation email
    And the order should appear in my order history
    And my cart should be empty
    
  Scenario: Checkout fails with invalid credit card
    Given I am on the checkout page with items in my cart
    When I enter invalid credit card number "1234567890123456"
    And I fill all other required fields with valid data
    And I click the "Place Order" button
    Then I should see an error message "Invalid credit card number"
    And I should remain on the checkout page
    And my order should not be placed
    
  Scenario: Checkout with empty shipping address
    Given I am on the checkout page with items in my cart
    When I leave the shipping address fields empty
    And I click the "Place Order" button
    Then I should see validation errors:
      | Field           | Error Message              |
      | Street Address  | Street address is required |
      | City            | City is required           |
      | State           | State is required          |
      | ZIP Code        | ZIP code is required       |
    And I should remain on the checkout page
    
  Scenario Outline: Checkout with different payment methods
    Given I am on the checkout page with items in my cart
    When I select "<payment_method>" as payment method
    And I enter valid payment details for "<payment_method>"
    And I click the "Place Order" button
    Then the order should be placed successfully
    And the payment method should be recorded as "<payment_method>"
    
    Examples:
      | payment_method |
      | Credit Card    |
      | PayPal         |
      | Apple Pay      |
      | Google Pay     |
```

## Example 2: User Profile Management

```gherkin
Feature: User Profile Management
  As a registered user
  I want to manage my profile information
  So that I can keep my account details up to date
  
  Background:
    Given I am logged in as a user
    And I am on my profile page
    
  Scenario: Update profile with valid information
    Given my current profile shows:
      | Field      | Current Value        |
      | First Name | John                 |
      | Last Name  | Doe                  |
      | Email      | john.doe@example.com |
      | Phone      | 555-0100             |
    When I click the "Edit Profile" button
    And I update the first name to "Jonathan"
    And I update the phone to "555-0200"
    And I click the "Save Changes" button
    Then I should see a success message "Profile updated successfully"
    And the profile should display:
      | Field      | Updated Value        |
      | First Name | Jonathan             |
      | Last Name  | Doe                  |
      | Email      | john.doe@example.com |
      | Phone      | 555-0200             |
    
  Scenario: Update profile fails with invalid email
    Given I am editing my profile
    When I change the email to "invalid-email-format"
    And I click the "Save Changes" button
    Then I should see an error message "Please enter a valid email address"
    And the email field should be highlighted in red
    And my profile should not be updated
    And I should remain on the edit profile page
    
  Scenario: Cancel profile update
    Given I am editing my profile
    When I change the first name to "Test Name"
    And I click the "Cancel" button
    Then I should see a confirmation dialog "Are you sure you want to cancel? Your changes will be lost."
    When I click "Yes, Cancel" in the dialog
    Then I should return to the profile view page
    And my profile should show the original values (not "Test Name")
    
  Scenario: Upload profile picture
    Given I am on my profile page
    When I click the "Change Photo" button
    And I select an image file "profile-photo.jpg" (size: 2MB, format: JPEG)
    And I click "Upload"
    Then I should see a success message "Profile picture updated"
    And the new profile picture should be displayed
    And the image should be cropped/resized to 200x200 pixels
    
  Scenario: Upload profile picture with invalid file
    Given I am on my profile page
    When I click the "Change Photo" button
    And I select a file "document.pdf" (not an image)
    And I click "Upload"
    Then I should see an error message "Please upload a valid image file (JPG, PNG, or GIF)"
    And my profile picture should remain unchanged
```

## Example 3: API Testing - User Management

```gherkin
Feature: User Management API
  As a system administrator
  I want to manage users via API
  So that I can automate user management tasks
  
  Background:
    Given the API base URL is "https://api.example.com/v1"
    And I have a valid admin API token
    
  Scenario: Create new user via API
    Given I send a POST request to "/users"
    And I include the following headers:
      | Header          | Value                    |
      | Authorization   | Bearer <admin_token>     |
      | Content-Type    | application/json         |
    And I include the following request body:
      """
      {
        "firstName": "Jane",
        "lastName": "Smith",
        "email": "jane.smith@example.com",
        "password": "SecurePass123!",
        "role": "user"
      }
      """
    When the request is sent
    Then the response status code should be 201
    And the response should contain a "user" object
    And the user object should have:
      | Field      | Value                    |
      | id         | a valid UUID             |
      | email      | jane.smith@example.com   |
      | firstName  | Jane                     |
      | lastName   | Smith                    |
      | role       | user                     |
    And the response should NOT contain the "password" field
    And the "createdAt" field should be a valid ISO 8601 timestamp
    
  Scenario: Create user fails with duplicate email
    Given a user with email "existing@example.com" already exists
    When I send a POST request to "/users" with email "existing@example.com"
    Then the response status code should be 409
    And the response should contain:
      """
      {
        "error": "User with this email already exists",
        "code": "DUPLICATE_EMAIL"
      }
      """
    
  Scenario: Get user by ID
    Given a user exists with ID "user-123"
    When I send a GET request to "/users/user-123"
    And I include the Authorization header with admin token
    Then the response status code should be 200
    And the response should contain the user details
    And the response time should be less than 500 milliseconds
    
  Scenario: Update user via API
    Given a user exists with ID "user-123"
    When I send a PATCH request to "/users/user-123"
    And I include the following request body:
      """
      {
        "firstName": "Updated Name",
        "phone": "555-9999"
      }
      """
    Then the response status code should be 200
    And the response should contain the updated user object
    And the "firstName" should be "Updated Name"
    And the "phone" should be "555-9999"
    And the "updatedAt" field should be updated
    
  Scenario: Delete user via API
    Given a user exists with ID "user-123"
    When I send a DELETE request to "/users/user-123"
    And I include the Authorization header with admin token
    Then the response status code should be 204
    When I send a GET request to "/users/user-123"
    Then the response status code should be 404
    And the response should contain "User not found"
    
  Scenario: API requires authentication
    Given I do not include an Authorization header
    When I send a GET request to "/users/user-123"
    Then the response status code should be 401
    And the response should contain:
      """
      {
        "error": "Unauthorized",
        "message": "Authentication required"
      }
      """
    
  Scenario Outline: API validates required fields
    Given I send a POST request to "/users"
    And I include a request body missing the "<field>" field
    When the request is sent
    Then the response status code should be 400
    And the response should contain an error message about "<field>" being required
    
    Examples:
      | field     |
      | firstName |
      | lastName  |
      | email     |
      | password  |
```

## Example 4: Search and Filtering

```gherkin
Feature: Product Search and Filtering
  As a customer
  I want to search and filter products
  So that I can find products that meet my criteria
  
  Background:
    Given I am on the products page
    And there are multiple products available
    
  Scenario: Search products by name
    Given I am on the products page
    When I enter "laptop" in the search field
    And I click the "Search" button
    Then I should see products containing "laptop" in the name
    And the results should display:
      | Product Name        | Price  | In Stock |
      | Dell Laptop Pro     | $999   | Yes      |
      | HP Laptop 15        | $799   | Yes      |
      | MacBook Pro Laptop  | $1,299 | Yes      |
    And I should see a message "3 products found"
    
  Scenario: Search with no results
    Given I am on the products page
    When I enter "nonexistentproductxyz" in the search field
    And I click the "Search" button
    Then I should see a message "No products found matching 'nonexistentproductxyz'"
    And I should see a "Clear Search" button
    And I should see a "Browse All Products" link
    
  Scenario: Filter products by price range
    Given I am on the products page
    When I set the price filter to "$500 - $1,000"
    And I click "Apply Filters"
    Then I should see only products priced between $500 and $1,000
    And each product price should be within the range
    And the filter should show as "Active"
    
  Scenario: Combine search and filters
    Given I am on the products page
    When I enter "laptop" in the search field
    And I select "Electronics" category filter
    And I set price range to "$800 - $1,200"
    And I click "Apply Filters"
    Then I should see laptops in Electronics category priced between $800 and $1,200
    And the search term "laptop" should remain in the search field
    And all active filters should be displayed
    
  Scenario: Clear all filters
    Given I have applied multiple filters:
      | Filter Type | Value           |
      | Category    | Electronics     |
      | Price Range | $500 - $1,000   |
      | Brand       | Dell            |
    When I click the "Clear All Filters" button
    Then all filters should be removed
    And I should see all products (unfiltered)
    And the filter count badge should disappear
    
  Scenario Outline: Search is case-insensitive
    Given I am on the products page
    When I enter "<search_term>" in the search field
    And I click the "Search" button
    Then I should see the same results as searching for "laptop"
    
    Examples:
      | search_term |
      | LAPTOP      |
      | Laptop      |
      | laptop      |
      | LaPtOp      |
```

## Example 5: Form Validation

```gherkin
Feature: Form Validation
  As a user
  I want forms to validate my input
  So that I can correct errors before submitting
  
  Background:
    Given I am on the registration page
    
  Scenario: Real-time email validation
    Given I am on the registration page
    When I enter "invalid-email" in the email field
    And I click outside the email field (blur event)
    Then I should see an error message "Please enter a valid email address"
    And the email field should have a red border
    When I correct the email to "valid@example.com"
    Then the error message should disappear
    And the email field should have a green border (indicating valid)
    
  Scenario: Password strength indicator
    Given I am on the registration page
    When I enter "weak" in the password field
    Then I should see a password strength indicator showing "Weak"
    And the indicator should be red
    When I enter "WeakPass1" in the password field
    Then the indicator should show "Medium" and be yellow
    When I enter "StrongPass123!" in the password field
    Then the indicator should show "Strong" and be green
    
  Scenario: Required field validation on submit
    Given I am on the registration page
    When I leave the "First Name" field empty
    And I leave the "Email" field empty
    And I click the "Register" button
    Then I should see validation errors:
      | Field      | Error Message          |
      | First Name | First name is required |
      | Email      | Email is required      |
    And I should remain on the registration page
    And the form should not be submitted
    
  Scenario Outline: Password validation rules
    Given I am on the registration page
    When I enter "<password>" in the password field
    And I click the "Register" button
    Then I should see "<error_message>"
    
    Examples:
      | password      | error_message                                    |
      | "short"       | "Password must be at least 8 characters"         |
      | "nouppercase" | "Password must contain at least one uppercase letter" |
      | "NOLOWERCASE" | "Password must contain at least one lowercase letter" |
      | "NoNumbers!"  | "Password must contain at least one number"       |
      | "NoSpecial1"  | "Password must contain at least one special character" |
      | "ValidPass123!" | No error (password is valid)                  |
    
  Scenario: Confirm password match validation
    Given I am on the registration page
    When I enter "Password123!" in the password field
    And I enter "DifferentPass123!" in the confirm password field
    And I click outside the confirm password field
    Then I should see an error message "Passwords do not match"
    When I change the confirm password to "Password123!"
    Then the error message should disappear
```

## Example 6: Responsive Design Testing

```gherkin
Feature: Responsive Design
  As a user
  I want the application to work on different screen sizes
  So that I can use it on any device
  
  Scenario: Mobile view - navigation menu
    Given I am viewing the application on a mobile device (375x667)
    When I load the homepage
    Then I should see a hamburger menu icon instead of the full navigation
    When I click the hamburger menu icon
    Then the navigation menu should slide in from the left
    And I should see all navigation links
    When I click outside the menu
    Then the menu should slide out and close
    
  Scenario: Tablet view - layout adaptation
    Given I am viewing the application on a tablet (768x1024)
    When I load the products page
    Then products should display in a 2-column grid
    And the sidebar filters should be visible
    And the layout should be optimized for tablet viewing
    
  Scenario: Desktop view - full features
    Given I am viewing the application on a desktop (1920x1080)
    When I load the products page
    Then products should display in a 4-column grid
    And all features should be visible without scrolling horizontally
    And hover effects should work on interactive elements
    
  Scenario Outline: Responsive breakpoints
    Given I am viewing the application at "<width>x<height>" resolution
    When I load the homepage
    Then the layout should match "<expected_layout>"
    
    Examples:
      | width | height | expected_layout        |
      | 320   | 568    | Mobile (single column) |
      | 768   | 1024   | Tablet (2 columns)      |
      | 1024  | 768    | Desktop (3 columns)    |
      | 1920  | 1080   | Desktop (4 columns)    |
```

These examples demonstrate real-world Gherkin scenarios covering various testing aspects from UI flows to API testing, form validation, and responsive design.

