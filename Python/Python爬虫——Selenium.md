## 安装
安装selenium
```
pip3 install selenium
```
安装chromium
官方下载地址是http://chromedriver.chromium.org/downloads,注意需要和本地安装的Chrome浏览器版本相匹配。如当前ChoreDriver2.42支持的Chrome版本是v68到v70
![image.png](https://upload-images.jianshu.io/upload_images/143845-fa5e2cbe82179900.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

同时需要设置chromium的环境变量
```
 mv chromedriver /usr/local/bin
```
验证chromium安装是否正确
```
$ chromedriver
Starting ChromeDriver 2.42.591059 (a3d9684d10d61aa0c45f6723b327283be1ebaad8) on port 9515
Only local connections are allowed.
```
##模拟访问页面
```
from selenium import webdriver

browser = webdriver.Chrome()
browser.get('http://www.baidu.com')
print(browser.page_source)
browser.close()
```
## 查找节点
```
from selenium import webdriver

browser = webdriver.Chrome()
browser.get('http://www.taobao.com')
search_first = browser.find_element_by_id('q')
search_two = browser.find_element_by_name('q')
search_three = browser.find_element_by_xpath("//input[@id='q']")
search_four = browser.find_element_by_css_selector('#q')
print(search_first)
print(search_two)
print(search_three)
print(search_four)
browser.close()
```
##节点交互
```
from selenium import webdriver

browser = webdriver.Chrome()
browser.get('http://www.baidu.com')
input=browser.find_element_by_id('kw')
input.send_keys('apple')
button = browser.find_element_by_id('su')
button.click() 
```
##ip代理
```
from selenium import webdriver

chromeOptions = webdriver.ChromeOptions()
chromeOptions.add_argument('--proxy-server=http://117.67.130.173:25080')
driver =webdriver.Chrome(chrome_options=chromeOptions)
driver.get('http://httpbin.org/ip')
```
##Cookies
```
from selenium import webdriver

brower =webdriver.Chrome()
brower.get('https://www.zhihu.com/explore#monthly-hot')
print(brower.get_cookies())
```
##等待
####隐式等待
调用driver.implicitly_wait。那么在获取不可用的元素之前，会先等待xx秒中的时间
```
from selenium import webdriver
import time

browser =webdriver.Chrome()
browser.implicitly_wait(10)
browser.get('https://www.jd.com/')
input = browser.find_element_by_id('key')
input.send_keys('iphone')
time.sleep(3)
input.clear()
input.send_keys('小米')
btn= browser.find_element_by_xpath('//button[@class="button"]')
btn.click()
```
####显式等待：
显示等待是表明某个条件成立后才执行获取元素的操作。也可以在等待的时候指定一个最大的时间，如果超过这个时间那么就抛出一个异常。显示等待应该使用selenium.webdriver.support.excepted_conditions期望的条件和selenium.webdriver.support.ui.WebDriverWait来配合完成。
```
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
browser =webdriver.Chrome()
browser.get('https://www.jd.com/')
wait = WebDriverWait(browser,10)
input =wait.until(EC.presence_of_element_located((By.ID,'key')))
input.send_keys('iphone')
btn= wait.until(EC.presence_of_element_located((By.XPATH,'//button[@class="button"]')))
btn.click()
```
