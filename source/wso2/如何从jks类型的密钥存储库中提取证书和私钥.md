[TOC]

## 一、如何从jks类型的密钥存储库中提取证书和私钥

WSO2产品的密钥对存储在wso2carbon.jks密钥存储库中。 本中我们将看到如何使用keytool和openssl从wso2cabon.jks库中提取出PEM格式的公钥证书和私钥。

我们知道，使用keytool可以从jks类型的密钥存储库直接导出二进制的公钥：

```
keytool -export -alias wso2carbon -file  wso2carbon.crt -keystore wso2carbon.jks
```

然后使用openssl转换二进制公钥为PEM格式：

```
openssl x509 -inform der -in wso2carbon.crt -out wso2carbon.pem
```

也可以不用openssl而直接使用keytool直接导出PEM格式的公钥证书：

```
keytool -exportcert -rfc -alias wso2carbon -keystore wso2carbon.jks -storepass wso2carbon -file wso2carbon_pub.pem
```

但导出私钥比较麻烦一些，下面我们一步步来做：

### 1、使用keytool转换JKS类型的密钥存储库为PCKS12类型的密钥存储库

```
keytool -importkeystore -srckeystore wso2carbon.jks -destkeystore wso2carbon.p12 -srcstoretype JKS -deststoretype PKCS12 -srcstorepass wso2carbon -deststorepass destpass -srcalias wso2carbon -destalias myalias -srckeypass wso2carbon -destkeypass destpass -noprompt
```

这将创建PKCS12类型的密钥存储库文件wso2carbon.p12

### 2、使用openssl从wso2carbon.p12库中提取PEM格式的公钥证书

```shell
openssl pkcs12 -in wso2carbon.p12  -out wso2carbon_pub.pem
```

这将创建包含公钥的wso2carbon_pub.pem文件

```
`-----BEGIN CERTIFICATE-----MIICNTCCAZ6gAwIBAgIES343gjANBgkqhkiG9w0BAQUFADBVMQswCQYDVQQGEwJVUzELMAkGA1UECAwCQ0ExFjAUBgNVBAcMDU1vdW50YWluIFZpZXcxDTALBgNVBAoMBFdTTzIxEjAQBgNVBAMMCWxvY2FsaG9zdDAeFw0xMDAyMTkwNzAyMjZaFw0zNTAyMTMwNzAyMjZaMFUxCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJDQTEWMBQGA1UEBwwNTW91bnRhaW4gVmlldzENMAsGA1UECgwEV1NPMjESMBAGA1UEAwwJbG9jYWxob3N0MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCUp/oV1vWc8/TkQSiAvTousMzOM4asB2iltr2QKozni5aVFu818MpOLZIr8LMnTzWllJvvaA5RAAdpbECb+48FjbBe0hseUdN5HpwvnH/DW8ZccGvk53I6Orq7hLCv1ZHtuOCokghz/ATrhyPq+QktMfXnRS4HrKGJTzxaCcU7OQIDAQABoxIwEDAOBgNVHQ8BAf8EBAMCBPAwDQYJKoZIhvcNAQEFBQADgYEAW5wPR7cr1LAdq+IrR44iQlRG5ITCZXY9hI0PygLP2rHANh+PYfTmxbuOnykNGyhM6FjFLbW2uZHQTY1jMrPprjOrmyK5sjJRO4d1DeGHT/YnIjs9JogRKv4XHECwLtIVdAbIdWHEtVZJyMSktcyysFcvuhPQK8Qc/E/Wq8uHSCo=
-----END CERTIFICATE-----`
```

### 3、使用openssl从wso2carbon.p12库中提取PEM格式的私钥

```
openssl pkcs12 -in wso2carbon.p12 -nocerts -out wso2carbon.key -passin pass:destpass
```

一旦执行这个命令你会被要求一个秘钥口令。 私钥将由这个秘钥口令执行安全加密。

加密过的私钥（wso2carbon.key）看起来如下：

