import pandas as pd

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException
import xlsxwriter
def Web_scraping(date):
    driver = webdriver.Chrome()
    driver.maximize_window()
    driver.implicitly_wait(10)

    driver.get("https://opstra.definedge.com/")
    continue_button = WebDriverWait(driver, 10).until(EC.presence_of_element_located(
        (By.XPATH, "//a[contains(@class, 'v-btn__content') and contains(text(), 'CONTINUE WITH LOGIN')]")))
    continue_button.click()
    element = driver.find_element(By.XPATH, "//button[contains(@class, 'v-btn__content') and contains(text(), 'Login')]")
    element.click()

    driver.find_element(By.ID,"username").send_keys("varadsikchi4002@gmail.com")
    driver.find_element(By.ID, "password").send_keys("Webscraping#123")
    login1 = driver.find_element(By.ID, "kc-login")
    login1.click()

    Builder = driver.find_element(By.XPATH, "//div[contains(@class, 'v-btn__content') and contains(text(), 'Builder')]")
    Builder.click()

    Optionchain = driver.find_element(By.XPATH,
                                  "//div[contains(@class, 'v-expansion-panel__header') and contains(., 'Option Chain')]")
    Optionchain.click()

    wait.until(EC.presence_of_element_located((By.CLASS_NAME, "ht_master")))

    table_element = driver.find_element(By.CLASS_NAME, "ht_master")
    table_html = table_element.get_attribute("outerHTML")
    df = pd.read_html(table_html)[0]

    driver.quit()
    return df


# List of dates for which we want to scrape option chain data
dates_to_scrape = ["2023-07-23", "2023-07-24", "2023-07-25", "2023-07-26", "2023-07-27"]

# Loop through each date, scrape data, and store it in separate CSV files
for date in dates_to_scrape:
    data_frame = Web_scraping(date)
    csv_file = f"option_prices_{date}.csv"
    data_frame.to_csv(csv_file, index=False)
