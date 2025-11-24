# Chapter 2 Exercises: Types of Tests

## Exercise 1: Test Type Identification

### Scenario 1

You're testing a login function that validates email format. You write a test that checks if `validateEmail("test@example.com")` returns `true`.

**What type of test is this?**
- [ ] Integration Test
- [ ] Unit Test
- [ ] E2E Test
- [ ] System Test

**Answer:** Unit Test - Testing a single function in isolation

---

### Scenario 2

You're testing that when a user clicks "Add to Cart" on the product page, the product appears in the cart, and the cart icon updates.

**What type of test is this?**
- [ ] Unit Test
- [ ] Integration Test
- [ ] E2E Test
- [ ] Performance Test

**Answer:** E2E Test - Testing complete user workflow across multiple components

---

### Scenario 3

You're testing that the Angular frontend correctly calls the REST API and displays the returned data.

**What type of test is this?**
- [ ] Unit Test
- [ ] Integration Test
- [ ] E2E Test
- [ ] Security Test

**Answer:** Integration Test - Testing how frontend and backend work together

---

## Exercise 2: Functional vs Non-Functional

### Categorize These Tests

Mark each as **Functional (F)** or **Non-Functional (NF)**:

1. [ ] Testing that login works with valid credentials
2. [ ] Testing that page loads in under 2 seconds
3. [ ] Testing that search returns correct results
4. [ ] Testing that the app works on mobile devices
5. [ ] Testing that users can't access admin pages without permission
6. [ ] Testing that forms are accessible via keyboard
7. [ ] Testing that payment processing works correctly
8. [ ] Testing that the app handles 1000 concurrent users

### Answers

1. **F** - Functional (testing feature works)
2. **NF** - Non-Functional (performance)
3. **F** - Functional (testing feature works)
4. **NF** - Non-Functional (compatibility)
5. **NF** - Non-Functional (security)
6. **NF** - Non-Functional (accessibility)
7. **F** - Functional (testing feature works)
8. **NF** - Non-Functional (performance/load)

---

## Exercise 3: Exploratory Testing

### Scenario

You're doing exploratory testing on a "Product Search" feature.

### Task

Create an exploratory testing charter:

**Charter:** Explore the product search feature to find usability issues and edge cases.

**Time-box:** 30 minutes

**Test Ideas:**
1. ________________________________
2. ________________________________
3. ________________________________
4. ________________________________
5. ________________________________

### Sample Ideas

1. Search with empty string
2. Search with special characters
3. Search with very long query
4. Search and then clear search
5. Search with SQL injection attempt
6. Search on mobile view
7. Search with filters applied
8. Search and sort results

---

## Exercise 4: Test Pyramid

### Question

You have 100 test cases to write. According to the test pyramid, how should you distribute them?

**Unit Tests:** _____ (should be most)  
**Integration Tests:** _____ (should be some)  
**E2E Tests:** _____ (should be few)

### Sample Answer

**Unit Tests:** 70 (70%)  
**Integration Tests:** 20 (20%)  
**E2E Tests:** 10 (10%)

**Why?**
- Unit tests are fast, cheap, many
- Integration tests are moderate
- E2E tests are slow, expensive, fewer

---

## Exercise 5: Real-World Scenario

### Scenario

You're testing a new "Checkout" feature for an e-commerce app.

### Task

List which types of tests you would perform:

**Functional Tests:**
1. ________________________________
2. ________________________________
3. ________________________________

**Non-Functional Tests:**
1. ________________________________
2. ________________________________
3. ________________________________

### Sample Answer

**Functional Tests:**
1. User can add items to cart and checkout
2. Payment processing works correctly
3. Order confirmation is sent

**Non-Functional Tests:**
1. Checkout completes in < 3 seconds (Performance)
2. Payment data is encrypted (Security)
3. Checkout works on mobile devices (Compatibility)
4. Forms are accessible (Accessibility)

---

## Exercise 6: UAT Scenario

### Scenario

A new "Report Generation" feature is ready. The product owner wants to do UAT.

