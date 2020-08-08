---
layout: post
title:  "TIP For Wordpress Login Bruteforce"
date:   2020-08-09 07:00 +0900
categories: Security
tags: [bugbounty, bugbountytip, vulnerability]
---


Tip for Get admin info of Wordpress

TL;DR
- Check  author list(author-sitemap.xml) and WP REST API(/wp-json/wp/v2/users) before try Login-BF to Wordpress, 


**yaostSEO plugin** 


![](./yoast-sitemap-index-noauthors.png "img")

>GET http://target#com/author-sitemap.xml

 - WordPress는 웹 사이트에 내용을 게시하는 모든 사용자를 위해 작성자 보관 페이지를 가지고있다.
 - 기본적으로 WordPress는 사용자가 Author Archive 페이지 URL에 로그인하는 'username'을 사용하며 이를 변경할 수 있는 방법을 제공하지 않는다.  
 - yaostSEO가 설치되면 기본적으로 사용자 이름으로 작성된 모든 작성자 URL을 포함하는 작성자 보관 시트맵을 작성하도록 활성화된다.  (Defualt)
 - 고로, 해당 파일내 'user name'으로 Login Bruteforce를 시도할 수 있다. 




**Wordpres REST API**
>GET http://target#com/wp-json/wp/v2/users
>GET http://target#com/wp-json/wp/v2/users/?per_page=100&page=1

![](./wpapiuser.png)

 - Wordpress REST API 접근권한 설정이 미비한경우, users function을 이용해, 유저/어드민 정보를 확보 할 수 있음. 


Ref). 
https://developer.wordpress.org/rest-api/
https://blog.sucuri.net/2020/03/wp-4-7-wp-json-content-injection.html




