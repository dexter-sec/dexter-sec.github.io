---
layout: post
title:  "Let's make Nuclei Template"
date:   2020-08-13 03:00 +0900
categories: Security
tags: [tool,bugbounty]
---
![Nuclei_logo](/assets/nuclei-logo.png)

https://github.com/projectdiscovery/nuclei
<br  />
<br  />
<br  />
### About Nuclei 
Nuclei는 projectdiscovery의 브랜치로 대규모 확장성과 사용 편의성을 제공하는 템플릿을 기반으로 구성 가능한 대상 스캔을 위한 빠른 도구로 GO로 작성되어 있다. 

사실 단순 http-go만으로 작성된 request 툴이라면, 그리 매력적인 도구는 아니지만, 
<br  />
 -  *.yaml로 작성하여 사용 가능한 커스텀 템플릿의 확장성이 좋다.
 - 오픈 소스인 만큼 templete pull을 해주는 Contributors가 있고, 템플릿의 규모가 커지고 있다.
 -  취약점 점검, 버그 바운티, 서비스 리콘 이외 이모조모 쓸모가 많으며, 자동화 도구 워크 플로우랑 궁합(?)이 좋다.
<br  />
<br  />

![Nuclei_templete](/assets/nuclei_templete.png)

nuclei templet(https://github.com/projectdiscovery/nuclei-templates)에는 (20.08.13기준) 13개 카타고리에 204개 템플릿이 있다. 
<br  />
<br  />
<br  />
### Let's make my own Templete

조금 더 자세한 doc은 Nuclei template를 참조하고(https://nuclei.projectdiscovery.io/templating-guide), 자주 사용하게될 HTTP Request/Response 부분을 보자, 
<br  />
<br  />
```
id: git-config

info:
  name: Git Config File
  author: Ice3man
  severity: medium
  description: Searches for the pattern /.git/config on passed URLs.

requests:
  - method: GET
    path:
      - "{{BaseURL}}/.git/config"
    matchers:
      - type: word
        words:
          - "[core]"

```
<br  />
<br  />
http request template에서는 크게 request, matchers로 나뉜다.
<br  />
<br  />

Request

| Field | Discription|
|--|--|
| method | http 메소드 |
| path | 경로(host name) |
| headers | http header |
| body | request body값 |

<br  />
<br  />

Matches

| Field | Discription|
|--|--|
| status | http 리스폰스 코드 |
| size | Content Length 사이즈 |
| word | 문자열 탐지 |
| regex | regex 문자열 탐지 |
| binary | 바이너리 |
| dsl | 조건문 |

<br  />
<br  />
<br  />
<br  />
<br  />
<br  />

Tomcat examples  template,

```
id: tomcat_examples-panel

info:
  name: tomcat_examples-panel
  author: dexter
  severity: info
  
requests:
  - method: GET
    path:
      - "{{BaseURL}}/examples/"
      - "{{BaseURL}}/examples"
    matchers:
      - type: word
        words:
          - ">Apache Tomcat"
```


![template_test](/assets/template_test.png)

잘 작동한다. :)

template 확장성이 좋아서 자주 애용할것 같다. 
범용적으로 사용하기 좋은 템플릿을 만들면, git pull도 올려봐야겠다. 

