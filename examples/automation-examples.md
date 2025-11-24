# Automation Examples - Cypress & Playwright

## Cypress Examples

### Setup

**Installation:**
```bash
npm install cypress --save-dev
```

**cypress.config.js:**
```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://app.example.com',
    viewportWidth: 1920,
    viewportHeight: 1080,
    defaultCommandTimeout: 10000,
    video: true,
    screenshotOnRunFailure: true,
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
  },
})
```

---

### Example 1: Login Test

**cypress/e2e/login.cy.js:**
```javascript
describe('User Login', () => {
  beforeEach(() => {
    // Visit login page before each test
    cy.visit('/login');
  });

  it('should login successfully with valid credentials', () => {
    // Enter credentials
    cy.get('[data-cy=email-input]').type('user@example.com');
    cy.get('[data-cy=password-input]').type('Password123!');
    
    // Click login button
    cy.get('[data-cy=login-button]').click();
    
    // Verify redirect to dashboard
    cy.url().should('include', '/dashboard');
    
    // Verify welcome message
    cy.get('[data-cy=welcome-message]').should('contain', 'Welcome');
    
    // Verify user menu shows logged-in user
    cy.get('[data-cy=user-menu]').should('be.visible');
  });

  it('should show error with invalid credentials', () => {
    cy.get('[data-cy=email-input]').type('wrong@example.com');
    cy.get('[data-cy=password-input]').type('WrongPassword');
    cy.get('[data-cy=login-button]').click();
    
    // Verify error message
    cy.get('[data-cy=error-message]')
      .should('be.visible')
      .and('contain', 'Invalid credentials');
    
    // Verify still on login page
    cy.url().should('include', '/login');
  });

  it('should validate required fields', () => {
    cy.get('[data-cy=login-button]').click();
    
    // Verify validation messages
    cy.get('[data-cy=email-error]')
      .should('be.visible')
      .and('contain', 'Email is required');
    
    cy.get('[data-cy=password-error]')
      .should('be.visible')
      .and('contain', 'Password is required');
  });

  it('should remember user when "Remember Me" is checked', () => {
    cy.get('[data-cy=email-input]').type('user@example.com');
    cy.get('[data-cy=password-input]').type('Password123!');
    cy.get('[data-cy=remember-me]').check();
    cy.get('[data-cy=login-button]').click();
    
    // Logout
    cy.get('[data-cy=logout-button]').click();
    
    // Return to login page
    cy.visit('/login');
    
    // Verify email is pre-filled
    cy.get('[data-cy=email-input]').should('have.value', 'user@example.com');
  });
});
```

---

### Example 2: Product Search

**cypress/e2e/product-search.cy.js:**
```javascript
describe('Product Search', () => {
  beforeEach(() => {
    cy.visit('/products');
  });

  it('should search and display results', () => {
    // Enter search term
    cy.get('[data-cy=search-input]').type('laptop');
    cy.get('[data-cy=search-button]').click();
    
    // Wait for results to load
    cy.get('[data-cy=product-list]').should('be.visible');
    
    // Verify results contain search term
    cy.get('[data-cy=product-item]').each(($product) => {
      cy.wrap($product)
        .find('[data-cy=product-name]')
        .should('contain.text', 'laptop');
    });
    
    // Verify results count
    cy.get('[data-cy=results-count]').should('contain', 'products found');
  });

  it('should show "no results" message for invalid search', () => {
    cy.get('[data-cy=search-input]').type('nonexistentproductxyz');
    cy.get('[data-cy=search-button]').click();
    
    cy.get('[data-cy=no-results-message]')
      .should('be.visible')
      .and('contain', 'No products found');
  });

  it('should filter results by price range', () => {
    cy.get('[data-cy=search-input]').type('laptop');
    cy.get('[data-cy=search-button]').click();
    
    // Apply price filter
    cy.get('[data-cy=price-filter-min]').type('500');
    cy.get('[data-cy=price-filter-max]').type('1000');
    cy.get('[data-cy=apply-filters]').click();
    
    // Verify all products are within price range
    cy.get('[data-cy=product-price]').each(($price) => {
      const price = parseFloat($price.text().replace('$', ''));
      expect(price).to.be.at.least(500);
      expect(price).to.be.at.most(1000);
    });
  });
});
```

