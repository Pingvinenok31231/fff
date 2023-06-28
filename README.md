import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from threading import Thread
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup
import requests
import logging

# Настройка логирования
log_file = 'error_log.txt'
logging.basicConfig(filename=log_file, level=logging.ERROR)

# Функция для выполнения логики на каждом сайте
def process_site(site_url, username, password, drivers):
    try:
        # Инициализация веб-драйвера
        options = webdriver.ChromeOptions()
        options.add_argument("executable_path=C:\\chromedriver\\chrome_proxy.exe")
        driver = webdriver.Chrome(options=options)

        # Открытие веб-сайта
        driver.get(site_url)

        # Поиск кнопки "Login"
        login_button = None
        if "coinpayz" in site_url:
            login_button = driver.find_element(By.XPATH, "//button[contains(text(),'Login')]")
        elif "keran" in site_url:
            login_button = driver.find_element(By.XPATH, "//button[contains(text(),'Login')]")
        elif "coinpayu" in site_url:
            login_button = driver.find_element(By.XPATH, "//a[contains(text(),'Sign In')]")
        
        # Нажатие на кнопку "Login"
        if login_button:
            login_button.click()

            # Ввод учетных данных
            username_field = driver.find_element(By.ID, "username")
            password_field = driver.find_element(By.ID, "password")
            username_field.send_keys(username)
            password_field.send_keys(password)

            # Нажатие на кнопку "Войти"
            submit_button = driver.find_element(By.XPATH, "//button[contains(text(),'Submit')]")
            submit_button.click()

            # Дополнительная логика после входа
            # ...

            # Закрытие веб-драйвера
            drivers.append(driver)
            driver.quit()
        else:
            logging.error(f"Не удалось найти кнопку 'Login' на сайте {site_url}")
    except Exception as e:
        logging.error(f"Ошибка при обработке сайта {site_url}: {str(e)}")

# Основной код
site1_url = 'https://coinpayz.xyz/'
site2_url = 'https://keran.co/index.php'
site3_url = 'https://www.coinpayu.com/dashboard'

site1_username = 'Sasha09Ilya@gmail.com'
site1_password = '20102005Link'

site2_username = 'Sasha09Ilya@gmail.com'
site2_password = '20102005Link'

site3_username = 'Sasha09Ilya@gmail.com'
site3_password = '20102005Link'

# Запуск сайтов по очереди
sites = [
    (site1_url, site1_username, site1_password),
    (site2_url, site2_username, site2_password),
    (site3_url, site3_username, site3_password)
]

drivers = []

for site_url, username, password in sites:
    process_site(site_url, username, password, drivers)

# Завершение выполнения скрипта
print("Сбор прибыли завершен.")
