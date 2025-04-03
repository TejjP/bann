from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
import time

# Instagram Credentials
INSTAGRAM_USERNAME = "your_username"
INSTAGRAM_PASSWORD = "your_password"
TARGET_USERNAME = "target_account"  # The account you want to report

def login_instagram(driver):
    driver.get("https://www.instagram.com/accounts/login/")
    time.sleep(5)

    # Enter username
    username_input = driver.find_element(By.NAME, "username")
    username_input.send_keys(INSTAGRAM_USERNAME)

    # Enter password
    password_input = driver.find_element(By.NAME, "password")
    password_input.send_keys(INSTAGRAM_PASSWORD)
    password_input.send_keys(Keys.RETURN)

    time.sleep(5)  # Wait for login

def report_user(driver):
    driver.get(f"https://www.instagram.com/{TARGET_USERNAME}/")
    time.sleep(5)

    try:
        # Click on the three-dot menu
        options_button = driver.find_element(By.XPATH, "//button[contains(@aria-label, 'Options')]")
        options_button.click()
        time.sleep(3)

        # Click on 'Report'
        report_button = driver.find_element(By.XPATH, "//span[contains(text(), 'Report')]")
        report_button.click()
        time.sleep(3)

        # Click on 'It's inappropriate'
        inappropriate_button = driver.find_element(By.XPATH, "//span[contains(text(), 'It’s inappropriate')]")
        inappropriate_button.click()
        time.sleep(3)

        print("🚨 Manual step required: Please complete the report process manually.")

    except Exception as e:
        print("Error:", e)

if __name__ == "__main__":
    # Initialize WebDriver
    service = Service(ChromeDriverManager().install())
    driver = webdriver.Chrome(service=service)

    try:
        login_instagram(driver)
        report_user(driver)
    finally:
        driver.quit()