---

### Example 3: Shopping Cart

**cypress/e2e/shopping-cart.cy.js:**
```javascript
describe('Shopping Cart', () => {
  beforeEach(() => {
    // Login first
    cy.login('user@example.com', 'Password123!');
    cy.visit('/products');
  });

  it('should add product to cart', () => {
    // Add first product to cart
    cy.get('[data-cy=product-item]').first().within(() => {
      cy.get('[data-cy=add-to-cart]').click();
    });
    
    // Verify success message
    cy.get('[data-cy=success-message]')
      .should('be.visible')
      .and('contain', 'added to cart');
    
    // Verify cart icon updates
    cy.get('[data-cy=cart-count]').should('contain', '1');
    
    // Navigate to cart
    cy.get('[data-cy=cart-icon]').click();
    
    // Verify product in cart
    cy.get('[data-cy=cart-item]').should('have.length', 1);
  });

  it('should update product quantity in cart', () => {
    // Add product and go to cart
    cy.addProductToCart('product-123');
    cy.visit('/cart');
    
    // Update quantity
    cy.get('[data-cy=quantity-input]').clear().type('3');
    cy.get('[data-cy=update-quantity]').click();
    
    // Verify total updates
    cy.get('[data-cy=cart-total]').should('contain', '$2,997');
  });

  it('should remove product from cart', () => {
    cy.addProductToCart('product-123');
    cy.visit('/cart');
    
    cy.get('[data-cy=remove-item]').click();
    cy.get('[data-cy=confirm-remove]').click();
    
    cy.get('[data-cy=empty-cart-message]')
      .should('be.visible')
      .and('contain', 'Your cart is empty');
  });
});
```

---

### Example 4: Custom Commands

**cypress/support/commands.js:**
```javascript
// Custom login command
Cypress.Commands.add('login', (email, password) => {
  cy.visit('/login');
  cy.get('[data-cy=email-input]').type(email);
  cy.get('[data-cy=password-input]').type(password);
  cy.get('[data-cy=login-button]').click();
  cy.url().should('include', '/dashboard');
});

// Custom command to add product to cart
Cypress.Commands.add('addProductToCart', (productId) => {
  cy.visit(`/products/${productId}`);
  cy.get('[data-cy=add-to-cart]').click();
  cy.get('[data-cy=success-message]').should('be.visible');
});

// Custom command to wait for API
Cypress.Commands.add('waitForApi', (method, url) => {
  cy.intercept(method, url).as('apiCall');
  cy.wait('@apiCall');
});
```

**Usage:**
```javascript
it('should use custom commands', () => {
  cy.login('user@example.com', 'Password123!');
  cy.addProductToCart('product-123');
  cy.waitForApi('GET', '/api/products');
});
```

---

### Example 5: API Testing with Cypress

**cypress/e2e/api.cy.js:**
```javascript
describe('API Tests', () => {
  it('should login via API', () => {
    cy.request({
      method: 'POST',
      url: 'https://api.example.com/v1/auth/login',
      body: {
        email: 'user@example.com',
        password: 'Password123!'
      }
    }).then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body).to.have.property('access_token');
      expect(response.body.access_token).to.be.a('string');
    });
  });

  it('should get user profile', () => {
    // First login to get token
    cy.request({
      method: 'POST',
      url: 'https://api.example.com/v1/auth/login',
      body: {
        email: 'user@example.com',
        password: 'Password123!'
      }
    }).then((loginResponse) => {
      const token = loginResponse.body.access_token;
      
      // Use token to get profile
      cy.request({
        method: 'GET',
        url: 'https://api.example.com/v1/users/me',
        headers: {
          'Authorization': `Bearer ${token}`
        }
      }).then((profileResponse) => {
        expect(profileResponse.status).to.eq(200);
        expect(profileResponse.body.user).to.have.property('email');
      });
    });
  });

  it('should handle API errors', () => {
    cy.request({
      method: 'POST',
      url: 'https://api.example.com/v1/auth/login',
      body: {
        email: 'wrong@example.com',
        password: 'WrongPassword'
      },
      failOnStatusCode: false
    }).then((response) => {
      expect(response.status).to.eq(401);
      expect(response.body).to.have.property('error');
    });
  });
});
```

