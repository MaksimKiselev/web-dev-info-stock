# Python

##### Selenium IDE запуск chromedriver в headless режиме бех X сервера

Установка зависимостей
```bash
# Chrome (debian) и xvfb 
apt-get wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get update && \
  apt-get install -y xvfb google-chrome-stable

# pyvirtualdisplay и webdriver
pip install --no-cache-dir selenium pyvirtualdisplay
```

Виртуальный монитор будем создавать используя pyvirtualdisplay который является обёрткой над xvfb, всё остальное как обычно:
```python
from pyvirtualdisplay import Display
from selenium import webdriver

display = Display(visible=0, size=(1024, 768))
display.start()

options = webdriver.ChromeOptions()
options.add_argument('--headless')
options.add_argument('--no-sandbox')
options.add_argument('--disable-gpu')
options.add_argument('--window-size=1024x768')
options.add_argument('--start-maximized')

driver = webdriver.Chrome(chrome_options=options)

driver.get('https://google.com/')
driver.save_screenshot('google.png')

driver.quit()
display.stop()
```
