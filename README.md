# zillow-demo
this my first github repository
  
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
import pandas as pd

import csv
import undetected_chromedriver as uc
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
# Automatically manages ChromeDriver
service = Service(ChromeDriverManager().install())

options = webdriver.ChromeOptions()
# options.add_argument("--headless=new")  # new headless mode for Chrome 109+
# options.add_argument("--disable-gpu")
# options.add_argument("--no-sandbox")
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
import time

driver = uc.Chrome(options=options, driver_executable_path=service.path, headless=False)


driver.get('https://www.zillow.com/homes/72718_rid/')


# time.sleep(2)
# WebDriverWait(driver, 15).until(
#     EC.presence_of_all_elements_located((By.CSS_SELECTOR,  "article[data-test='property-card']"))
# )

page_num = 1
# max_page=2
all_listings = []
# address_list=[]
# price_list=[]
# link_list=[]
# bed_list=[]
# bath_list=[]
# area_list=[]




while True:
    url = f"https://www.zillow.com/tampa-fl-33615/{page_num}_p/"
    driver.get(url)
    time.sleep(5)

    #
    # cards = driver.find_elements(By.CSS_SELECTOR,  "article[data-test='property-card']")
    # if not cards:
    #     break

    cards = driver.find_elements(By.CSS_SELECTOR,  "article[data-test='property-card']")

    for card in cards:
       try:
        address=card.find_element(By.CSS_SELECTOR,'a.property-card-link').text
        print(f"the address is:{address}")
        all_listings.append(address)
        # address_list.append(address)
       except:
        print(" address is none")
       try:
        price=card.find_element(By.XPATH,".//span[@data-test='property-card-price']").text
        print(f"the price is:{price.replace('$','')}")
        # price_list.append(price)
       except:
        print("price is none")
       try:
        links = card.find_element(By.CSS_SELECTOR, 'a.property-card-link').get_attribute("href")
        print(f"the link is:{links}")
        # link_list.append(links)
       except:
        print("links is none")

       try:
        details = card.find_elements(By.TAG_NAME, "li")  # Each li has beds/baths/sqft
        beds = details[0].text
        print('the beds are:',beds)
        # bed_list.append(beds)
        baths = details[1].text
        print("the baths are",baths)
        # bath_list.append(baths)
        area=details[2].text
        print(f"the are is: {area} per square ft")
        # area_list.append(area)

       except:
        print("none")
       print("*" * 30)


    print("*" * 60)
    print(f"Scraped page {page_num} — total listings so far: {len(all_listings)}")
    # if page_num==1:
    #     break
    page_num += 1
# df=pd.DataFrame({'ADDRESS':address_list,"PRICE":price_list,"LINK":link_list,"BEDS":bed_list,"BATH":bath_list,"AREA":area_list})
# df.to_csv("zillow_scrap",index=False)
# print(address_list,price_list,link_list,bed_list,bath_list,area_list)

driver.quit()