### Questions

1. **Who should perform UAT?**
   - [ ] Developers
   - [ ] QA team
   - [ ] End users / Product Owner
   - [ ] All of the above

2. **What is the purpose of UAT?**
   - [ ] Find bugs
   - [ ] Validate it meets business needs
   - [ ] Test performance
   - [ ] Write automation

3. **When should UAT happen?**
   - [ ] Before any testing
   - [ ] After QA testing is complete
   - [ ] During development
   - [ ] After production release

### Answers

1. **C** - End users / Product Owner (business stakeholders)
2. **B** - Validate it meets business needs
3. **B** - After QA testing is complete, before production

---

## Exercise 7: Test Type Selection

### Scenario

You need to test a "User Registration" feature. Which test types would you use?

### Task

Check all that apply:

- [ ] Unit Test: Test email validation function
- [ ] Integration Test: Test API + Database
- [ ] E2E Test: Complete registration flow
- [ ] Performance Test: Registration under load
- [ ] Security Test: SQL injection, XSS
- [ ] Accessibility Test: Keyboard navigation, screen reader
- [ ] Compatibility Test: Different browsers
- [ ] Usability Test: User can complete registration easily

**Answer:** All of them! Different test types cover different aspects.

---

## Exercise 8: Matching Test Types to Tools

### Task

Match the test type to the appropriate tool:

1. Unit Test → [ ] Cypress, [ ] Jest, [ ] Postman
2. E2E Test → [ ] Jest, [ ] Cypress, [ ] JMeter
3. API Test → [ ] Cypress, [ ] Postman, [ ] Jest
4. Performance Test → [ ] Postman, [ ] JMeter, [ ] Jest

### Answers

1. **Jest** - Unit testing framework
2. **Cypress** - E2E testing tool
3. **Postman** - API testing tool
4. **JMeter** - Performance testing tool

---

## Exercise 9: Create a Test Strategy

### Scenario

You're planning testing for a "Password Reset" feature.

### Task

Create a test strategy covering different test types:

**Unit Tests:**
- Test password validation function
- Test email format validation

**Integration Tests:**
- Test API endpoint receives reset request
- Test email service sends reset email

**E2E Tests:**
- User requests reset → receives email → resets password → logs in

**Security Tests:**
- Can't reset password without valid token
- Token expires after time limit
- Rate limiting on reset requests

**Performance Tests:**
- Reset email sent within 30 seconds
- API responds in < 500ms

---

## Exercise 10: Self-Assessment

### Rate Your Understanding

- [ ] Unit vs Integration vs E2E: _____
- [ ] Functional vs Non-Functional: _____
- [ ] Exploratory testing approach: _____
- [ ] Test pyramid concept: _____
- [ ] When to use each test type: _____

### Next Steps

- **All 4-5:** Great! Move to Chapter 3
- **Some 3 or below:** Review those sections
- **Many 1-2:** Re-read Chapter 2

---

## Practice Exercise: Test Type Decision

### Scenario

Your team is building a "Product Review" feature where users can:
- Write reviews
- Rate products (1-5 stars)
- See average rating

### Task

For each aspect, decide the test type:

1. **Rating calculation (average of all ratings):**
   - Test Type: _______________
   - Why: _______________

2. **User can submit a review:**
   - Test Type: _______________
   - Why: _______________

3. **Review submission is fast (< 1 second):**
   - Test Type: _______________
   - Why: _______________

4. **Reviews display correctly on mobile:**
   - Test Type: _______________
   - Why: _______________

---

## Reflection

1. **Which test type do you find most interesting?**
   ________________________________

2. **Which test type seems most challenging?**
   ________________________________

3. **What questions do you have about test types?**
   ________________________________

---

## Ready for Chapter 3?

Before moving to "Designing Tests," make sure you:
- [ ] Can identify different test types
- [ ] Understand functional vs non-functional
- [ ] Know when to use each test type
- [ ] Understand the test pyramid

**If yes, proceed to Chapter 3!**

