# Test Plan Template
## ISTQB Compliant

**ISTQB Standard:** This template follows ISTQB Foundation Level standards for test planning documentation (IEEE 829 standard alignment).

## Document Information

**Test Plan ID:** `TP-XXX-XXX`  
**Test Plan Name:** [e.g., Sprint 5 - User Management Features]  
**Project:** [Project name]  
**Version:** [1.0]  
**Created By:** [Your name]  
**Created Date:** [Date]  
**Last Updated:** [Date]  
**Approved By:** [QA Lead / Manager]  
**Approval Date:** [Date]

## 1. Introduction

### 1.1 Purpose
[Purpose of this test plan - what will be tested and why]

**Example:**
This test plan outlines the testing strategy for User Management features in Sprint 5, including user registration, profile management, and account settings.

### 1.2 Scope
**In Scope:**
- User registration flow
- User profile update
- Password change functionality
- Account deletion

**Out of Scope:**
- Email verification (covered in separate plan)
- Third-party integrations
- Performance testing (separate sprint)

### 1.3 Objectives
- Verify all user management features work as specified
- Ensure no regression in existing functionality
- Validate user experience meets acceptance criteria
- Identify and report defects early

## 2. Test Items

[List features/user stories to be tested]

| ID | User Story | Description | Priority |
|----|-----------|-------------|----------|
| US-101 | User Registration | As a new user, I want to register... | High |
| US-102 | Profile Update | As a user, I want to update my profile... | High |
| US-103 | Password Change | As a user, I want to change my password... | Medium |

## 3. Features to be Tested

### 3.1 Feature 1: User Registration
- Valid registration with all fields
- Registration with missing required fields
- Registration with invalid email format
- Registration with weak password
- Duplicate email registration

### 3.2 Feature 2: Profile Update
- Update profile with valid data
- Update profile with invalid data
- Update profile with special characters
- Cancel profile update

### 3.3 Feature 3: Password Change
- Change password with valid current password
- Change password with incorrect current password
- Change password with weak new password
- Password change validation

## 4. Features NOT to be Tested

[List features explicitly excluded]

- Email delivery (handled by email service)
- Payment processing (separate feature)
- Mobile app (web only in this sprint)

## 5. Test Approach

### 5.1 Test Levels (ISTQB Standard)

**ISTQB defines 4 test levels:**

**1. Component/Unit Testing (ISTQB):**
- Performed by developers
- Coverage: > 80% code coverage
- Tests individual components in isolation

**2. Integration Testing (ISTQB):**
- API endpoints tested with Postman
- Frontend-backend integration verified
- Tests interaction between components

**3. System Testing (ISTQB):**
- End-to-end user workflows
- Cross-browser testing (Chrome, Firefox, Edge)
- Tests complete integrated system

**4. Acceptance Testing (ISTQB):**
- UAT with product owner
- Business validation
- Tests that system meets business needs

### 5.2 Test Types

| Test Type | Coverage | Tool/Method |
|-----------|----------|-------------|
| Functional | All user stories | Manual + Cypress |
| Regression | Critical paths | Automated suite |
| Exploratory | Edge cases | Ad-hoc testing |
| Usability | User experience | User feedback |
| Security | Authentication, authorization | Manual + OWASP checks |

## 6. Entry Criteria

[Conditions that must be met before testing begins]

- [ ] All user stories have acceptance criteria defined
- [ ] Development environment is ready
- [ ] Test data is prepared
- [ ] Test cases are written and reviewed
- [ ] Test environment is configured
- [ ] Access credentials are available

## 7. Exit Criteria

[Conditions that must be met before testing is complete]

- [ ] All high-priority test cases executed
- [ ] All critical and high-severity bugs fixed and verified
- [ ] Test coverage > 90% for in-scope features
- [ ] No blocking bugs remaining
- [ ] UAT sign-off received
- [ ] Test summary report completed

