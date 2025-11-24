# Bug Report Examples

## Example 1: Critical Severity Bug

**Bug ID:** BUG-789  
**Title:** Application crashes when user submits form with special characters in name field  
**Severity:** Critical  
**Priority:** P1  
**Status:** New

**Description:**
The application crashes completely when a user enters special characters (specifically `<script>` tags) in the "First Name" field during registration. This causes a white screen of death and requires the user to refresh the page. This is a security concern as it suggests potential XSS vulnerability.

**Steps to Reproduce:**
1. Navigate to https://test-app.example.com/register
2. Enter "John<script>alert('xss')</script>" in the First Name field
3. Fill other required fields with valid data
4. Click "Register" button
5. Observe application crash

**Reproducibility:** Always (100%)

**Expected Result:**
Application should sanitize the input, display an error message about invalid characters, or handle the input safely without crashing.

**Actual Result:**
- Browser console shows: "Uncaught SyntaxError: Unexpected token '<'"
- Application displays white screen
- Network tab shows 500 Internal Server Error
- User must refresh page to continue

**Environment:**
- URL: https://test-app.example.com
- Browser: Chrome 120.0.6099.109
- OS: Windows 10 Pro
- Network: WiFi

**Attachments:**
- Screenshot: white-screen-error.png
- Console log: console-errors.txt
- Network log: network-500-error.json

**Console Errors:**
```
Uncaught SyntaxError: Unexpected token '<'
    at eval (eval at <anonymous> (app.js:45))
    at RegistrationService.submit (registration.service.ts:23)
```

**Impact:**
- **Security Risk:** High - Potential XSS vulnerability
- **User Impact:** All users attempting registration with special characters
- **Business Impact:** Critical - Application unusable, security risk

**Related Items:**
- User Story: US-102 (User Registration)
- Test Case: TC-REG-005 (Registration with special characters)

---

## Example 2: High Severity Bug

**Bug ID:** BUG-790  
**Title:** Password reset email not received - email service returns 500 error  
**Severity:** High  
**Priority:** P1  
**Status:** Assigned

**Description:**
When users request a password reset, the email is never sent. The API call to the email service fails with a 500 error, but the UI shows a success message. Users are left waiting for an email that never arrives.

**Steps to Reproduce:**
1. Navigate to https://test-app.example.com/login
2. Click "Forgot Password?" link
3. Enter valid email: "user@example.com"
4. Click "Send Reset Link" button
5. Observe success message: "Password reset email sent"
6. Check email inbox (no email received)
7. Check browser network tab (API call failed)

**Reproducibility:** Always (100%)

**Expected Result:**
- Email service successfully sends password reset email
- User receives email within 30 seconds
- Email contains valid reset link

**Actual Result:**
- UI displays: "Password reset email sent. Please check your inbox."
- No email is received
- Network tab shows: `POST /api/auth/reset-password` returns 500 Internal Server Error
- Error in response: `{"error": "Email service unavailable"}`

**Environment:**
- URL: https://test-app.example.com
- Browser: Chrome 120.0.6099.109
- OS: macOS 14.2
- Email: Gmail (verified email service is working)

**Attachments:**
- Screenshot: success-message.png (shows misleading success message)
- Network log: api-error-500.json
- Video: password-reset-flow.mp4

**Network Request:**
```
POST https://api.example.com/auth/reset-password
Status: 500 Internal Server Error
Request Body: {"email":"user@example.com"}
Response: {"error":"Email service unavailable","code":"EMAIL_SERVICE_ERROR"}
```

**Impact:**
- **User Impact:** All users attempting password reset
- **Business Impact:** High - Users cannot reset passwords, may need support
- **Frequency:** 100% of password reset attempts

**Workaround:**
Users can contact support to manually reset password, but this is not scalable.

**Related Items:**
- User Story: US-105 (Password Reset)
- Test Case: TC-AUTH-010 (Password reset email delivery)

**Assignee:** dev-team@example.com

---

## Example 3: Medium Severity Bug

**Bug ID:** BUG-791  
**Title:** Product search results not sorted correctly - "Sort by Price" doesn't work  
**Severity:** Medium  
**Priority:** P2  
**Status:** In Progress

**Description:**
When users select "Sort by Price: Low to High" in the product search results, the products are not sorted correctly. Some products appear out of order, making it difficult for users to find the cheapest items.

**Steps to Reproduce:**
1. Navigate to https://test-app.example.com/products
2. Search for "laptop"
3. Results display (e.g., 20 products)
4. Click "Sort by" dropdown
5. Select "Price: Low to High"
6. Observe products are not in ascending price order

**Reproducibility:** Always (100%)

**Expected Result:**
Products should be sorted in ascending order by price:
- Product A: $299
- Product B: $499
- Product C: $799
- Product D: $999

**Actual Result:**
Products are partially sorted but some are out of order:
- Product A: $299 ✓
- Product C: $799 ✗ (should be after $499)
- Product B: $499 ✗ (should be before $799)
- Product D: $999 ✓

**Environment:**
- URL: https://test-app.example.com
- Browser: Firefox 121.0
- OS: Windows 10
- Screen: 1920x1080

**Attachments:**
- Screenshot: unsorted-products.png (shows incorrect order)
- Screenshot: network-request.png (shows sort parameter sent correctly)

