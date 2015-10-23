# DNS查找
dns的查找是一个递归的过程，如果当前查找的dns服务器不知道要查找的域名，他就会向他的上级询问。而且，dns都有缓存功能，所以，并不是每次查找都要走一条完整路径。

Linux/Unix的dns工具是host，例如：

    libenchao@libenchao-Vostro-3900:~$ host -a -v baidu.com
    Trying "baidu.com"
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60703
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 10, AUTHORITY: 5, ADDITIONAL: 5
    
    ;; QUESTION SECTION:
    ;baidu.com.			IN	ANY
    
    ;; ANSWER SECTION:
    baidu.com.		1546	IN	TXT	"v=spf1 include:spf1.baidu.com include:spf2.baidu.com include:spf3.baidu.com a mx ptr -all"
    baidu.com.		74	IN	A	111.13.101.208
    baidu.com.		74	IN	A	123.125.114.144
    baidu.com.		74	IN	A	180.149.132.47
    baidu.com.		74	IN	A	220.181.57.217
    baidu.com.		75674	IN	NS	ns4.baidu.com.
    baidu.com.		75674	IN	NS	ns3.baidu.com.
    baidu.com.		75674	IN	NS	dns.baidu.com.
    baidu.com.		75674	IN	NS	ns7.baidu.com.
    baidu.com.		75674	IN	NS	ns2.baidu.com.
    
    ;; AUTHORITY SECTION:
    baidu.com.		75674	IN	NS	ns4.baidu.com.
    baidu.com.		75674	IN	NS	ns2.baidu.com.
    baidu.com.		75674	IN	NS	ns3.baidu.com.
    baidu.com.		75674	IN	NS	ns7.baidu.com.
    baidu.com.		75674	IN	NS	dns.baidu.com.
    
    ;; ADDITIONAL SECTION:
    dns.baidu.com.		75895	IN	A	202.108.22.220
    ns2.baidu.com.		75895	IN	A	61.135.165.235
    ns3.baidu.com.		75895	IN	A	220.181.37.10
    ns4.baidu.com.		75895	IN	A	220.181.38.10
    ns7.baidu.com.		75895	IN	A	119.75.219.82
    
    Received 433 bytes from 127.0.1.1#53 in 11 ms