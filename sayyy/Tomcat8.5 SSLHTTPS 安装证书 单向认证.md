# Tomcat8.5 SSL/HTTPS 安装证书 单向认证

2018年06月12日 14:05:16

阅读数：34

## 准备

1、为localhost生成证书库。[生成证书参考这里](https://blog.csdn.net/sayyy/article/details/80662849) 
证书库名称：localhost.jks 
证书库格式：JKS 
证书库密码：localhost 
证书别名：localhost 
2、tomcat 8.5.24 
3、JDK 1.8

## 步骤

1、将localhost.jks证书库放到tomcat的conf文件夹中。 
2、修改tomcat的 server.xml配置文件。在server.xml中 找到下面被注释的这段：

```xml
    <!--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/localhost-rsa.jks"
                         type="RSA" />
        </SSLHostConfig>
    </Connector>
    -->
```

干掉注释，并将内容改为：

```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true" >
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="${catalina.home}/conf/localhost.jks"
                         certificateKeystorePassword="localhost"
                         type="RSA"  />
        </SSLHostConfig>
    </Connector>
```

3、启动tomcat。 
4、测试tomcat是否启用https/SSL 
在浏览器中打开地址：[https://localhost:8443](https://localhost:8443/)

## 其它

Tomcat 8.5 以上版本支持 SNI （ 同IP可以安装多个证书 ）, 至少 jre 7 以上版本

```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true" defaultSSLHostConfigName="localhost">
        <SSLHostConfig hostName="localhost">
            <Certificate certificateKeystoreFile="${catalina.home}/conf/localhost.jks"
                         certificateKeystorePassword="localhost"
                         type="RSA"  />
        </SSLHostConfig>
        // 其他站点复制多个 SSLHostConfig
    </Connector>
```