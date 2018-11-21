# java keytool工具操作java带的(JKS)证书库

2018年06月12日 11:07:00

阅读数：68

## 前言

java ： jdk1.8 
证书库：java自带证书库。 
证书库密码：java自带证书库的默认密码为“changeit”。 
jdk安装位置：C:\Program Files\Java\jdk1.8.0_144\ 
证书库位置：C:\Program Files\Java\jdk1.8.0_144\jre\lib\security\ 
证书库文件名：cacerts

## 查看证书库

```shell
keytool -list -keystore "C:\Program Files\Java\jdk1.8.0_144\jre\lib\security\cacerts" -storetype JKS -storepass changeit
```

## 查看证书库中证书详情

```shell
//打印所有证书的详情
keytool -list -v -keystore "C:\Program Files\Java\jdk1.8.0_144\jre\lib\security\cacerts" -storetype JKS -storepass changeit
//打印某一个证书的详情
keytool -list -v -alias <证书别名，需要替换> -keystore "C:\Program Files\Java\jdk1.8.0_144\jre\lib\security\cacerts" -storetype JKS -storepass changeit
```

## 导入证书

```shell
keytool -import -keystore "C:\Program Files\Java\jdk1.8.0_144\jre\lib\security\cacerts" -storetype JKS -storepass changeit -file <证书文件，需要替换，不知道相对路径时，可以使用绝对路径> -alias <证书别名，需要替换，自己起一个有含义的名字即可>
```

注意：在windows下操作时，确保 cmd “以管理员身份运行”。否则，会提示导入失败。