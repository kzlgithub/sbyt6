

### 安装dns工具，用于各种测试：
```
apt-get install dnsutils
```
### 安装bind9：
```
apt-get install bind9
```
### 备份文件named.conf.options：
```
cp /etc/bind/named.conf.options /etc/bind/named.conf.options.backup && chmod 777 /etc/bind/named.conf.options
```
### 写入内容到/etc/bind/named.conf.options：
<details>
<summary>点击展开查看完整代码</summary>
 
```
options {
 directory "/var/cache/bind";

 // If there is a firewall between you and nameservers you want
 // to talk to, you may need to fix the firewall to allow multiple
 // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

 // If your ISP provided one or more IP addresses for stable 
 // nameservers, you probably want to use them as forwarders.  
 // Uncomment the following block, and insert the addresses replacing 
 // the all-0's placeholder.

 allow-query { any; };//允许查询
 allow-query-cache { any; };//允许查询缓存的IP地址列表

 recursion yes;//允许递归查询
 allow-recursion { any; };//允许递归查询的的IP地址列表
 recursive-clients 25;

 //forward only;//只转发

  forwarders {
   77.88.8.8;
   195.46.39.39;
   195.46.39.40;
   77.88.8.1;
   //8.8.8.8;//通用谷歌DNS
   //8.8.4.4;
  };

 //========================================================================
 // If BIND logs error messages about the root key being expired,
 // you will need to update your keys.  See https://www.isc.org/bind-keys
 //========================================================================
 dnssec-validation no;//默认是auto

 listen-on-v6 { any; };
};

```
</details>

### 重启bind：
```
systemctl restart bind9
```
### 查看bind运行状态：
```
systemctl status bind9
```
### 新建文件/etc/rewrite.resolved.sh：
```
touch /etc/rewrite.resolved.sh && chmod 777 /etc/rewrite.resolved.sh
```

### 写入内容到/etc/rewrite.resolved.sh：
<details>
<summary>点击展开查看完整代码</summary>
```
#!/bin/bash

#爬取ipinfo.io获取信息
full_ipinfo=$(curl ipinfo.io)

#截取"ip":之前内容，要用转义字符
ipinfo=${full_ipinfo#*\"ip\":}

#截取"之前内容，要用转义字符
ipinfo=${ipinfo#*\"}

#截取"之后的内容，要用转义字符，注意*号位置和之前不同
ipinfo=${ipinfo%%\"*}

nameserver="nameserver "${ipinfo}

echo ${nameserver} > /etc/resolv.conf

echo ${full_ipinfo}
echo -e "\n\n\n"
echo "公网IP: "${ipinfo}
echo "写入内容: "${nameserver}
echo "/etc/resolv.conf已经更新"

echo -e "\033[32m主机重启或者网络服务重启需要运行此脚本，或设为开机启动\033[0m"
#echo -e "\033[32m        by科卓利企业管理\033[0m"

```
</details>
