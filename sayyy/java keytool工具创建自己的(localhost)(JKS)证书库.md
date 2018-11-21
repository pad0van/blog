# java keytool工具创建自己的(localhost)(JKS)证书库

2018年06月12日 11:28:06

阅读数：36

## 前言

java ： jdk1.8 
证书库：用于在本地测试的、locahost域名证书库。 
证书库密码：密码为“localhost”。 
证书库位置：当前路径 
证书库文件名：localhost.jks 
证书库格式：JKS

## 创建证书库及localhost证书

```shell
keytool -genkey -alias localhost -keyalg RSA –keysize 4096 -keypass localhost -keystore localhost.jks -storepass localhost -dname "cn=localhost,ou=localhost,o=localhost,l=beijing,st=beijing,c=cn" -validity 3650
```

## 查看证书库

```shell
keytool -list -keystore localhost.jks -storetype JKS -storepass localhost
```

## 导出证书

```shell
keytool -export -alias localhost -file localhost.cer -keystore localhost.jks 
```