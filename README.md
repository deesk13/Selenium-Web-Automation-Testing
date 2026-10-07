# Selenium Web Automation Testing

A Python-based Selenium automation project containing individual test cases for practicing web automation concepts such as login, JavaScript alerts, mouse actions, drag and drop, explicit waits, and dynamic elements.

## Technologies Used

* Python 3
* Selenium WebDriver
* Chrome Browser
* ChromeDriver
* WebDriver Manager
* Pytest

## Project Structure

```text
selenium-automation-project/
│
├── README.md
├── requirements.txt
├── .gitignore
│
└── tests/
    ├── tc01_saucedemo_login.py
    ├── tc02_alert_accept.py
    ├── tc03_alert_dismiss.py
    ├── tc04_prompt_alert.py
    ├── tc05_mouse_hover.py
    ├── tc06_double_click.py
    ├── tc07_drag_drop.py
    ├── tc08_explicit_wait.py
    ├── tc09_clickable_wait.py
    └── tc10_alert_wait.py
```

## Test Cases

| Test Case | Concept                  | Website                  |
| --------- | ------------------------ | ------------------------ |
| TC01      | Login Automation         | SauceDemo                |
| TC02      | Accept JavaScript Alert  | Automation Testing       |
| TC03      | Dismiss JavaScript Alert | Automation Testing       |
| TC04      | Handle Prompt Alert      | Automation Testing       |
| TC05      | Mouse Hover              | Automation Testing       |
| TC06      | Double Click             | Test Automation Practice |
| TC07      | Drag and Drop            | jQuery UI                |
| TC08      | Explicit Wait            | The Internet             |
| TC09      | Element to Be Clickable  | The Internet             |
| TC10      | Alert Wait               | Automation Testing       |

## TC01 - SauceDemo Login

The test opens the SauceDemo website, enters valid login credentials, clicks the login button, and verifies that the inventory page is displayed.

### Test Flow

```text
Open SauceDemo
      ↓
Enter Username
      ↓
Enter Password
      ↓
Click Login
      ↓
Verify Inventory Page
      ↓
Test Passed
```

## TC02 - Accept Alert

Tests handling of a JavaScript confirmation alert.

### Test Flow

```text
Open Alerts page
      ↓
Select Alert with OK & Cancel
      ↓
Click Alert Button
      ↓
Wait for Alert
      ↓
Accept Alert
      ↓
Verify "Ok"
```

## TC03 - Dismiss Alert

Tests dismissing a JavaScript confirmation alert.

### Test Flow

```text
Open Alerts page
      ↓
Select Alert with OK & Cancel
      ↓
Click Alert Button
      ↓
Wait for Alert
      ↓
Dismiss Alert
      ↓
Verify "Cancel"
```

## TC04 - Prompt Alert

Tests entering text into a JavaScript prompt alert.

### Test Flow

```text
Open Alerts page
      ↓
Select Prompt Alert
      ↓
Click Prompt Button
      ↓
Enter Text
      ↓
Accept Alert
      ↓
Verify Entered Text
```

## TC05 - Mouse Hover

Tests mouse hover functionality using Selenium `ActionChains`.

## TC06 - Double Click

Tests double-click functionality using Selenium `ActionChains`.

## TC07 - Drag and Drop

Tests dragging an element from a source location and dropping it onto a target element.

## TC08 - Explicit Wait

Tests Selenium's `WebDriverWait` with a dynamically loaded element.

## TC09 - Element to Be Clickable

Tests waiting for an element to become clickable before interacting with it.

## TC10 - Alert Wait

Tests waiting for a JavaScript alert using Selenium's `alert_is_present()` expected condition.

## Selenium Concepts Practiced

* WebDriver initialization
* Locators

  * ID
  * XPath
  * CSS Selector
* `find_element()`
* `click()`
* `send_keys()`
* `get_attribute()`
* `current_url`
* JavaScript alerts

  * `accept()`
  * `dismiss()`
  * `send_keys()`
* Explicit waits
* Expected Conditions
* Mouse hover
* Double click
* Drag and drop
* Assertions
* Exception handling
* Browser cleanup

## Installation

Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Running Individual Tests

Each test case is maintained separately.

Example:

```bash
python tests/tc01_saucedemo_login.py
```

Run TC02:

```bash
python tests/tc02_alert_accept.py
```

Run any test individually to isolate and debug failures.

