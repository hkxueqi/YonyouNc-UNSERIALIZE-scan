# YonyouNc-UNSERIALIZE-scan
Yonyou-UNSERIALIZE,用友NC 反序列化检查工具,批量检测用友NC 反序列化漏洞。

yonyouNC 漏洞批量检测工具(无依赖) 
by: xueqi

## 介绍
实现的功能：
1.出网检测：通过dnslog回显检测存在漏洞的接口
该方式又分为两种方法：cc链中URLDNS 和 执行ping命令
执行命令的方法 需要判断目标操作系统，而决定增加`cmd /c ping dnslog`或`/bin/sh -c  “ping -c 1 dnslog“` 防止执行失败
2.不出网检测：通过执行系统命令写入文本来判断存在漏洞接口
该方式需要判断目标操作系统，而决定增加`cmd /c`或`/bin/sh -c ““` 防止执行失败
## 使用说明
```
optional arguments:
  -h, --help      show this help message and exit
  -u , --url      example: -u http://xxx.xxx  #yonyou的url
  -f , --file     example: -f url.txt #一行一个带协议,dns最大999行
  -d , --dnslog   example: -d xxx.dnslog.cn #最长27字符
  -e , --exp      example: -e txtwin/dnslin/urldns #回显方式
```

## 注意事项

没有linux环境，未测试，使用写入文本模式 可能会产生错误导致检测失败。

dnslog域名最大支持27个字符，程序自动增加 6个字符(5字符子域名加 `.`点号分隔)，最终全长33个字符,不够在前缀补位

```
xxxxx01.03d30ba4.ipv6.1433.eu.org
11101.36c1f3f7.ipv6.bypass.eu.org
xxxxxxxxxxxxxx01.rggehd.dnslog.cn
c6xxxxxxxxxxxxxx01.qpnn6a.ceye.io
```

#### 
