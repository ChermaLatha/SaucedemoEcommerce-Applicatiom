SauceDemo E-Commerce Automation Testing
Project Overview
This project automates the functional testing of the SauceDemo e-commerce web application using Selenium WebDriver, Python and PyTest. It follows the Page Object Model (POM) to separate page locators and actions from test logic.

Application URL: https://www.saucedemo.com/
Tools and Technologies
- Python 3
- Selenium WebDriver
- PyTest and pytest-html
- Google Chrome
- PyCharm
- Page Object Model (POM)
- Data-driven testing using PyTest parametrization
- Explicit waits and assertion-based validation
Project Structure
ECommerceWebApplication/
├── Pages/
│   ├── __init__.py
│   ├── login_page.py
│   ├── products_page.py
│   ├── cart_page.py
│   └── checkout_page.py
├── TestCases/
│   ├── __init__.py
│   ├── test_login.py
│   ├── test_products.py
│   └── test_checkout.py
├── TestData/
│   └── login_data.py
├── Reports/
├── Screenshots/
├── pytest.ini
└── README.md
The structure above reflects the planned framework; optional shared fixtures and test data files may vary in the local project.
Test Scenarios
  ID                      Scenario                                  Expected Result
  TC01                    Login with predefined users               Valid users can log in; locked-out user receives an error.

  TC02                    Login with invalid or empty credentials   Appropriate validation messages are displayed.

  TC03                    Logout                                    User returns to the login page.

  TC04                    Cart icon visibility                      Shopping cart icon is visible after login.

  TC05                    Random product selection                  Four products are selected and their names and prices are retrieved.

  TC06                    Add products to cart                      Cart badge displays four items.

  TC07                    Cart product validation                   Cart contains the selected products with matching names and prices.

  TC08                    Complete checkout                         Order summary and subtotal match selected products; order confirmation is displayed.

  TC09                    Product sorting                           Prices are sorted low to high and names are sorted Z to A.
  TC10                    Reset App State                           Cart is cleared and the application returns to its default state with no selections retained.
Parameterized tests may produce more than ten individual execution results.
Installation and Setup
1. Install Python 3 and Google Chrome.
2. Open the project in PyCharm and select the project Python interpreter.
3. Open the PyCharm terminal in the project root directory.
4. Install the required packages:
python -m pip install selenium pytest pytest-html
Selenium Manager normally handles ChromeDriver automatically with recent Selenium versions. Internet access may be required on the first run.

Test Data
The public SauceDemo test credentials used for standard shopping flows are:
- Username: standard_user
- Password: secret_sauce
Login tests also cover other predefined users and invalid or empty credentials. Shopping tests select four random products, so product names and order totals may vary between executions.

Test Execution
Run all test cases:
python -m pytest TestCases -v

Run individual modules:
python -m pytest TestCases/test_login.py -v
python -m pytest TestCases/test_products.py -v
python -m pytest TestCases/test_checkout.py -v
Run TC10 individually:
python -m pytest TestCases/test_checkout.py::test_reset_app_state -v -s

HTML Test Report
Generate a detailed, self-contained HTML report:
python -m pytest TestCases -v --capture=tee-sys --tb=long --durations=0 --html=Reports/SauceDemo_Test_Report.html --self-contained-html
Open Reports/SauceDemo_Test_Report.html in a browser after execution. The report contains test outcomes, durations, captured output and failure tracebacks. Embedded screenshots are available if the optional pytest-html screenshot hook has been configured.

If the HTML arguments are not recognized, install pytest-html using the same interpreter that runs the tests:
python -m pip install pytest-html

Screenshots
The framework includes screenshot capture for checkout and reset failure diagnostics.
Screenshots are stored in the Screenshots/ directory. Automatic screenshot capture for every failed test requires a shared reporting hook.
Known Application Defect
Bug ID: BUG_001
Related Test Case: TC10 -- Reset App State
Suggested Severity: Medium
Status: Open (observed during testing)

Steps to Reproduce:
1. Log in as standard_user.
2. Add four products to the cart.
3. Navigate to the cart and return to the products page.
4. Open the sidebar menu and select Reset App State.
5. Close the menu and verify the cart badge and product buttons.
Expected Result: 
All cart items are cleared and all product buttons return to the default Add to cart state.

Actual Result: The cart badge clears, but previously selected products continue to display Remove. 
All product buttons return to Add to cart only after refreshing the page. 
The empty cart was also verified during the diagnostic flow.

Workaround: Refresh the products page.

Test Result: FAILED when immediate reset behavior is required. The refresh is used for diagnosis, not to override the original assertion.

Evidence: Screenshots/TC10_Reset_Defect.png (if saved by the diagnostic test).
Framework Notes
- Page classes encapsulate locators and reusable browser actions.
- Explicit waits synchronize browser interactions.
- Decimal arithmetic rounded to two decimal places is used for subtotal comparison to avoid floating-point precision mismatches.
- Browser fixtures should close the browser after execution.
- Chrome may show a password breach warning for public demo credentials. If necessary, disable password leak detection only in the automated test browser profile, not in a personal browser profile.

Conclusion
The project demonstrates functional UI automation of login, product selection, cart management, checkout, sorting and application reset. 
The known reset-state issue is documented rather than suppressed to preserve the accuracy of the test results. Refer to the latest generated HTML report for actual execution counts and outcomes.