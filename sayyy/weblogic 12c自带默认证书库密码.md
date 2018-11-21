# weblogic 12c自带默认证书库密码

2018年06月14日 17:58:34

阅读数：73

# 身份认证证书库（DemoIdentity.jks）

Keystore password（证书库密码） ：DemoIdentityKeyStorePassPhrase

## localhost域名对应的证书

别名：demoidentity 
域名：localhost（一般情况下是localhost，redhat7下是：localhost.localdomain，查看方法见后面内容） 
Private key password（私钥库密码） ：DemoIdentityPassPhrase

# 信任证书库（DemoTrust.jks）

Trust store password（证书库密码） : DemoTrustKeyStorePassPhrase

# 证书操作

## 查看证书列表

```shell
keytool -list -keystore DemoIdentity.jks -storetype JKS -storepass DemoIdentityKeyStorePassPhrase
keytool -list -keystore DemoTrust.jks -storetype JKS -storepass DemoTrustKeyStorePassPhrase
```

## 查看别名为demoidentity的证书对应的域名

```shell
keytool -list -keystore DemoIdentity.jks -storetype JKS -storepass DemoIdentityKeyStorePassPhrase -alias demoidentity -v
```

在反馈信息中找：所有者信息 -> CN的内容。