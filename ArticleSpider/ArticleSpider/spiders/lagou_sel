import scrapy
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import CrawlSpider, Rule
import time
import pickle
import datetime
import sys
import io
class LagouSpider(CrawlSpider):
    name = 'lagou_sel'
    allowed_domains = ['www.lagou.com']
    start_urls = ['https://www.lagou.com/']
    headers={
        "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
        "Accept-Encoding":"gzip, deflate, br",
        "Accept-Language":"zh-CN,zh;q=0.8",
        "Connection":"keep-alive",
        "User-Agent":"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36",
         "Referer":'https://www.lagou.com',
         'Connection': 'keep-alive',
         "HOST": "www.lagou.com"
        }
    custom_settings = {
        "COOKIES_ENABLED": True
    }
    rules = (
        Rule(LinkExtractor(allow=r'gongsi/j/\d+.html'), follow=True),
        Rule(LinkExtractor(allow=r'zhaopin/.*'), follow=True),
        Rule(LinkExtractor(allow=r'jobs/\d+.html'), callback='parse_job', follow=True),
    )
    def parse_item(self, response):
        pass
    def start_requests(self):
        from selenium import webdriver
        sys.stdout=io.TextIOWrapper(sys.stdout.buffer,encoding='gb18030')
        chrome_opt=webdriver.ChromeOptions()
        prefs={"profile.managed_default_content_settings.images":2}
        chrome_opt.add_experimental_option("prefs",prefs)
        browser = webdriver.Chrome(executable_path="E:/tmp/chromedriver.exe",chrome_options=chrome_opt)
        browser.get("https://passport.lagou.com/login/login.html?service=https%3a%2f%2fwww.lagou.com%2f")
        browser.find_elements_by_css_selector(".input.input_white")[0].send_keys("xxx")
        browser.find_elements_by_css_selector(".input.input_white")[1].send_keys("xx")
        # browser.find_element_by_xpath("/html/body/section/div[1]/div[2]/form/div[2]/input").send_keys(password)
        browser.find_element_by_css_selector(".btn.btn_green.btn_active.btn_block.btn_lg").click()
        time.sleep(10)
        Cookies = browser.get_cookies()
        cookie_dict={}
        for cookie in Cookies:
            f=open('H:/cookies123'+cookie['name']+'.lagou','wb')
            pickle.dump(cookie,f)
            f.close()
            cookie_dict[cookie['name']]=cookie['value']
        browser.close()
        return [scrapy.Request(url=self.start_urls[0], dont_filter=True, cookies=cookie_dict)]
