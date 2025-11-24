# Azure DevOps QA Workflow Guide

## Overview

This guide walks you through using Azure DevOps for QA activities in an Angular + REST API project.

---

## 1. Setting Up Test Plans

### 1.1 Creating a Test Plan

1. **Navigate to Test Plans:**
   - Go to your Azure DevOps project
   - Click "Test Plans" in the left navigation

2. **Create New Test Plan:**
   - Click "+ New Test Plan"
   - Fill in details:
     - **Name:** "Sprint 5 - User Management"
     - **Description:** "Test plan for user registration, profile management, and account settings"
     - **Area Path:** Select your project area
     - **Iteration:** Select current sprint (e.g., "Sprint 5")

3. **Save:** Click "Create"

### 1.2 Organizing Test Suites

**Types of Test Suites:**

**Static Suite:**
- Manual selection of test cases
- Use for: Specific test cases you want to group

**Requirement-based Suite:**
- Automatically linked to user stories
- Use for: Testing specific features/user stories

**Query-based Suite:**
- Dynamic based on work item query
- Use for: Tests matching certain criteria

**Best Practice:** Create suites by feature/module:
```
Test Plan: Sprint 5
├── Suite: User Registration
├── Suite: Profile Management
└── Suite: Account Settings
```

---

## 2. Creating Test Cases

### 2.1 Adding Test Cases to a Suite

1. **Open Test Suite:**
   - Click on a test suite in your test plan

2. **Add Test Case:**
   - Click "+ New" → "New Test Case"
   - Or click "Add existing test case"

3. **Fill Test Case Details:**

**Title:**
```
User Login with Valid Credentials
```

**Steps:**
```
1. Navigate to login page
2. Enter valid email: user@example.com
3. Enter valid password: Password123!
4. Click "Login" button
```

**Expected Result:**
```
User is successfully logged in and redirected to dashboard
```

4. **Save:** Click "Save test case"

### 2.2 Linking Test Cases to User Stories

**Method 1: From Test Case**
1. Open test case
2. Click "Links" tab
3. Click "Link to"
4. Select "Tests" link type
5. Search and select user story
6. Click "OK"

**Method 2: From User Story**
1. Open user story
2. Click "Links" tab
3. Click "Link to"
4. Select "Tested By" link type
5. Search and select test case
6. Click "OK"

**Visual Result:**
- User story shows linked test cases
- Test case shows linked user story
- Both appear in traceability views

---

## 3. Executing Tests

### 3.1 Running Tests

1. **Open Test Suite:**
   - Navigate to your test plan
   - Click on a test suite

2. **Start Test Run:**
   - Click "Run" button (or click on a test case)
   - Test Runner opens

3. **Execute Test Steps:**
   - Follow each step in the test case
   - Mark each step as:
     - ✅ **Pass:** Step executed successfully
     - ❌ **Fail:** Step failed
     - ⏭️ **Not Applicable:** Step not relevant

4. **Add Comments:**
   - Click on a step to add comments
   - Document observations, issues, or notes

5. **Attach Evidence:**
   - Click "Attachments" icon
   - Upload screenshots, videos, or logs
   - Best practice: Attach screenshots for failed steps

6. **Mark Test Result:**
   - Overall result: Pass / Fail / Blocked
   - Add summary comment if needed

7. **Save and Close:**
   - Click "Save and Close"
   - Test result is recorded

### 3.2 Bulk Test Execution

1. **Select Multiple Tests:**
   - Check boxes next to test cases
   - Or select all in suite

2. **Run Selected:**
   - Click "Run" button
   - Test Runner opens with all selected tests

3. **Navigate Between Tests:**
   - Use "Next" / "Previous" buttons
   - Or click test name in sidebar

---

## 4. Reporting Bugs

### 4.1 Creating Bug from Test Result

**Method 1: From Failed Test**
1. In Test Runner, mark test as "Fail"
2. Click "Create bug" button
3. Bug is pre-populated with:
   - Title: Based on test case name
   - Steps: From test case steps
   - Attachments: From test run
4. Fill in additional details:
   - Actual result
   - Environment
   - Severity/Priority
5. Click "Create"
6. Bug is automatically linked to test case

**Method 2: Manual Bug Creation**
1. Go to "Boards" → "Work Items"
2. Click "+ New Work Item" → "Bug"
3. Fill bug details (see bug report template)
4. Link to test case:
   - Go to "Links" tab
   - Link to test case that found the bug

### 4.2 Bug Workflow

**States:**
- **New:** Bug just created
- **Active:** Bug assigned, being worked on
- **Resolved:** Developer fixed the bug
- **Closed:** QA verified the fix

**Workflow:**
```
New → Assigned → Active → Resolved → Closed
                    ↓
                 Reopened (if fix doesn't work)
```

---

## 5. Tracking Progress

### 5.1 Test Execution Status

**View Test Results:**
1. Go to Test Plans
2. Click on test plan
3. View summary:
   - Total tests
   - Passed
   - Failed
   - Blocked
   - Not Run

