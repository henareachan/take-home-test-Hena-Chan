# take-home-test-Hena-Chan

Take Home Test

(1)
In my previous role, we utilized the following automation tools for various purposes:

Playwright: For end-to-end testing of web applications.
Cucumber: To write tests in a Behavior-Driven Development (BDD) format.
Postman: For API testing.
Jenkins: To manage Continuous Integration and Continuous Delivery (CI/CD).

(2)
CI/CD involves regularly merging and deploying code changes. Automated testing plays a crucial role in this process by verifying that new code doesn't break existing functionality and that the software functions correctly as a whole.

(3)
I will use Git to manage my test scripts by:
- Version Control: Tracking all modifications and maintaining a history of changes.
- Branching: Isolating and testing new features separately from the main codebase.
- Collaboration: Enabling team members to contribute and work together on test scripts.
- Code Review: Allowing team members to review and approve changes before they are merged.
- Automation: Integrating with the CI/CD pipeline to run tests automatically whenever new changes are made.

(4)
Smoke Testing: Conducted to verify that basic features of the application are functioning properly. It serves as an initial check to ensure the application is stable enough for further testing.

Regression Testing: Performed to ensure that existing components of the application continue to work correctly after new features or changes are introduced. Ideally, it is executed every time new updates are made.

(5)
1. Identify parts of application that needs to work together
2. Write test case based on requirements
3. Perform unit testing
4. Conduct integration testing
5. Run regression testing
6. Monitor and test in production

(6)
TDD (Test-Driven Development) is a development approach where you first write tests for a feature before writing the actual code. This method ensures that features are thoroughly tested from the outset and helps reduce the likelihood of bugs throughout the development process.

(7)
1. Prioritizing test cases based on impact
2. Break down test scripts into smaller pieces 
3. Reuse test scripts
4. Run tests in parallel 
5. Review test scripts regularly
6. Maintain clear documentation

(8)
I prioritize test cases using a risk-based approach by first identifying which components are impacted by new changes. I then assess their importance to the application and rank them from highest to lowest priority before starting testing.

In my previous role, we consistently used a risk-based testing approach. During sprint planning, our team evaluated each ticket based on its impact and difficulty. This practice guided me on which features to test and deploy first.

(9)
import { Selector } from 'testcafe';

// Define page selectors
const loginPage = {
    usernameInput: Selector('#username'),
    passwordInput: Selector('#password'),
    loginButton: Selector('#loginButton'),
    twoFactorCodeInput: Selector('#twoFactorCode'),
    submitTwoFactorButton: Selector('#submitTwoFactorButton'),
    passwordRecoveryLink: Selector('#passwordRecoveryLink'),
    recoveryEmailInput: Selector('#recoveryEmail'),
    sendRecoveryEmailButton: Selector('#sendRecoveryEmailButton'),
    confirmationMessage: Selector('#confirmationMessage'),
    errorMessage: Selector('#errorMessage')
};

fixture `Login and 2FA Tests`
    .page `https://example.com/login`;

test('Login with 2FA and password recovery', async t => {
    // Test Data
    const username = 'testuser';
    const password = 'testpassword';
    const twoFactorCode = '123456'; // Replace with actual 2FA code
    const recoveryEmail = 'testuser@example.com';

    // Test Login with 2FA
    await t
        .typeText(loginPage.usernameInput, username)
        .typeText(loginPage.passwordInput, password)
        .click(loginPage.loginButton);

    // Simulate entering 2FA code
    await t
        .typeText(loginPage.twoFactorCodeInput, twoFactorCode)
        .click(loginPage.submitTwoFactorButton);

    // Verify successful login or handle failure
    const isLoggedIn = await Selector('#logoutButton').exists;
    await t.expect(isLoggedIn).ok('Login failed after 2FA');

    // Test Password Recovery
    await t
        .click(loginPage.passwordRecoveryLink)
        .typeText(loginPage.recoveryEmailInput, recoveryEmail)
        .click(loginPage.sendRecoveryEmailButton);

    // Verify password recovery email sent confirmation
    await t
        .expect(loginPage.confirmationMessage.innerText)
        .eql('Password recovery email sent successfully');

    // Optionally: Simulate receiving the recovery email and resetting password (additional steps needed)
});

(10)
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16' # Specify your Node.js version

      - name: Install Dependencies
        run: npm install

      - name: Run Automated Tests
        run: npm test

      - name: Build Application
        run: npm run build

      - name: Deploy Application
        # Add your deployment steps here
        run: echo "Deploying application..."
        # Example: run a deployment script or command
