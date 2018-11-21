# java keytool工具操作PCKS12证书库

2018年06月12日 10:28:04

阅读数：21

## 前言

java : jdk1.8

## 查看证书库

```shell
keytool -list -keystore <证书库文件名，如：xx.p12，需要替换> -storetype PKCS12 -storepass <密码，需要替换>
```

## 从证书库中导出证书

```shell
keytool -export -alias <证书别名，需要替换> -file <证书文件名，如my.cer，需要替换> -keystore <证书库文件名，如：xx.p12，需要替换> -storetype PKCS12 -storepass <密码，需要替换>
```

证书别名：通过查看证书库，确定证书别名。