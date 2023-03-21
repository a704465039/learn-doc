# kali下的基础命令

### 设置linux为中文

```
 LANG="zh_CH.UTF-8"
```

# 工具说明

## 信息收集工具

### DNSenum

DNS 枚举工具,获取 dns 解析 IP,谷歌上查找域名需要代理

```
dnsenum --enum example.com
```

### whois

域名基本信息

```
whois example.com
```

### host

域名基本信息

```
dns 信息
host -t ns example.com 
IP 信息
host -t A example.com 
```

### fierce

```
fierce --domain example.com
```

### nmap 扫描工具

1. ip 及是否在线
```
nmap -sn example.com  
```
2. 端口扫描 -sS 扫描的是 tcp 的端口
```
nmap -sS example.com  
```
3. 速度较慢 -sU 扫描的是 udp 的端口
```
nmap -sU example.com  
```
4. 端口及主机操作系统
```
nmap -O example.com  
```
5. 查看对应端口组件
```
nmap -sV example.com  
```
6. 操作系统更详细的信息
```
nmap -A example.com  
```
7. 防火墙过滤端口
```
nmap -sA example.com  
```

## git项目
### 子域名采集相关
1. [OneForAll](https://github.com/shmilylty/OneForAll)
```
python3 oneforall.py --target example.com run
```
2. [Sublist3r](https://github.com/aboul3la/Sublist3r)
3. [subfinder](https://github.com/projectdiscovery/subfinder)


### js漏洞扫描工具
1. [LinkFinder](https://github.com/GerbenJavado/LinkFinder)
```
python linkfinder.py -i http://example.com/ -o results.html
```

### gobuster
dns爆破,但是什么都执行不出来·····可能环境有问题


扫描和收集的工具

## 终端搜索引擎shodan
## 针对WordPress的工具WPScan
## 漏洞扫描工具Nessus

