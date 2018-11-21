# WebLogic8.1 配置SSL/HTTPS单向认证

2017年10月26日 17:46:34

阅读数：165

## 准备

1、已经生成名为localhost.jks的证书库，证书库的密码为localhost。 
2、证书库中有别名为localhost的证书，证书的域名为localhost。 
3、已安装WebLogic8.1。

## 步骤

1、将localhost.jks证书库上传到/weblogic/bea/weblogic81/server/lib/目录下。 
/weblogic/bea/weblogic81/server/lib/srieservice.com_keystore.jks

2、在WebLogic的console（控制台）中找到Keystores & SSL 
server->Configuration->Keystores & SSL

3、选择自定义 
Change->Custom Indentity And Custom Trust->continue

4、填写 Custom Identity 
Custom Identity Key Store File Name: /weblogic/bea/weblogic81/server/lib/srieservice.com_keystore.jks 
Custom Identity Key Store Type: JKS 
Custom Identity Key Store Pass Phrase(密码): localhost 
Confirm Custom Identity Key Store Pass Phrase: localhost 
填写完成后，continue

5、填写Review SSL Private Key Settings 
Private Key Alias（证书别名）: localhost 
Passphrase(密码): localhost 
Confirm Passphrase: localhost 
填写完成后，continue，finish。

6、重启WebLogic。 
7、测试。 
在浏览器中打开地址：[https://localhost:7901](https://localhost:7901/) 
server->Configuration->General->SSL Listen Port是https端口。

PS：WebLogic8.1只能识别签名算法为md5、sha1的证书。目前为止，签名算法为md5、sha1的证书已经不安全了。解决办法为：升级WebLogic（这个好像比较麻烦），或者使用反向代理（比如使用nginx将https请求转成http请求）。