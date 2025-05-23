# carCover
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
import time

options = webdriver.ChromeOptions()
options.add_argument("--headless")
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)

url = "https://www.olx.in/items/q-car-cover"
driver.get(url)

time.sleep(5)

listings = driver.find_elements(By.CSS_SELECTOR, 'li.EIR5N')
results = []

for item in listings:
    try:
        title = item.find_element(By.CSS_SELECTOR, 'span._2tW1I').text
        price = item.find_element(By.CSS_SELECTOR, 'span._89yzn').text
        location = item.find_element(By.CSS_SELECTOR, 'span._2TVI3').text
        results.append(f"{title} | {price} | {location}")
    except:
        continue

driver.quit()

with open("olx_car_covers.txt", "w", encoding="utf-8") as f:
    for line in results:
        f.write(line + "\n")

print("Search results saved to olx_car_covers.txt")