## Purpose

This project was created to practice Selenium WebDriver automation using Python and to understand how different web elements and browser interactions can be automated and validated.

# code:
```
import time

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service


def get_driver():

    options = webdriver.ChromeOptions()
    options.add_argument("--start-maximized")
    options.add_argument("--disable-notifications")

    service = Service(ChromeDriverManager().install())

    driver = webdriver.Chrome(
        service=service,
        options=options
    )

    return driver


# ============================================================
# TC01 - SauceDemo Login
# ============================================================

def tc01_saucedemo_login():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC01] SauceDemo Login")

        driver.get("https://www.saucedemo.com/")

        driver.find_element(
            By.ID, "user-name"
        ).send_keys("standard_user")

        driver.find_element(
            By.ID, "password"
        ).send_keys("secret_sauce")

        driver.find_element(
            By.ID, "login-button"
        ).click()

        assert "inventory.html" in driver.current_url

        print("TC01 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC02 - Accept Alert
# ============================================================

def tc02_alert_accept():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC02] Accept Confirmation Alert")

        driver.get(
            "https://demo.automationtesting.in/Alerts.html"
        )

        cancel_tab = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//a[@href='#CancelTab']")
            )
        )

        cancel_tab.click()

        confirm_button = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//div[@id='CancelTab']/button")
            )
        )

        confirm_button.click()

        alert = wait.until(
            EC.alert_is_present()
        )

        alert.accept()

        result = wait.until(
            EC.visibility_of_element_located(
                (By.ID, "demo")
            )
        )

        assert "Ok" in result.text

        print("TC02 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC03 - Dismiss Alert
# ============================================================

def tc03_alert_dismiss():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC03] Dismiss Confirmation Alert")

        driver.get(
            "https://demo.automationtesting.in/Alerts.html"
        )

        cancel_tab = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//a[@href='#CancelTab']")
            )
        )

        cancel_tab.click()

        confirm_button = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//div[@id='CancelTab']/button")
            )
        )

        confirm_button.click()

        alert = wait.until(
            EC.alert_is_present()
        )

        alert.dismiss()

        result = wait.until(
            EC.visibility_of_element_located(
                (By.ID, "demo")
            )
        )

        assert "Cancel" in result.text

        print("TC03 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC04 - Prompt Alert
# ============================================================

def tc04_prompt_alert():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC04] Prompt Alert")

        driver.get(
            "https://demo.automationtesting.in/Alerts.html"
        )

        textbox_tab = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//a[@href='#Textbox']")
            )
        )

        textbox_tab.click()

        prompt_button = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//div[@id='Textbox']/button")
            )
        )

        prompt_button.click()

        coupon_code = "DISCOUNT20"

        alert = wait.until(
            EC.alert_is_present()
        )

        alert.send_keys(coupon_code)

        alert.accept()

        result = wait.until(
            EC.visibility_of_element_located(
                (By.ID, "demo1")
            )
        )

        assert coupon_code in result.text

        print("TC04 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC05 - Mouse Hover
# ============================================================

def tc05_mouse_hover():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC05] Mouse Hover")

        driver.get(
            "https://demo.automationtesting.in/Alerts.html"
        )

        switch_to = wait.until(
            EC.visibility_of_element_located(
                (By.XPATH, "//a[contains(text(),'SwitchTo')]")
            )
        )

        ActionChains(driver).move_to_element(
            switch_to
        ).perform()

        alerts_option = wait.until(
            EC.visibility_of_element_located(
                (By.XPATH, "//a[contains(text(),'Alerts')]")
            )
        )

        assert alerts_option.is_displayed()

        print("TC05 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC06 - Double Click
# ============================================================

def tc06_double_click():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC06] Double Click")

        driver.get(
            "https://testautomationpractice.blogspot.com/"
        )

        field1 = wait.until(
            EC.presence_of_element_located(
                (By.ID, "field1")
            )
        )

        driver.execute_script(
            "arguments[0].scrollIntoView(true);",
            field1
        )

        copy_button = wait.until(
            EC.element_to_be_clickable(
                (
                    By.XPATH,
                    "//button[contains(text(),'Copy Text')]"
                )
            )
        )

        ActionChains(driver).double_click(
            copy_button
        ).perform()

        field2 = wait.until(
            EC.presence_of_element_located(
                (By.ID, "field2")
            )
        )

        assert field2.get_attribute(
            "value"
        ) == "Hello World!"

        print("TC06 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC07 - Drag and Drop
# ============================================================

def tc07_drag_drop():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC07] Drag and Drop")

        driver.get(
            "https://jqueryui.com/resources/demos/droppable/default.html"
        )

        source = wait.until(
            EC.presence_of_element_located(
                (By.ID, "draggable")
            )
        )

        target = wait.until(
            EC.presence_of_element_located(
                (By.ID, "droppable")
            )
        )

        ActionChains(driver).drag_and_drop(
            source,
            target
        ).perform()

        wait.until(
            lambda d: "Dropped!" in target.text
        )

        assert "Dropped!" in target.text

        print("TC07 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC08 - Explicit Wait
# ============================================================

def tc08_explicit_wait():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC08] Explicit Wait")

        driver.get(
            "https://the-internet.herokuapp.com/dynamic_loading/2"
        )

        start_button = wait.until(
            EC.element_to_be_clickable(
                (By.CSS_SELECTOR, "#start button")
            )
        )

        start_button.click()

        finish = wait.until(
            EC.visibility_of_element_located(
                (By.ID, "finish")
            )
        )

        assert "Hello World!" in finish.text

        print("TC08 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC09 - Element to Be Clickable
# ============================================================

def tc09_clickable_wait():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC09] Clickable Wait")

        driver.get(
            "https://the-internet.herokuapp.com/dynamic_controls"
        )

        enable_button = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//button[text()='Enable']")
            )
        )

        enable_button.click()

        input_field = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//input[@type='text']")
            )
        )

        input_field.send_keys("Order Placed")

        assert input_field.get_attribute(
            "value"
        ) == "Order Placed"

        print("TC09 PASSED")

    finally:

        driver.quit()


# ============================================================
# TC10 - Alert Wait
# ============================================================

def tc10_alert_wait():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC10] Alert Wait")

        driver.get(
            "https://demo.automationtesting.in/Alerts.html"
        )

        ok_tab = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//a[@href='#OKTab']")
            )
        )

        ok_tab.click()

        alert_button = wait.until(
            EC.element_to_be_clickable(
                (By.XPATH, "//div[@id='OKTab']/button")
            )
        )

        alert_button.click()

        alert = wait.until(
            EC.alert_is_present()
        )

        alert_text = alert.text

        alert.accept()

        assert "I am an alert box!" in alert_text

        print("TC10 PASSED")

    finally:

        driver.quit()


# ============================================================
# RUN ALL TEST CASES
# ============================================================

if __name__ == "__main__":

    print("\n========================================")
    print("   SELENIUM AUTOMATION TEST SUITE")
    print("========================================")

    tc01_saucedemo_login()
    tc02_alert_accept()
    tc03_alert_dismiss()
    tc04_prompt_alert()
    tc05_mouse_hover()
    tc06_double_click()
    tc07_drag_drop()
    tc08_explicit_wait()
    tc09_clickable_wait()
    tc10_alert_wait()

    print("\n========================================")
    print("   ALL 10 TEST CASES PASSED")
    print("========================================")
```
<img width="946" height="921" alt="image" src="https://github.com/user-attachments/assets/de67303b-6e7a-4efa-b50c-871d9c8690b4" />
<img width="1917" height="856" alt="image" src="https://github.com/user-attachments/assets/2db61f1a-420e-46d0-a5c7-1f671d708bab" />
<img width="1917" height="736" alt="image" src="https://github.com/user-attachments/assets/6382d12f-bb79-4235-9b7f-92903d0698ab" />
<img width="1917" height="951" alt="image" src="https://github.com/user-attachments/assets/a0b56e10-78c6-4af5-8344-99b3d682d464" />
<img width="1915" height="1006" alt="image" src="https://github.com/user-attachments/assets/3d998699-98d8-4499-9220-578cab65cf2e" />
<img width="1917" height="682" alt="image" src="https://github.com/user-attachments/assets/049746aa-8bcd-4b3e-a4a2-3ec696992ea0" />
<img width="1917" height="477" alt="image" src="https://github.com/user-attachments/assets/298b5d86-d048-45a1-9225-75bb53bbb322" />
<img width="1917" height="867" alt="image" src="https://github.com/user-attachments/assets/1abbd5dc-3603-449f-bd8e-ae059b405099" />








