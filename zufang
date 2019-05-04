import requests
from bs4 import BeautifulSoup
import re
import os
from urllib.parse import urlparse,urljoin
import random
import csv
import time


def get_info(html):
    soup=BeautifulSoup(html,'lxml')
    titles=soup.select("#content p.content__list--item--title.twoline a")
    styles=soup.select("#content p.content__list--item--bottom.oneline")
    prices=soup.select("#content div span em")
    squares=soup.select("#content p.content__list--item--des")
    data=[]
    for ti,st,sq,pr, in zip(titles,styles,squares,prices):
        info={}
        title=ti.get_text().strip()
        info['标题']=title
        style=st.get_text().strip()
        info['户型']=style
        square=re.findall('(\d+)㎡',sq.get_text().strip())[0]
        info['面积(平方)']=square
        price=pr.get_text().strip()
        info['房租(元/月)']=price
        data.append(info)
    return data

def write2csv(url,data):
    name=url.split('/')[-2]
    print('正在写入%s',name)
    with open ('d:\\pythontest\\zufang\\%s.csv'%name,'a',newline='',encoding='utf-8-sig') as f:
        fieldnames=['标题','户型','面积(平方)','房租(元/月)']
        writer=csv.DictWriter(f,fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(data)
        print("写入成功")
    

def get_req(url):
    headers={
        'User-Agent':
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64)\
                        AppleWebKit/537.36 (KHTML, like Gecko) \
                        Chrome/73.0.3683.86 Safari/537.36'
        }
    try:
        html=requests.get(url,headers=headers).text
        return html
    except BaseException:
        print('request error')
        pass
 
def get_page_num(url):
    html=get_req(url)
    soup=BeautifulSoup(html,'lxml')
    try:
        nums=soup.select("#content > div.content__article > div.content__pg")[0]["data-totalpage"]
        return nums
    except BaseException:
        print("暂无房源")
        return False

if __name__=="__main__":
    names=['yanta','weiyang','lianhu','beilin','baqiao','changan4','xinchengqu','gaoling1','xixianxinquxian','yanliang','lintong']
    for name in names:
        base_url="https://xian.lianjia.com/zufang/%s/pg1"%name
        num=get_page_num(name)
        if not num:
            for page in range(1,int(num)+1):
                url=base_url="https://xian.lianjia.com/zufang/%s/pg%s"%(name,page)
                html=get_req(url)
                data=get_info(html)
                write2csv(url,data)
                time.sleep(int(format(random.randint(0,5))))
