# YonyouNc-UNSERIALIZE-scan
Yonyou-UNSERIALIZE,用友NC 反序列化检查工具,批量检测用友NC 反序列化漏洞。

yonyouNC 漏洞批量检测工具(无依赖) 
by: xueqi

## 介绍
漏洞利用很简单，实现方式也很简单，需要注意
该工具仅实现漏洞检测功能，由于该类利用简单且危害大，利用工具部分暂不公开。

**吐槽部分：**
hw经常遇到yonyou，但公开的工具要么直接忽略反序列化，要么就是零星几个poc。

然后我就随便google历史漏洞，好家伙，您猜怎么着，收集了二十多条漏洞，有预警过的有1day，还有少量厂商无预警无细节无编号的。搭建环境一测你再猜怎么着？全能用啊！

例如nc.message.bs.NCMessageServlet接口，goby在2月就已经有了poc，22年4月就有了补丁更新。我工具都写在里面了 ，23年5月份厂商预警这么个洞，x都吃不上热乎的

点名批评某几个厂商，不行就趁早洗洗睡吧

<img width="321" alt="image" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/03feef43-db8f-4944-9c4f-f68d56cbc455">


### 实现的功能：

**1.出网检测**：通过dnslog回显检测存在漏洞的接口

该方式又分为两种方法：cc链中**URLDNS** 和 **执行ping命令**

URLDNS方式不需要判断操作系统，win和linux通用

执行命令的方法 需要判断目标操作系统，而决定增加`cmd /c ping dnslog`或`/bin/sh -c  “ping -c 1 dnslog“` 防止执行失败

**2.不出网检测**：通过执行系统命令写入文本来判断存在漏洞接口

该方式需要判断目标操作系统，而决定增加`cmd /c`或`/bin/sh -c ““` 防止执行失败


## 使用说明
详情看**使用详解**部分
```
optional arguments:
  -h, --help      show this help message and exit
  -u , --url      example: -u http://xxx.xxx  #yonyou的url
  -f , --file     example: -f url.txt #一行一个带协议,dns最大999行
  -d , --dnslog   example: -d xxx.dnslog.cn #最长27字符
  -e , --exp      example: -e txtwin/dnslin/urldns #回显方式
```

## 注意事项

linux环境未测试，使用写入文本模式（执行命令方法）可能会产生BUG导致检测失败。

dnslog域名最大支持27个字符，程序自动增加 6个字符(5字符子域名加 `.`点号分隔)，最终全长33个字符,不够在前缀补位

```
xxxxx01.03d30ba4.ipv6.1433.eu.org
11101.36c1f3f7.ipv6.bypass.eu.org
xxxxxxxxxxxxxx01.rggehd.dnslog.cn
c6xxxxxxxxxxxxxx01.qpnn6a.ceye.io
```
## dnslog格式说明  
### 检测单个url 

`-u http://url` 

dnslog处理格式
<img width="703" alt="image-20230519162917413" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/7f4c885b-a0f1-4eb6-98be-26fd7b9c2fa6">


### 批量url：

`-f 1.txt #补位变为行数标识`

<img width="674" alt="image-20230519162813383" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/44c0ccbd-14d3-4a9a-af05-d6e4ec71f431">

## 使用详解

### 1.dnslog-URLDNS
1.使用urldns链 ，通过dnslog出网测试。

`python3 YonyouNcScan2.py -u http://172.16.1.200:8000 -d qpnn6a.ceye.io --exp urldns`

<img width="966" alt="image-20230519155021950" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/5cbe9d9b-5cae-4715-b5ab-ec0ca92f1fd5">

<img width="756" alt="image-20230519155038819" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/8c1c8650-99aa-44af-abd5-5fc63cd10db5">

### 2.dnslog-ping
使用cc6链，执行ping命令 通过dnslog回显

需要指定系统，-e dnswin或 dnslin

`python3 YonyouNcScan2.py -u http://172.16.1.200:8000 -d m4ssvh.dnslog.cn -e dnswin`

<img width="940" alt="image-20230519155210357" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/e3009fcd-b929-4381-9632-48019b160fd7">

### 3.txt文本
使用cc6链，执行echo命令 通过重定向符追加到文本至web根目录回显

需要指定系统，-e txtwin或 txtlin；txtwin2使用了cc1链

`python3 YonyouNcScan2.py -u http://172.16.1.200:8000 -d m4ssvh.dnslog.cn -e txtwin`

<img width="791" alt="image-20230519155611731" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/29c02efe-85e2-4e84-90a4-07a0581ba780">

<img width="629" alt="image-20230519155702982" src="https://github.com/hkxueqi/YonyouNc-UNSERIALIZE-scan/assets/42443541/5e56307b-bfbb-44d1-8ba0-56bea20bc656">