**Additional Information:**
- API request includes correct sort parameter: `?sort=price&order=asc`
- API response appears to have products in correct order
- Issue seems to be in frontend sorting logic

**Impact:**
- **User Impact:** Users looking for cheapest products
- **Business Impact:** Medium - Affects user experience, may reduce sales
- **Frequency:** 100% when sorting by price

**Related Items:**
- User Story: US-203 (Product Search and Filtering)
- Test Case: TC-PROD-015 (Product sorting)

**Assignee:** frontend-dev@example.com

---

## Example 4: Low Severity Bug

**Bug ID:** BUG-792  
**Title:** Typo in error message: "Plese try again" should be "Please try again"  
**Severity:** Low  
**Priority:** P4  
**Status:** Fixed

**Description:**
There is a spelling error in the error message displayed when API request fails. The word "Please" is misspelled as "Plese".

**Steps to Reproduce:**
1. Navigate to any page that makes API calls
2. Simulate API failure (or wait for actual failure)
3. Observe error message: "An error occurred. Plese try again."

**Reproducibility:** Always (when error occurs)

**Expected Result:**
Error message should display: "An error occurred. Please try again."

**Actual Result:**
Error message displays: "An error occurred. Plese try again." (typo in "Please")

**Environment:**
- All browsers and environments
- Error message appears in error-handler.component.ts

**Attachments:**
- Screenshot: typo-error-message.png

**Impact:**
- **User Impact:** Minor - Cosmetic issue, doesn't affect functionality
- **Business Impact:** Low - Affects professionalism/branding
- **Frequency:** Every time error message is displayed

**Related Items:**
- Component: error-handler.component.ts (line 45)

**Resolution:**
Fixed typo in error message string. Changed "Plese" to "Please" in error-handler.component.ts.

**Fixed By:** dev-team@example.com  
**Fixed Date:** 2024-01-20  
**Fix Version:** v2.1.2

**Verification:**
- [x] Verified error message now displays correctly
- [x] Tested in Chrome, Firefox, Edge
- [x] Verified in all error scenarios

**Verified By:** QA-Engineer  
**Verified Date:** 2024-01-21  
**Status:** Verified - Bug is fixed

---

## Example 5: Intermittent Bug

**Bug ID:** BUG-793  
**Title:** Cart icon count sometimes doesn't update after adding product  
**Severity:** Medium  
**Priority:** P2  
**Status:** New

**Description:**
Occasionally, after adding a product to the cart, the cart icon badge count doesn't update immediately. The product is added to the cart (verified by navigating to cart page), but the icon still shows the old count. Refreshing the page fixes the issue.

**Steps to Reproduce:**
1. Navigate to products page
2. Add a product to cart (cart icon shows "1")
3. Add another product to cart
4. Observe cart icon count (sometimes still shows "1" instead of "2")

**Reproducibility:** Sometimes (approximately 30% of the time)

**Expected Result:**
Cart icon should immediately update to show correct count (e.g., "2") after adding product.

**Actual Result:**
Cart icon sometimes doesn't update, showing old count. Navigating to cart page shows correct items. Refreshing page updates icon correctly.

**Environment:**
- URL: https://test-app.example.com
- Browser: Chrome 120.0.6099.109 (most common)
- OS: Windows 10
- Network: Both WiFi and Ethernet (issue occurs on both)

**Attachments:**
- Video: cart-icon-not-updating.mp4 (shows issue occurring)
- Console log: no-errors-in-console.txt (no errors logged)

**Additional Information:**
- Issue seems to occur more frequently when:
  - Adding products quickly in succession
  - Network is slow
  - Multiple tabs open
- No console errors when issue occurs
- API calls complete successfully
- Issue appears to be frontend state management problem

**Impact:**
- **User Impact:** Some users (30% of cases)
- **Business Impact:** Medium - Confusing UX, but workaround exists (refresh)
- **Frequency:** Intermittent (~30% of add-to-cart actions)

**Workaround:**
Refresh page to see correct cart count.

**Related Items:**
- User Story: US-301 (Shopping Cart)
- Test Case: TC-CART-008 (Cart icon updates)

---

## Tips for Writing Effective Bug Reports

### ✅ Good Bug Report Characteristics:

1. **Clear Title:** One-line summary that describes the issue
2. **Detailed Steps:** Anyone can reproduce the bug
3. **Specific Environment:** Browser, OS, version numbers
4. **Visual Evidence:** Screenshots, videos, logs
5. **Impact Assessment:** Who is affected and how
6. **Reproducibility:** How often it occurs
7. **Expected vs Actual:** Clear comparison

### ❌ Common Mistakes to Avoid:

1. **Vague descriptions:** "It doesn't work"
2. **Missing steps:** Can't reproduce the bug
3. **No environment info:** Can't identify the issue
4. **No screenshots:** Hard to understand the problem
5. **Wrong severity/priority:** Misleading prioritization
6. **Duplicate bugs:** Not checking if bug already exists
7. **Incomplete information:** Missing key details

### 📝 Quick Checklist:

- [ ] Clear, descriptive title
- [ ] Detailed steps to reproduce
- [ ] Expected vs actual results
- [ ] Environment information
- [ ] Screenshots/videos attached
- [ ] Console/network logs included
- [ ] Severity and priority set correctly
- [ ] Related items linked
- [ ] Impact described