**Filter Tests:**
- By status: Pass, Fail, Blocked, Not Run
- By tester: Who ran the test
- By configuration: Environment, browser

### 5.2 Charts and Reports

**Test Results Chart:**
1. Go to Test Plans
2. Click "Charts" tab
3. View:
   - Test execution progress
   - Pass/fail trends
   - Test coverage

**Analytics Views:**
1. Go to "Analytics" → "Test Plans"
2. Create custom reports:
   - Test execution rate
   - Defect density
   - Test coverage by feature

---

## 6. Linking Work Items

### 6.1 Link Types

**Common Links:**
- **Tests:** Test case tests a user story
- **Tested By:** User story is tested by test case
- **Related To:** General relationship
- **Duplicate Of:** Bug is duplicate of another
- **Child Of:** Hierarchical relationship

### 6.2 Creating Traceability

**Example Workflow:**
1. **User Story:** "As a user, I want to log in"
2. **Test Cases:** Create test cases for login
3. **Link:** Link test cases to user story (Tests link)
4. **Execute:** Run test cases
5. **Bugs:** Create bugs if tests fail
6. **Link Bugs:** Link bugs to user story (Related To)

**Result:**
- User story shows all test cases
- User story shows all bugs
- Full traceability from requirement to test to bug

---

## 7. Best Practices

### 7.1 Test Organization

✅ **DO:**
- Organize test suites by feature/module
- Use requirement-based suites for user stories
- Name test cases clearly and descriptively
- Keep test cases focused (one scenario per test)

❌ **DON'T:**
- Create one huge test suite
- Use vague test case names
- Mix multiple scenarios in one test case

### 7.2 Test Execution

✅ **DO:**
- Execute tests as soon as features are ready (test early, don't wait until sprint end)
- Attach screenshots for failures
- Add detailed comments
- Update test status regularly

❌ **DON'T:**
- Wait until end of sprint to test
- Skip adding evidence
- Leave tests in "Not Run" state

### 7.3 Bug Reporting

✅ **DO:**
- Create bugs immediately when found
- Link bugs to test cases and user stories
- Set appropriate severity and priority
- Add screenshots and steps to reproduce

❌ **DON'T:**
- Wait to report bugs
- Create duplicate bugs
- Use vague bug descriptions

### 7.4 Collaboration

✅ **DO:**
- Review test cases with team
- Share test results in standups
- Update stakeholders on progress
- Use @mentions in comments

❌ **DON'T:**
- Work in isolation
- Keep test results private
- Ignore developer questions

---

## 8. Common Workflows

### 8.1 Sprint Planning

1. **Review User Stories:**
   - Read each user story
   - Understand acceptance criteria

2. **Create Test Cases:**
   - Write test cases for each story
   - Link test cases to stories

3. **Estimate Testing Effort:**
   - Estimate time for each test case
   - Share estimates with team

### 8.2 During Sprint

1. **Test as Features Develop:**
   - Test features as soon as they're ready
   - Don't wait for "testing phase"

2. **Report Bugs Early:**
   - Create bugs immediately
   - Link to test cases and stories

3. **Update Test Status:**
   - Mark tests as Pass/Fail/Blocked
   - Update daily in standup

### 8.3 Sprint Review

1. **Prepare Test Summary:**
   - Test execution status
   - Bugs found and fixed
   - Test coverage

2. **Demo Tested Features:**
   - Show features work as expected
   - Highlight any issues

### 8.4 Sprint Retrospective

1. **Share Quality Insights:**
   - What went well
   - What didn't go well
   - Process improvements

2. **Update Test Strategy:**
   - Adjust based on learnings
   - Improve test coverage

---

## 9. Quick Reference

### Keyboard Shortcuts

- **Ctrl+K:** Quick search
- **Ctrl+Shift+F:** Search across work items
- **Ctrl+Shift+P:** Command palette

### Common Queries

**Active Bugs:**
```
Work Item Type = Bug AND State = Active
```

**Failed Tests:**
```
Test Case AND Outcome = Failed
```

**Tests for User Story:**
```
Work Item Type = Test Case AND Links.Tests = [User Story ID]
```

### Useful Views

- **Test Plans:** Test case management
- **Test Runs:** Test execution history
- **Boards:** Kanban board for bugs
- **Backlogs:** User stories and bugs
- **Queries:** Custom work item queries
- **Analytics:** Reports and charts

---

## 10. Troubleshooting

### Issue: Can't see Test Plans

**Solution:**
- Check you have "Basic + Test Plans" license
- Verify you have permissions
- Contact project administrator

### Issue: Test cases not linking to user stories

**Solution:**
- Check link type is correct ("Tests" or "Tested By")
- Verify both work items exist
- Check you have permissions to link

### Issue: Can't attach files

**Solution:**
- Check file size (max 10MB typically)
- Verify file type is allowed
- Check storage quota

---

## Next Steps

1. **Practice:** Create a test plan for a small feature
2. **Execute:** Run a few test cases
3. **Report:** Create a bug from a failed test
4. **Explore:** Try different views and reports

**Questions?** Review the main roadmap or ask your team lead!

