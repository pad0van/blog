# 错误java.security.UnrecoverableKeyException: Cannot recover key

2018年07月18日 10:18:12

阅读数：22

## 前言

- jdk1.8
- window 7
- eclipse 4.7
- 执行之前已经成功执行过很多次的代码，发生错误java.security.UnrecoverableKeyException: Cannot recover key

## 分析

- 经过网上查找为证书库中keypass密码不对所致。
- 在执行代码前，我从其它证书库中拷贝了一个证书到原证书库中，但未对新的证书设置keypass

## 解决办法

- 简单的解决办法：keystore密码和keypass密码使用相同的密码。
- 另一个解决办法：如果有代码的话，可以将keystore密码和keypass密码分别指定。（多数情况下，代码中将keystore密码和keypass密码作为同一个配置项了）

修改keypass密码方法：

```shell
keytool -keystore <证书库文件> -storetype JKS -storepass <证书库密码> -keypasswd -alias <证书别名> -keypass <原keypass密码> -new <新keypass密码，与keystore密码一致>
```