## 8. Test Environment

**Environment Details:**
- **URL:** https://test-app.example.com
- **Browser:** Chrome 120+, Firefox 121+, Edge 120+
- **OS:** Windows 10, macOS 14
- **Database:** SQL Server 2019
- **API:** REST API v2.1

**Test Data:**
- Test user accounts: testuser1@example.com, testuser2@example.com
- Admin account: admin@example.com
- Test data files: `test-data/users.json`

## 9. Test Schedule

| Phase | Duration | Start Date | End Date | Responsible |
|-------|----------|------------|----------|-------------|
| Test Planning | 2 days | 2024-01-15 | 2024-01-16 | QA Lead |
| Test Case Design | 3 days | 2024-01-17 | 2024-01-19 | QA Team |
| Test Execution | 5 days | 2024-01-22 | 2024-01-26 | QA Team |
| Bug Verification | 2 days | 2024-01-29 | 2024-01-30 | QA Team |
| Test Reporting | 1 day | 2024-01-31 | 2024-01-31 | QA Lead |

## 10. Resource Allocation

| Role | Name | Responsibilities |
|------|------|-----------------|
| QA Lead | [Name] | Test planning, coordination, reporting |
| QA Engineer | [Name] | Test execution, bug reporting |
| QA Engineer | [Name] | Test execution, automation |

## 11. Risks and Mitigation

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Delayed development | High | Medium | Start testing early, test incrementally |
| Environment issues | Medium | Low | Have backup environment ready |
| Insufficient test data | Medium | Low | Prepare test data in advance |
| Scope creep | High | Medium | Strict change control process |

## 12. Test Deliverables

- [ ] Test Plan (this document)
- [ ] Test Cases (in Azure DevOps)
- [ ] Test Execution Reports
- [ ] Bug Reports (in Azure DevOps)
- [ ] Test Summary Report
- [ ] Automation Scripts (if applicable)

## 13. Defect Management (ISTQB Standard)

**ISTQB Defect Classification:**

**Severity Levels (Impact on System):**
- **Critical:** System crash, data loss, security breach
- **High:** Major feature broken, workaround exists
- **Medium:** Feature partially broken, minor impact
- **Low:** Cosmetic, minor inconvenience

**Priority Levels (Urgency to Fix):**
- **P1 (Critical):** Fix immediately (same day)
- **P2 (High):** Fix in current sprint
- **P3 (Medium):** Fix in next sprint
- **P4 (Low):** Fix when time permits

**ISTQB Defect Lifecycle:**
New → Assigned → In Progress → Fixed → Verified → Closed

**Note:** Severity = Impact, Priority = Urgency (ISTQB distinction)

## 14. Test Metrics (ISTQB Standard)

**ISTQB Standard Metrics:**

**Test Execution Metrics:**
- Total test cases: [Number]
- Test cases executed: [Number]
- Test cases passed: [Number]
- Test cases failed: [Number]
- Test cases blocked: [Number]
- Test execution rate: [%]

**Defect Metrics (ISTQB):**
- Total defects found: [Number]
- Defects fixed: [Number]
- Defects verified: [Number]
- Defect density: [Defects per feature/module]
- Defect detection percentage (DDP): [%]
- Defect removal efficiency: [%]

**Coverage Metrics:**
- Test coverage: [Percentage of requirements covered]
- Code coverage: [Percentage] (for unit tests)
- Requirement coverage: [Percentage]

## 15. Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| QA Lead | | | |
| Development Lead | | | |
| Product Owner | | | |

---

## Example: Sprint Test Plan Summary

**Test Plan Name:** Sprint 5 - User Management  
**Sprint Duration:** 2 weeks  
**Test Execution:** Week 2  
**Test Cases:** 45  
**Features:** 3 user stories  
**Browsers:** Chrome, Firefox, Edge  
**Exit Criteria:** All P1/P2 bugs fixed, 90% test execution

