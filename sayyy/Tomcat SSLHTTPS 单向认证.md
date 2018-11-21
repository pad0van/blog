# Tomcat SSL/HTTPS 单向认证

2017年10月26日 17:24:13

阅读数：48

## 准备

1、已经生成名为localhost.jks的证书库，证书库的密码为localhost。 
2、证书库中有别名为localhost的证书，证书的域名为localhost。 
3、已安装tomcat7。

## 步骤

1、将localhost.jks证书库放到tomcat的conf文件夹中。 
2、修改tomcat的 server.xml配置文件。在server.xml中 找到下面被注释的这段：

```xml
<!--
<Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
       maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
       clientAuth="false" sslProtocol="TLS" />
-->
```

干掉注释，并将内容改为：

```xml
<Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol" 
        SSLEnabled="true" clientAuth="false" 
        keystoreFile="${catalina.base}/conf/localhost.jks" keystorePass="localhost" keystoreType="JKS" 
        maxThreads="150" scheme="https" secure="true" sslProtocol="TLS"/>
```

3、测试tomcat是否启用https/SSL 
在浏览器中打开地址：[https://localhost:8443](https://localhost:8443/)

4、在项目中启用https/SSL 
打开应用的 web.xml 文件，增加配置如下：

```xml
<security-constraint>
    <web-resource-collection>
        <web-resource-name>securedapp</web-resource-name>
        <url-pattern>/*</url-pattern>
    </web-resource-collection>
    <user-data-constraint>
        <transport-guarantee>CONFIDENTIAL</transport-guarantee>
    </user-data-constraint>
</security-constraint>
```

将 URL 映射设为 /* ，这样你的整个应用都要求是 HTTPS 访问，而 transport-guarantee 标签设置为 CONFIDENTIAL 以便使应用支持 SSL。

5、测试项目中是否启用https/SSL 
在浏览器中输入<http://localhost:8080/>后，自动跳转到<https://localhost:8443/>。