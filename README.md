# kklt
内容储存
import html
import requests
from bs4 import BeautifulSoup
from pictureContent import content

import time
url = "https://www.umei.cc/"
dic = {
	"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.0.0 Safari/537.36"
}
# resp = requests.get(url= url, headers = dic)
# resp.encoding = 'utf-8'
# print(resp.text)
# resp.close()

resp = content
# print(resp)
# 把代码给bsp
# main_page = BeautifulSoup(resp.text, "html.parser")
main_page = BeautifulSoup(resp, "html.parser")
alist = main_page.find("div", class_="swiper-box").find_all("a")
# print(alist)
for a in alist:
	href = a.get('href')
	# 拿到a链接的href 属性
	# print(href)
	# print(url+href)
	href = (url+href)

	child_page_resp = requests.get(href)
	child_page_resp.encoding ='utf-8'
	child_page_text = child_page_resp.text
	# print(child_page_text)
	# 在子网页中， 获得图片下载路径
	# 获取源码
	child_page = BeautifulSoup(child_page_text, "html.parser")
	p = child_page.find("section", class_="img-content")
	"""
	在于图片路径进入网站之后， 图片在的文件夹不同，这里的值 就不同
	"""

	img = p.find("img")
	src = img.get("src")

	# 下载图片
	img_resp = requests.get(src)
	# img_resp.content #这里拿到的是字节 需要 把字节写道文件中
	img_name = src.split("/")[-1]
	# 拿到url中  /以后的内容
	with open(img_name, mode = "wb") as f:
		f.write(img_resp.content) #图片写入文件
	print("over!!!", img_name)
	time.sleep(1)
	img_resp.close()
print("all_over")