---

## Playwright Examples

### Setup

**Installation:**
```bash
npm install playwright
npx playwright install
```

**playwright.config.js:**
```javascript
const { defineConfig, devices } = require('@playwright/test');

module.exports = defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'https://app.example.com',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
  ],
});
```

---

### Example 1: Login Test

**tests/login.spec.js:**
```javascript
const { test, expect } = require('@playwright/test');

test.describe('User Login', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should login successfully with valid credentials', async ({ page }) => {
    // Enter credentials
    await page.fill('[data-cy=email-input]', 'user@example.com');
    await page.fill('[data-cy=password-input]', 'Password123!');
    
    // Click login button
    await page.click('[data-cy=login-button]');
    
    // Wait for navigation
    await page.waitForURL('**/dashboard');
    
    // Verify welcome message
    const welcomeMessage = page.locator('[data-cy=welcome-message]');
    await expect(welcomeMessage).toBeVisible();
    await expect(welcomeMessage).toContainText('Welcome');
  });

  test('should show error with invalid credentials', async ({ page }) => {
    await page.fill('[data-cy=email-input]', 'wrong@example.com');
    await page.fill('[data-cy=password-input]', 'WrongPassword');
    await page.click('[data-cy=login-button]');
    
    // Verify error message
    const errorMessage = page.locator('[data-cy=error-message]');
    await expect(errorMessage).toBeVisible();
    await expect(errorMessage).toContainText('Invalid credentials');
    
    // Verify still on login page
    await expect(page).toHaveURL(/.*login/);
  });

  test('should validate required fields', async ({ page }) => {
    await page.click('[data-cy=login-button]');
    
    // Verify validation messages
    await expect(page.locator('[data-cy=email-error]')).toBeVisible();
    await expect(page.locator('[data-cy=email-error]')).toContainText('Email is required');
    
    await expect(page.locator('[data-cy=password-error]')).toBeVisible();
    await expect(page.locator('[data-cy=password-error]')).toContainText('Password is required');
  });
});
```

---

### Example 2: Product Search

**tests/product-search.spec.js:**
```javascript
const { test, expect } = require('@playwright/test');

test.describe('Product Search', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/products');
  });

  test('should search and display results', async ({ page }) => {
    // Enter search term
    await page.fill('[data-cy=search-input]', 'laptop');
    await page.click('[data-cy=search-button]');
    
    // Wait for results
    await page.waitForSelector('[data-cy=product-list]');
    
    // Verify results
    const products = page.locator('[data-cy=product-item]');
    const count = await products.count();
    expect(count).toBeGreaterThan(0);
    
    // Verify each product contains search term
    for (let i = 0; i < count; i++) {
      const productName = await products.nth(i).locator('[data-cy=product-name]').textContent();
      expect(productName.toLowerCase()).toContain('laptop');
    }
  });

  test('should filter by price range', async ({ page }) => {
    await page.fill('[data-cy=search-input]', 'laptop');
    await page.click('[data-cy=search-button]');
    
    // Apply filters
    await page.fill('[data-cy=price-filter-min]', '500');
    await page.fill('[data-cy=price-filter-max]', '1000');
    await page.click('[data-cy=apply-filters]');
    
    // Wait for filtered results
    await page.waitForTimeout(1000);
    
    // Verify prices are in range
    const prices = page.locator('[data-cy=product-price]');
    const count = await prices.count();
    
    for (let i = 0; i < count; i++) {
      const priceText = await prices.nth(i).textContent();
      const price = parseFloat(priceText.replace('$', '').replace(',', ''));
      expect(price).toBeGreaterThanOrEqual(500);
      expect(price).toBeLessThanOrEqual(1000);
    }
  });
});
```

