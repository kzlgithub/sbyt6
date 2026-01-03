

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
