# keytool复制证书

2018年06月14日 18:03:41

阅读数：13

## 前言

有时需要将一个证书从A证书库复制到B证书库。

## 操作方法

```shell
keytool -importkeystore  
        -srcstoretype <A证书库存储格式，如PKCS12、JKS> \
        -srckeystore <A证书库文件> \
        -srcstorepass <A证书库密码> \
        -srcalias <A证书库中证书的别名>  \
        -deststoretype <B证书库存储格式，如PKCS12、JKS>  \
        -destkeystore <B证书库文件>  \
        -deststorepass <B证书库密码>  \
        -destalias <复制到B证书库后，证书的别名>\
```