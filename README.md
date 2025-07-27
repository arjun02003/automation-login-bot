# automation-login-bot
# 🕸️ Selenium Automation: Candidate Portal Login Bot

This is a Python automation project built using Selenium. The script automates the login process to the [PCET Nagpur Candidate Portal](https://mis.pcenagpur.edu.in/modules/Candidate/candidateLogin.php) and navigates through specific sections like the **Personal** tab and **Next** buttons.

---

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
import time

# Initialize the Chrome driver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))

try:
    # Navigate to the login page
    url = "https://mis.pcenagpur.edu.in/modules/Candidate/candidateLogin.php?admParam=c4ca4238a0b923820dcc509a6f75849b"
    driver.get(url)

    # Wait for the login page to load
    WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, "username")))

    # Locate and fill login fields
    username_field = driver.find_element(By.ID, "username")
    password_field = driver.find_element(By.ID, "password")
    login_button = driver.find_element(By.ID, "candSignInBtn")

    username_field.send_keys("230110495")
    password_field.send_keys("PASS110495")
    login_button.click()

    # Wait for the post-login page to load
    WebDriverWait(driver, 15).until(EC.presence_of_element_located((By.TAG_NAME, "body")))

    # Debug: Print all clickable links and buttons to find "Personal" and "Next"
    print("Debugging: Listing all clickable elements...")
    elements = driver.find_elements(By.XPATH, "//a | //button | //input[@type='submit' or @type='button']")
    for i, elem in enumerate(elements, 1):
        text = elem.text.strip() or "No text"
        tag = elem.tag_name
        id_attr = elem.get_attribute("id") or "No ID"
        class_attr = elem.get_attribute("class") or "No class"
        print(f"Element {i}: Tag={tag}, Text='{text}', ID={id_attr}, Class={class_attr}")

    # Attempt to click the "Personal" tab
    try:
        personal_tab = WebDriverWait(driver, 10).until(
            EC.element_to_be_clickable(
                (By.XPATH, "//a[contains(translate(text(), 'ABCDEFGHIJKLMNOPQRSTUVWXYZ', 'abcdefghijklmnopqrstuvwxyz'), 'personal')] | "
                           "//button[contains(translate(text(), 'ABCDEFGHIJKLMNOPQRSTUVWXYZ', 'abcdefghijklmnopqrstuvwxyz'), 'personal')] | "
                           "//*[contains(@id, 'personal') or contains(@class, 'personal')]")
            )
        )
        personal_tab.click()
        print("Clicked on Personal tab")
    except Exception as e:
        print("Could not find or click Personal tab:", str(e))

    # Attempt to click "Next" buttons repeatedly
    max_steps = 10
    step_count = 0
    while step_count < max_steps:
        try:
            next_button = WebDriverWait(driver, 5).until(
                EC.element_to_be_clickable(
                    (By.XPATH, "//button[contains(translate(text(), 'ABCDEFGHIJKLMNOPQRSTUVWXYZ', 'abcdefghijklmnopqrstuvwxyz'), 'next')] | "
                               "//input[@type='submit' or @type='button'][contains(@value, 'Next')] | "
                               "//*[contains(@id, 'next') or contains(@class, 'next')]")
                )
            )
            next_button.click()
            print(f"Clicked Next button (step {step_count + 1})")
            step_count += 1
            time.sleep(1)  # Wait for page load
        except Exception as e:
            print("No more Next buttons found or error occurred:", str(e))
            break

    # Wait to observe the final result
    time.sleep(5)

except Exception as e:
    print("An error occurred:", str(e))

finally:
    # Close the browser
    driver.quit()
