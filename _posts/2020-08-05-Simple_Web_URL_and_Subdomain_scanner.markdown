---
layout: post
title:  "Simple_Web_URL_and_Subdomain_scanner"
date:   2020-08-05 04:00
categories: Programming
tags: [python, tool]
---
  
This simple code will help you when you can't use pulic web scanner. :)  
  
퍼블릭툴을 쓰기 어렵거나 불가능한 장소에서 요긴하게 사용할 간단한 스캐너이다.  
  
  
  
feature.  

 - [x] Web Directory / url scan base on %Dic.txt  
 - [x] result will be : URL / Response code / Response size  
 - [x] Simple Subdomain scan on hackertagrget.com  




  
  
Usage : python3 scanner.py --help  
python3 scanner.py d --dir URL.txt (http/https)://target.com  
  
  
  
  
```
import requests
import argparse,textwrap
import time
from urllib.request import urlopen
from urllib.parse import urlparse
from bs4 import BeautifulSoup
import re
import ssl

requests.packages.urllib3.disable_warnings()

#인자값 받은 인스턴스 생성
parser = argparse.ArgumentParser(description='scanner test',usage = 'use "python scanner.py --help"',formatter_class=argparse.RawTextHelpFormatter)

#인자값 등록D
parser.add_argument('option', choices=['d','i','s'], default ='d',
                    help= textwrap.dedent('''
                    d = 디렉토리 스캔, text 파일 필요 ex)scanner.py -d --dir url.txt https://target.com
                    i = URL 내 다른 디렉토리 확인
                    s = 서브 도메인 스캔 ex)scanner.py -s (http/https)target.com
                    '''))
parser.add_argument("url", help="url(with http/https)")
parser.add_argument("--dir", help='directory',required=False)

#인자값 저장
args = parser.parse_args()


#url validation확인
def urlcheck(url):
    if not (url.startswith('http://') or url.startswith('https://')):
        print('you need define http/https to your target, defualt = http')
        url = 'http://'+url
    return url

# 디렉토리 스캔
def directory(url,dirlist):
     with open(dirlist, 'rt', encoding='utf-8') as file:
        line = None  # 변수 line을 None으로 초기화
        while line != '':
            try:
                line = file.readline()
                URI = url + line.strip('\n')  # 파일에서 읽어온 문자열에서 \n 삭제하여 저장
                response = requests.get(URI, allow_redirects=False, verify=False)  # 리스판스 저장
                size = len(response.content)
                time.sleep(0.1)
                print(URI, response.status_code, size)
            except Exception:
                print('Connection Error :',url)
                pass

url = urlcheck(args.url)
print('target:{}'.format(url))

if args.option is 'd':
    dirlist = args.dir
    if dirlist is 'None':
        print('you need use option : --dir [url.txt]')
    else :
        directory(url,dirlist)

# url 추출
elif args.option is 'i':

    context = ssl._create_unverified_context()

    target = url
    html = urlopen(target, context=context)
    bs = BeautifulSoup(html, 'html.parser')

    internalLinks = []
    for link in bs.find_all('a', href=re.compile('^(/|.*' + target + ')')):
        if link.attrs['href'] is not None:
            if link.attrs['href'] not in internalLinks:
                if (link.attrs['href'].startswith('/')):
                    internalLinks.append(target + link.attrs['href'])
                else:
                    internalLinks.append(link.attrs['href'])
    i = '\n'.join(internalLinks)
    print(i)

# 서브도메인 스캔
elif args.option is 's':
    parse = urlparse(args.url)
    url = parse.netloc
    response = requests.get('https://api.hackertarget.com/hostsearch/?q={}'.format(url))
    print(response.text)
else:
    print('usage: scanner_1.py [-h] option url')
```  