```
`-----BEGIN ENCRYPTED PRIVATE KEY-----MIICxjBABgkqhkiG9w0BBQ0wMzAbBgkqhkiG9w0BBQwwDgQIUOibMhIr4VUCAggAMBQGCCqGSIb3DQMHBAhUdc2GGS3L8gSCAoAs/RzzpoW8G3AHKQPAELcTzAic6nDxlccNoAzbO6yAdfodYqsjFYICwVg9fYMo7hDF0C3W/TkT1/Wk4ettmWO4J7dPVpeAuUW1kD9HmzRlizcg8+Ot0wO+RV1gTiB73uB2pPllcZmXSVxc9JDWWUzO+DDv+gbNdtqq+8n9so7NIgMHy+zNC0hVL4IWRevAcN6zzu4qLKj/YdwrjNGjN15wuLZZgYnGBd4zfil+nH9bjrG1hGnei6F8GDbZ79KspL0b99rZKkScU8ND0dyZRm06ItXTD2RvemwFAV0rBfm8IVKdRQ0v2z5hHl808okX6d4vgensc1QYoEG9LuDpBcZRPXizdoSu89DVxWJgtiad5mPc67VHPBQMU/tO1yzZJuuw9bZEITbkWQNB0XMEYkIiCEFJVjF6ifLUKj35x6a8uJvbZTPJZcMbdGfWtwP/RnXxmjK5GF2e+JmAjZl2VuiBftF5IAthCMh/0qarIlpwzXB+My1o1dGVCk9TyQ0UO/wQD+uVRA29pmX8NuyHPx9B6v+KUXvJGZwLYeI1DAGG/H64HSRaiUihSBDC1fY5rX24pQAgPn5fc90ENAzIn+a9rQOMa6azLkJQH7yC1Gkz1npIoIZAaYvcpJ2X9/CltPzf7oK/ZQ2BHI1bcYvyALO/QW1SnB1MyjRBtjcfoSpDCQo8BuXzyNOjjTDxn6UbVMqlbzuiU4k5Gqlqslji1vvttKerQqwnNTY4Xf/4Nhsxp/Tf/fOFYsLocMwhwjJdMpEBz9Co6cKZvkyXMYxoiHoFLeoB2GorbHmHJ/xWYCQHSVQvF8e2QnjmQHBTwD7+EbaRgPZCK2EJ3tFk7aA4EykC
-----END ENCRYPTED PRIVATE KEY-----`
```

### 4、得到不加密的私钥（从加密私钥中删除秘钥口令）

```
openssl rsa -in wso2carbon.key -out wso2carbon_nopass.key
```

