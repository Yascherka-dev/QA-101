# Complete QA Learning Roadmap
## From Junior Developer to QA Lead

**Your Journey:** Junior Web Developer → QA Lead  
**Tech Stack:** Angular (Frontend) + REST API + Azure DevOps  
**Approach:** Progressive, hands-on, real-world focused

---

## 📚 Table of Contents

1. [QA Foundations](#1-qa-foundations)
2. [Types of Tests](#2-types-of-tests)
3. [Designing Tests](#3-designing-tests)
4. [Executing Tests](#4-executing-tests)
5. [QA Tools - Azure DevOps](#5-qa-tools---azure-devops)
6. [Advanced Topics](#6-advanced-topics)
7. [Building Your QA Strategy](#7-building-your-qa-strategy)

---

## 1. QA Foundations

### 1.1 What is QA?

**Short Answer:** QA (Quality Assurance) ensures software meets requirements, works correctly, and provides value to users.

**Long Answer:** QA is a systematic process of:
- **Preventing defects** before they reach production
- **Finding defects** through testing
- **Ensuring quality** throughout the development lifecycle
- **Validating** that software meets business and user needs

### 1.2 QA vs Testing

| QA (Quality Assurance) | Testing |
|------------------------|---------|
| Process-oriented | Activity-oriented |
| Preventive (build quality in) | Detective (find defects) |
| Throughout SDLC | Specific phase |
| Process improvement | Defect identification |

**Key Insight:** Testing is a subset of QA. QA includes testing, but also process, standards, and continuous improvement.

### 1.3 QA Role & Responsibilities

**Core Responsibilities:**
1. **Test Planning** - Create test strategies and plans
2. **Test Design** - Write test cases and scenarios
3. **Test Execution** - Run tests manually and automated
4. **Defect Management** - Report, track, and verify bug fixes
5. **Quality Metrics** - Track test coverage, defect rates, etc.
6. **Collaboration** - Work with devs, BAs, PMs, stakeholders
7. **Process Improvement** - Suggest improvements to workflows

**Daily Activities:**
- Review user stories and acceptance criteria
- Design test cases for new features
- Execute regression tests
- Report and verify bugs
- Update test documentation
- Attend standups, planning, retrospectives

### 1.4 SDLC (Software Development Lifecycle)

**Traditional Waterfall:**
```
Requirements → Design → Development → Testing → Deployment
```

**Agile/Scrum (Your Context):**
```
Sprint Planning → Development → Testing (parallel) → Review → Retrospective
```

**QA Involvement at Each Stage:**
- **Planning:** Review stories, clarify requirements, estimate testing effort
- **Development:** Review code, test early builds, provide feedback
- **Testing:** Execute test cases, exploratory testing, regression
- **Review:** Demo features, validate acceptance criteria
- **Retrospective:** Share quality insights, suggest improvements

### 1.5 QA in Scrum

**Sprint Activities:**

**Sprint Planning:**
- Review user stories
- Identify test scenarios
- Estimate testing effort
- Ask clarifying questions

**During Sprint:**
- Test features as they're developed (shift-left testing)
- Daily standups: report blockers, test progress
- Continuous testing, not just at the end

**Sprint Review:**
- Demo tested features
- Validate acceptance criteria met
- Gather stakeholder feedback

**Sprint Retrospective:**
- Discuss what went well/poorly
- Identify process improvements
- Share quality metrics

**Key Principle:** Test early, test often. Don't wait until the end of the sprint.

---

## 2. Types of Tests

### 2.1 Functional Testing

**Definition:** Testing that the software functions correctly according to requirements.

#### Unit Testing
- **What:** Testing individual components/functions in isolation
- **Who:** Developers (but QA reviews)
- **Example:** Testing a login validation function
- **Tool:** Jasmine, Jest (for Angular)

**Example Scenario:**
```typescript
// Unit test example
describe('LoginService', () => {
  it('should reject empty email', () => {
    expect(loginService.validateEmail('')).toBe(false);
  });
});
```

#### Integration Testing
- **What:** Testing how components work together
- **Example:** Testing API + Database, Frontend + Backend
- **Tool:** Postman (API), Protractor/Cypress (E2E)

**Example Scenario:**
- Test that Angular app correctly calls REST API
- Verify API returns data, frontend displays it

#### End-to-End (E2E) Testing
- **What:** Testing complete user workflows
- **Example:** User logs in → navigates → performs action → logs out
- **Tool:** Cypress, Playwright, Protractor

**Example Scenario:**
```
1. User opens app
2. User logs in with valid credentials
3. User navigates to "Products" page
4. User adds product to cart
5. User checks out
6. User receives confirmation
```

#### System Testing
- **What:** Testing the complete system as a whole
- **Scope:** All integrated components
- **Focus:** System meets requirements

#### User Acceptance Testing (UAT)
- **What:** Testing by end-users/stakeholders
- **Purpose:** Validate software meets business needs
- **When:** Before production release
- **Who:** Business users, product owners

#### Exploratory Testing
- **What:** Unscripted, ad-hoc testing
- **Approach:** Learn → Design → Execute → Report (simultaneously)
- **When:** After scripted tests, for edge cases
- **Skills:** Critical thinking, creativity

**Technique:**
1. Start with a mission (e.g., "Test checkout flow")
2. Explore freely, take notes
3. Report interesting findings
4. Follow hunches and edge cases

### 2.2 Non-Functional Testing

#### Performance Testing
- **What:** Testing speed, responsiveness, stability
- **Types:**
  - **Load Testing:** Normal expected load
  - **Stress Testing:** Beyond normal capacity
  - **Volume Testing:** Large amounts of data
- **Tool:** JMeter, K6, Lighthouse

**Example Metrics:**
- Page load time < 2 seconds
- API response time < 500ms
- Supports 100 concurrent users

#### Security Testing
- **What:** Testing for vulnerabilities
- **Focus:** Authentication, authorization, data protection
- **Example Checks:**
  - SQL injection
  - XSS (Cross-Site Scripting)
  - Authentication bypass
  - Sensitive data exposure

**Example Scenario:**
- Try to access admin page without login
- Test SQL injection in search field
- Verify passwords are encrypted

#### Accessibility Testing
- **What:** Testing for users with disabilities
- **Standards:** WCAG 2.1 (A, AA, AAA levels)
- **Focus:** Screen readers, keyboard navigation, color contrast
- **Tool:** axe DevTools, WAVE, Lighthouse

**Example Checks:**
- All images have alt text
- Forms have labels
- Keyboard navigation works
- Color contrast meets standards

#### Usability Testing
- **What:** Testing user experience
- **Focus:** Ease of use, intuitiveness
- **Method:** User observation, feedback

#### Compatibility Testing
- **What:** Testing across browsers, devices, OS
- **Example:** Chrome, Firefox, Safari, Edge
- **Mobile:** iOS, Android, different screen sizes

---

## 3. Designing Tests

### 3.1 Test Case Structure

**Standard Test Case Template:**
```
Test Case ID: TC-001
Test Case Name: User Login with Valid Credentials
Module: Authentication
Priority: High
Preconditions: User account exists, app is accessible
Test Steps:
  1. Navigate to login page
  2. Enter valid email
  3. Enter valid password
  4. Click "Login" button
Expected Result: User is logged in, redirected to dashboard
Actual Result: [Fill during execution]
Status: Pass/Fail/Blocked
```

**See:** `templates/test-case-template.md` for full template

### 3.2 Acceptance Criteria

**What:** Conditions that must be met for a feature to be considered "done"

**Format (Given-When-Then):**
```
Given: [Initial context]
When: [Action performed]
Then: [Expected outcome]
```

**Example:**
```
Given: User is on the login page
When: User enters valid credentials and clicks "Login"
Then: User is redirected to dashboard and sees welcome message
```

**See:** `templates/acceptance-criteria-template.md`

### 3.3 BDD / Gherkin

**BDD (Behavior-Driven Development):** Writing tests in natural language

**Gherkin Syntax:**
```gherkin
Feature: User Login
  As a user
  I want to log into the application
  So that I can access my account

  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter "user@example.com" in the email field
    And I enter "password123" in the password field
    And I click the "Login" button
    Then I should be redirected to the dashboard
    And I should see the welcome message "Welcome, User"

  Scenario: Login fails with invalid credentials
    Given I am on the login page
    When I enter "wrong@example.com" in the email field
    And I enter "wrongpassword" in the password field
    And I click the "Login" button
    Then I should see an error message "Invalid credentials"
    And I should remain on the login page
```

**Keywords:**
- `Feature:` - High-level description
- `Scenario:` - Test scenario
- `Given:` - Precondition/initial state
- `When:` - Action/trigger
- `Then:` - Expected outcome
- `And:` - Additional step (same keyword)
- `Background:` - Common steps for all scenarios

**See:** `templates/gherkin-template.md` and `examples/gherkin-examples.md`

### 3.4 Test Design Techniques

#### Equivalence Partitioning
**Concept:** Group inputs that should behave the same way

**Example - Age Validation:**
- Valid: 18-65 (one partition)
- Invalid: < 18 (one partition)
- Invalid: > 65 (one partition)

Test one value from each partition.

#### Boundary Value Analysis
**Concept:** Test values at boundaries (edges)

**Example - Age 18-65:**
- Test: 17, 18, 19 (lower boundary)
- Test: 64, 65, 66 (upper boundary)

**Why:** Bugs often occur at boundaries.

#### Decision Tables
**Concept:** Test all combinations of conditions

**Example - Login:**
| Email Valid | Password Valid | Remember Me | Expected Result |
|------------|----------------|-------------|-----------------|
| Yes | Yes | Yes | Login success, remember |
| Yes | Yes | No | Login success, no remember |
| Yes | No | Yes | Error: invalid password |
| No | Yes | Yes | Error: invalid email |
| No | No | Yes | Error: invalid credentials |

#### State Transition Testing
**Concept:** Test transitions between states

**Example - User Account:**
```
New → Activated → Suspended → Activated → Deleted
```

Test each transition.

#### Error Guessing
**Concept:** Use experience to guess where errors might occur
- Empty fields
- Special characters
- Very long inputs
- Negative numbers where positive expected

---

## 4. Executing Tests

### 4.1 Manual Test Workflow

**Step-by-Step Process:**

1. **Prepare:**
   - Review test cases
   - Set up test environment
   - Gather test data
   - Check preconditions

2. **Execute:**
   - Follow test steps exactly
   - Document actual results
   - Take screenshots/videos if needed
   - Note any observations

3. **Report:**
   - Mark test case as Pass/Fail/Blocked
   - Log defects for failures
   - Update test documentation

4. **Verify:**
   - Re-test fixed bugs
   - Update test status

### 4.2 Exploratory Testing Approach

**Session-Based Testing:**

1. **Charter:** Define mission (e.g., "Explore checkout flow")
2. **Time-box:** Set time limit (e.g., 60 minutes)
3. **Explore:** Test freely, follow hunches
4. **Document:** Take notes, screenshots
5. **Debrief:** Report findings

**Heuristics:**
- **SFDPO:** Structure, Function, Data, Platform, Operations
- **CRUD:** Create, Read, Update, Delete
- **CRUSSPIC STMPL:** Capability, Reliability, Usability, Security, Scalability, Performance, Installability, Compatibility, Supportability, Testability, Maintainability, Portability, Localizability

### 4.3 Writing Bug Reports

**Essential Elements:**

1. **Title:** Clear, concise summary
2. **Description:** What happened
3. **Steps to Reproduce:** Detailed, numbered steps
4. **Expected Result:** What should happen
5. **Actual Result:** What actually happened
6. **Environment:** Browser, OS, version
7. **Severity:** Impact on system
8. **Priority:** Urgency to fix
9. **Screenshots/Videos:** Visual evidence
10. **Attachments:** Logs, test data

**See:** `templates/bug-report-template.md` and `examples/bug-report-examples.md`

### 4.4 Severity vs Priority

**Severity:** Impact of the bug on the system/functionality

**Levels:**
- **Critical:** System crash, data loss, security breach
- **High:** Major feature broken, workaround exists
- **Medium:** Feature partially broken, minor impact
- **Low:** Cosmetic, minor inconvenience

**Priority:** Urgency to fix the bug

**Levels:**
- **P1 (Critical):** Fix immediately
- **P2 (High):** Fix in current sprint
- **P3 (Medium):** Fix in next sprint
- **P4 (Low):** Fix when time permits

**Examples:**
- **High Severity, High Priority:** Login doesn't work (blocks all users)
- **High Severity, Low Priority:** Print functionality broken (rarely used)
- **Low Severity, High Priority:** Company logo misspelled (branding issue)

---

## 5. QA Tools - Azure DevOps

### 5.1 Azure DevOps Test Plans

**Overview:** Azure DevOps Test Plans provides test case management, test execution, and reporting.

#### Creating Test Plans

1. **Navigate:** Azure DevOps → Test Plans
2. **Create Plan:**
   - Name: "Sprint 5 - User Management"
   - Description: Test plan for user management features
   - Area Path: Select project area
   - Iteration: Select sprint

#### Creating Test Suites

**Types:**
- **Static Suite:** Manual selection of test cases
- **Requirement-based Suite:** Linked to user stories
- **Query-based Suite:** Dynamic based on query

**Best Practice:** Organize by feature/module

#### Creating Test Cases

1. **Add Test Case:**
   - Title: Clear, descriptive
   - Steps: Detailed actions
   - Expected Results: What should happen
   - Attachments: Screenshots, documents

2. **Link to Work Items:**
   - Link to user story
   - Link to bug (if regression)

#### Executing Tests

1. **Run Tests:**
   - Open test suite
   - Click "Run" for test case
   - Mark steps as Pass/Fail
   - Add comments
   - Attach screenshots

2. **Test Results:**
   - View execution history
   - Track pass/fail rates
   - Generate reports

### 5.2 Azure DevOps Boards

#### Linking Work Items

**Types of Links:**
- **Tested By:** Test case tests a user story
- **Tests:** User story is tested by test case
- **Related To:** General relationship
- **Duplicate Of:** Bug is duplicate
- **Child Of:** Hierarchical relationship

**Workflow:**
1. Create user story
2. Create test cases
3. Link test cases to story (Tests link)
4. Execute tests
5. Create bugs if found
6. Link bugs to story (Related To)

#### Tracking Progress

**Queries:**
- Active bugs
- Test cases by status
- Test execution progress
- Defect trends

**Dashboards:**
- Test execution status
- Bug count by severity
- Test coverage

### 5.3 Best Practices

1. **Organize:** Use test suites by feature/module
2. **Link:** Always link test cases to user stories
3. **Document:** Add detailed steps and expected results
4. **Track:** Update test status regularly
5. **Report:** Use built-in reports for stakeholders

**See:** `guides/azure-devops-workflow.md` for detailed walkthrough

---

## 6. Advanced Topics

### 6.1 API Testing with Postman

**Why Test APIs:**
- APIs are the backbone of modern apps
- Test backend independently of frontend
- Faster than UI testing
- Can test edge cases easily

#### Postman Basics

**Creating Requests:**
1. **Method:** GET, POST, PUT, DELETE, etc.
2. **URL:** API endpoint
3. **Headers:** Authorization, Content-Type
4. **Body:** Request payload (JSON, XML, etc.)

**Example - Login API:**
```
POST https://api.example.com/auth/login
Headers:
  Content-Type: application/json
Body:
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Assertions:**
- Status code: 200, 201, 400, 401, etc.
- Response time: < 500ms
- Response body: Contains expected data
- Headers: Correct content-type

**Collections:**
- Organize requests by feature
- Use variables for environments
- Run collections as test suites

**See:** `examples/postman-api-examples.md` for detailed examples

### 6.2 Automation Introduction

**Why Automate:**
- Faster execution
- Repeatable
- Catches regressions
- Frees time for exploratory testing

**What to Automate:**
- ✅ Regression tests
- ✅ Smoke tests
- ✅ High-value, frequently run tests
- ❌ One-time tests
- ❌ Frequently changing features
- ❌ Exploratory tests

#### Cypress Basics

**Installation:**
```bash
npm install cypress --save-dev
```

**Example Test:**
```javascript
describe('Login Flow', () => {
  it('should login successfully', () => {
    cy.visit('/login');
    cy.get('[data-cy=email]').type('user@example.com');
    cy.get('[data-cy=password]').type('password123');
    cy.get('[data-cy=login-button]').click();
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome, User');
  });
});
```

**Key Concepts:**
- **Selectors:** Use data-cy attributes (best practice)
- **Commands:** cy.visit(), cy.get(), cy.click(), etc.
- **Assertions:** .should(), .expect()
- **Fixtures:** Test data management

#### Playwright Basics

**Installation:**
```bash
npm install playwright
npx playwright install
```

**Example Test:**
```javascript
test('login flow', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[data-cy=email]', 'user@example.com');
  await page.fill('[data-cy=password]', 'password123');
  await page.click('[data-cy=login-button]');
  await expect(page).toHaveURL(/.*dashboard/);
  await expect(page.locator('text=Welcome, User')).toBeVisible();
});
```

**Key Features:**
- Multi-browser support
- Auto-waiting
- Network interception
- Screenshot/video on failure

**See:** `examples/automation-examples.md` for more examples

---

## 7. Building Your QA Strategy

### 7.1 Test Pyramid

**Concept:** More unit tests, fewer E2E tests

```
        /\
       /  \     E2E Tests (Few)
      /____\
     /      \   Integration Tests (Some)
    /________\
   /          \  Unit Tests (Many)
  /____________\
```

**Why:**
- Unit tests: Fast, cheap, many
- Integration tests: Moderate speed, moderate cost
- E2E tests: Slow, expensive, fewer

### 7.2 Test Strategy for a Sprint

**Week 1 (Planning + Early Development):**
- Review user stories
- Write test cases
- Set up test data
- Clarify requirements

**Week 2 (Development + Testing):**
- Test features as developed (shift-left)
- Execute test cases
- Exploratory testing
- Report bugs early

**Week 3 (Testing + Bug Fixes):**
- Regression testing
- Re-test fixed bugs
- Final exploratory testing
- Prepare for demo

**Week 4 (Stabilization):**
- Final regression
- UAT support
- Documentation updates
- Retrospective prep

### 7.3 Quality Metrics

**Track:**
- Test coverage: % of requirements covered
- Defect density: Bugs per feature
- Defect leakage: Bugs found in production
- Test execution rate: Tests run vs planned
- Bug fix rate: Time to fix bugs

**Use Metrics To:**
- Identify problem areas
- Improve processes
- Communicate status
- Make data-driven decisions

### 7.4 Continuous Improvement

**Regular Activities:**
- Retrospectives: What went well/poorly?
- Root cause analysis: Why did bugs escape?
- Process refinement: How can we improve?
- Skill development: Learn new techniques

---

## 🎯 Next Steps

1. **Review this roadmap** - Understand the full picture
2. **Start with Chapter 1** - QA Foundations
3. **Use templates** - Begin creating test cases
4. **Practice** - Do the exercises
5. **Ask questions** - When you need clarification

**Ready to begin?** Let's start with Chapter 1 exercises, or dive deeper into any section!

---

## 📝 Quick Reference

- **Templates:** `templates/` directory
- **Examples:** `examples/` directory
- **Exercises:** `exercises/` directory
- **Guides:** `guides/` directory

