import csv
import os
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# クロームの立ち上げ
driver = webdriver.Chrome()

# ページ接続
driver.get('https://disaportal.gsi.go.jp/maps/')

# CSVファイルのパス
csv_file_path = 'input/address.csv'

# 出力フォルダのパス
output_folder = 'output'
os.makedirs(output_folder, exist_ok=True)

# CSVファイルから緯度と経度を取得
with open(csv_file_path, 'r') as file:
    reader = csv.DictReader(file)
    for row in reader:
        building_name = row['建物名']
        latitude = row['緯度']
        longitude = row['経度']
        print(f'Processing: {building_name} ({latitude}, {longitude})')

        # キー入力
        xpath_query = '//*[@id="query"]'
        wait = WebDriverWait(driver, 30)  # 最大30秒間待機
        input_element = wait.until(EC.presence_of_element_located((By.XPATH, xpath_query)))
        input_element.clear()  # 入力フィールドをクリア
        input_element.send_keys(f'{latitude},{longitude}')  # 緯度と経度を入力

        # 検索ボタンをクリック
        search_button_xpath = '//*[@id="search_f"]/div/span/button[1]/img'
        search_button = wait.until(EC.presence_of_element_located((By.XPATH, search_button_xpath)))
        search_button.click()

        # 洪水浸水を表示するボタンをクリック
        kouzui_button_xpath = '//*[@id="kouzui_map_icon"]'
        kouzui_button = wait.until(EC.presence_of_element_located((By.XPATH, kouzui_button_xpath)))
        kouzui_button.click()

        # ズームボタンをクリック
        zoom_button_xpath = '//*[@id="map"]/div[2]/div[3]/div[1]/a[1]'
        zoom_button = wait.until(EC.presence_of_element_located((By.XPATH, zoom_button_xpath)))
        zoom_button.click()

        # 10秒待機
        time.sleep(10)

        # スクリーンショットを撮影し保存
        screenshot_name = f'{output_folder}/{building_name}_想定最大浸水.png'
        driver.save_screenshot(screenshot_name)
        print(f'Screenshot saved as: {screenshot_name}')

        # 最大規模の非表示ボタンをクリック
        max_scale_hide_button_xpath = '//*[@id="MobileMainMenuDialog"]/div[2]/div[2]/div/div[3]/ul[2]/li[1]/div[1]/a/div[2]'
        max_scale_hide_button = wait.until(EC.presence_of_element_located((By.XPATH, max_scale_hide_button_xpath)))
        max_scale_hide_button.click()

        # 計画規模の表示ボタンをクリック
        max_scale_hide_button_xpath = '//*[@id="MobileMainMenuDialog"]/div[2]/div[2]/div/div[3]/ul[2]/li[2]/div[1]/a/div[2]'
        max_scale_hide_button = wait.until(EC.presence_of_element_located((By.XPATH, max_scale_hide_button_xpath)))
        max_scale_hide_button.click()

        # 10秒待機
        time.sleep(10)

        # スクリーンショットを撮影し保存
        screenshot_name = f'{output_folder}/{building_name}_計画規模浸水.png'
        driver.save_screenshot(screenshot_name)
        print(f'Screenshot saved as: {screenshot_name}')

        # 洪水浸水を表示するボタンをクリック(洪水非表示)
        kouzui_button_xpath = '//*[@id="kouzui_map_icon"]'
        kouzui_button = wait.until(EC.presence_of_element_located((By.XPATH, kouzui_button_xpath)))
        kouzui_button.click()

        # 高潮氾濫を表示するボタンをクリック
        takashio_button_xpath = '//*[@id="takashio_map_icon"]'
        takashio_button = wait.until(EC.presence_of_element_located((By.XPATH, takashio_button_xpath)))
        takashio_button.click()

        # 10秒待機
        time.sleep(10)

        # スクリーンショットを撮影し保存
        screenshot_name = f'{output_folder}/{building_name}_高潮.png'
        driver.save_screenshot(screenshot_name)
        print(f'Screenshot saved as: {screenshot_name}')

        # 高潮氾濫を表示するボタンをクリック（高潮非表示)
        takashio_button_xpath = '//*[@id="takashio_map_icon"]'
        takashio_button = wait.until(EC.presence_of_element_located((By.XPATH, takashio_button_xpath)))
        takashio_button.click()

        # 津波浸水を表示するボタンをクリック
        tsunami_button_xpath = '//*[@id="tsunami_map_icon"]'
        tsunami_button = wait.until(EC.presence_of_element_located((By.XPATH, tsunami_button_xpath)))
        tsunami_button.click()

        # 10秒待機
        time.sleep(10)

        # スクリーンショットを撮影し保存
        screenshot_name = f'{output_folder}/{building_name}_津波.png'
        driver.save_screenshot(screenshot_name)
        print(f'Screenshot saved as: {screenshot_name}')

        # 津波浸水を表示するボタンをクリック(非表示)
        tsunami_button_xpath = '//*[@id="tsunami_map_icon"]'
        tsunami_button = wait.until(EC.presence_of_element_located((By.XPATH, tsunami_button_xpath)))
        tsunami_button.click()

# クロームの終了処理
driver.quit()