现在wso2carbon_nopass.key就包含了不加密的私钥，看起来如下：`

```
-----BEGIN RSA PRIVATE KEY-----MIICXAIBAAKBgQCUp/oV1vWc8/TkQSiAvTousMzOM4asB2iltr2QKozni5aVFu818MpOLZIr8LMnTzWllJvvaA5RAAdpbECb+48FjbBe0hseUdN5HpwvnH/DW8ZccGvk53I6Orq7hLCv1ZHtuOCokghz/ATrhyPq+QktMfXnRS4HrKGJTzxaCcU7OQIDAQABAoGAS/+ooju4a9po67zIGTEkqrQmsJC1HAPZo0bOmQK38LRzcps8Bmao9tjjbuVqogEj2xgjtHyNPSn3oBUA3v33usJ6YqwVrWsC6FwmZhq8Avsf94qm4hiTHe1AdxWmZGTs1eSYc6JnPIp0iVjHEfssIlGN+7LX1Q6kdbCf482dTnUCQQDvLwmtjlUASW84zL5PEnNCorlcJ8qjGKlbcur2Lrn3vSCyX4cIWMxPNsCGvS2IO1Ctmz7yssnobhX6iOaFOZVPAkEAnxuSwN4Kdw9Zku8cc7aifnJuEjzuEemM1cmwGSqilL0xUijVeeyqfyy+1o7VFDa/nWPmmEZSqPNR6utcvLQU9wJAIycmpPtmQsSINDDjR3vOtNx1obW3coENYwNgxQ3ZBzAkvhKMJg3m+T1yzlq/dmZBVUKb3c+pHSAQ2uGD/9CWwQJAVRy46ndc/ce2UQWcIMJINoAcJaF2cRqQfiTAERZfllWGtr6lQ+24XwOeqsQJdCC9bAJu7nJf8YUIAzUYjNGAjQJBAKskkwcdhzvVcs7llm3+wWEzbMXzvNBmkZGRhDX6jtUIJ4U9RTHivqMeym4vp0mggaD4zc8qzG1NPDOp0p5AxBg=
-----END RSA PRIVATE KEY-----
```

## 二、秘钥对管理常用命令

### 1、生成密钥对到jks类型的秘钥存储库中

```
keytool -genkey -keyalg RSA -alias is.wd.mtn -validity 3650 -keysize 2048 -dname "EMAILADDRESS=admin@wd.mtn, CN=is.wd.mtn, O=长城数字, L=西安, S=陕西, C=CN, OU=安全中心"  -keystore wso2carbon.jks -storepass wso2carbon
```

执行成功会创建（若不存在）一个 wso2carbon.jks文件，其中包含一个密钥对（证书是自签名的）。

### 2、导出公钥证书到证书存储库中

```
keytool -export -alias is.wd.mtn -file is.wd.mtn.crt -keystore wso2carbon.jks -storepass wso2carbon
```

> 因为“keytool”不支持密钥导出功能

### 3、导入证书到证书存储库中

```
keytool -import -v -trustcacerts -noprompt -alias is.wd.mtn -file is.wd.mtn.crt -keystore %JAVA_HOME%/jre/lib/security/cacerts -keypass changeit -storepass changeit
```

> %JAVA_HOME%/jre/lib/security/cacerts中的证书一般是客户端应用来使用

### 4、查看证书存储库中别名为is.wc.mtn的证书

```
keytool -list -v -alias  is.wc.mtn -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
```

### 5、删除证书存储库中别名为wso2carbon的证书

```
keytool -delete -alias  wso2carbon -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
```

### 6、打印证书内容到控制台

```
keytool -printcert -v -file mydomain.crt
```

### 7、打印证书存储库中的全部证书到控制台

```
keytool -list -v -keystore keystore.jks
```

### 8、为现有Java密钥存储区中的别名is.wd.mtn生成证书签名请求（CSR）

```
keytool -certreq -alias is.wd.mtn -keystore keystore.jks -file is.wd.mtn.csr
```

### 9、修改证书存储库的密码

```
keytool -storepasswd -new new_storepass -keystore keystore.jks
```

### 10、查看受信任的CA证书

```
keytool -list -v -keystore $JAVA_HOME/jre/lib/security/cacerts
```

## 三、参考资源

- 使用windows的mmc管理证书的方法：

- ```
  Win+R > mmc.exe > OK > File > Add/Remove 管理单元 > Certificates > Add > Computer account > Next > Local computer > Finish > OK
  ```

  证书格式在线转换器：https://decoder.link/converter

- SSL证书格式转换工具：https://www.chinassl.net/ssltools/convert-ssl.html

- 常用的Java Keytool Keystore命令：https://www.chinassl.net/ssltools/keytool-commands.html

- [How can I find the private key for my SSL certificate?](https://helpdesk.ssls.com/hc/en-us/articles/360007608912-How-can-I-find-the-private-key-for-my-SSL-certificate-)

- https://www.namecheap.com/support/knowledgebase/article.aspx/9834/69/how-can-i-find-the-private-key-for-my-ssl-certificate

- [How to extract Certificate and private key from jks ](https://jenananthan.wordpress.com/2016/04/16/how-to-extract-certificate-and-private-key-from-jks/)  这是一个wso2的爱好者写的博客

- [Extracting-Certificate-crt-and-PrivateKey-key-from-a-Certificate-pfx-File](https://helpcenter.gsx.com/hc/en-us/articles/115015887447-Extracting-Certificate-crt-and-PrivateKey-key-from-a-Certificate-pfx-File)



四、我的实验脚本

```shell
REM 给is.wc.mtn生成证书
keytool -genkey -keyalg RSA -alias is.wc.mtn -validity 3650 -keysize 2048 -dname "EMAILADDRESS=admin@wc.mtn, CN=is.wc.mtn, O=长城数字, L=西安, S=陕西, C=CN, OU=安全中心"  -keystore wso2carbon.jks -storepass wso2carbon
keytool -export -alias is.wc.mtn -file is.wc.mtn.crt -keystore wso2carbon.jks -storepass wso2carbon
keytool -delete -alias is.wc.mtn -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias is.wc.mtn -file is.wc.mtn.crt -keystore %JAVA_HOME%/jre/lib/security/cacerts -keypass changeit -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias is.wc.mtn -file is.wc.mtn.crt -keystore client-truststore.jks -keypass wso2carbon -storepass wso2carbon