---

### Example 3: API Testing with Playwright

**tests/api.spec.js:**
```javascript
const { test, expect } = require('@playwright/test');

test.describe('API Tests', () => {
  let authToken;

  test('should login via API', async ({ request }) => {
    const response = await request.post('https://api.example.com/v1/auth/login', {
      data: {
        email: 'user@example.com',
        password: 'Password123!'
      }
    });

    expect(response.status()).toBe(200);
    const body = await response.json();
    expect(body).toHaveProperty('access_token');
    authToken = body.access_token;
  });

  test('should get user profile with token', async ({ request }) => {
    // First login
    const loginResponse = await request.post('https://api.example.com/v1/auth/login', {
      data: {
        email: 'user@example.com',
        password: 'Password123!'
      }
    });
    const loginBody = await loginResponse.json();
    const token = loginBody.access_token;

    // Get profile
    const profileResponse = await request.get('https://api.example.com/v1/users/me', {
      headers: {
        'Authorization': `Bearer ${token}`
      }
    });

    expect(profileResponse.status()).toBe(200);
    const profileBody = await profileResponse.json();
    expect(profileBody.user).toHaveProperty('email');
  });

  test('should handle 401 unauthorized', async ({ request }) => {
    const response = await request.get('https://api.example.com/v1/users/me', {
      headers: {
        'Authorization': 'Bearer invalid-token'
      }
    });

    expect(response.status()).toBe(401);
    const body = await response.json();
    expect(body).toHaveProperty('error');
  });
});
```

---

### Example 4: Page Object Model

**pages/LoginPage.js:**
```javascript
class LoginPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.locator('[data-cy=email-input]');
    this.passwordInput = page.locator('[data-cy=password-input]');
    this.loginButton = page.locator('[data-cy=login-button]');
    this.errorMessage = page.locator('[data-cy=error-message]');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email, password) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }

  async getErrorMessage() {
    return await this.errorMessage.textContent();
  }
}

module.exports = { LoginPage };
```

**tests/login-pom.spec.js:**
```javascript
const { test, expect } = require('@playwright/test');
const { LoginPage } = require('../pages/LoginPage');

test.describe('Login with Page Object Model', () => {
  test('should login successfully', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login('user@example.com', 'Password123!');
    
    await expect(page).toHaveURL(/.*dashboard/);
  });

  test('should show error for invalid credentials', async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login('wrong@example.com', 'WrongPassword');
    
    const errorMessage = await loginPage.getErrorMessage();
    expect(errorMessage).toContain('Invalid credentials');
  });
});
```

---

## Best Practices

### ✅ DO:

- Use data-cy attributes for selectors (most stable)
- Use custom commands/helpers for reusable actions
- Wait for elements explicitly
- Use Page Object Model for complex pages
- Test on multiple browsers
- Take screenshots on failure
- Use fixtures for test data
- Keep tests independent

### ❌ DON'T:

- Use brittle selectors (CSS classes that change)
- Use hardcoded waits (use proper waits)
- Write tests that depend on each other
- Ignore flaky tests
- Test implementation details
- Skip error scenarios

---

## Running Tests

**Cypress:**
```bash
# Open Cypress UI
npx cypress open

# Run headless
npx cypress run

# Run specific test
npx cypress run --spec "cypress/e2e/login.cy.js"
```

**Playwright:**
```bash
# Run all tests
npx playwright test

# Run specific test
npx playwright test login

# Run in UI mode
npx playwright test --ui

# Run in specific browser
npx playwright test --project=chromium
```