keytool -exportcert -rfc -alias is.wc.mtn -keystore wso2carbon.jks -storepass wso2carbon -file is.wc.mtn.pem

REM 给is.wd.mtn生成证书
keytool -genkey -keyalg RSA -alias is.wd.mtn -validity 3650 -keysize 2048 -dname "EMAILADDRESS=admin@wd.mtn, CN=is.wd.mtn, O=长城数字, L=西安, S=陕西, C=CN, OU=安全中心"  -keystore wso2carbon.jks -storepass wso2carbon
keytool -export -alias is.wd.mtn -file is.wd.mtn.crt -keystore wso2carbon.jks -storepass wso2carbon
keytool -delete -alias is.wd.mtn -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias is.wd.mtn -file is.wd.mtn.crt -keystore %JAVA_HOME%/jre/lib/security/cacerts -keypass changeit -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias is.wd.mtn -file is.wd.mtn.crt -keystore client-truststore.jks -keypass wso2carbon -storepass wso2carbon

keytool -exportcert -rfc -alias is.wd.mtn -keystore wso2carbon.jks -storepass wso2carbon -file is.wd.mtn.pem

REM 给cas.wc.mtn生成证书
keytool -genkey -keyalg RSA -alias cas.wc.mtn -validity 3650 -keysize 2048 -dname "EMAILADDRESS=admin@wd.mtn, CN=cas.wc.mtn, O=长城数字, L=西安, S=陕西, C=CN, OU=安全中心"  -keystore wso2carbon.jks -storepass wso2carbon
keytool -export -alias cas.wc.mtn -file cas.wc.mtn.crt -keystore wso2carbon.jks -storepass wso2carbon
keytool -delete -alias cas.wc.mtn -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias cas.wc.mtn -file cas.wc.mtn.crt -keystore %JAVA_HOME%/jre/lib/security/cacerts -keypass changeit -storepass changeit

REM 给cas.wd.mtn生成证书
keytool -genkey -keyalg RSA -alias cas.wd.mtn -validity 3650 -keysize 2048 -dname "EMAILADDRESS=admin@wd.mtn, CN=cas.wd.mtn, O=长城数字, L=西安, S=陕西, C=CN, OU=安全中心"  -keystore wso2carbon.jks -storepass wso2carbon
keytool -export -alias cas.wd.mtn -file cas.wd.mtn.crt -keystore wso2carbon.jks -storepass wso2carbon
keytool -delete -alias cas.wd.mtn -keystore %JAVA_HOME%/jre/lib/security/cacerts -storepass changeit
keytool -import -v -trustcacerts -noprompt -alias cas.wd.mtn -file cas.wd.mtn.crt -keystore %JAVA_HOME%/jre/lib/security/cacerts -keypass changeit -storepass changeit

```

