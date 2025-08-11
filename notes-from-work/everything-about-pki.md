## [译] 写给工程师：关于证书（certificate）和公钥基础设施（PKI）的一切（SmallStep, 2018）

http://arthurchiao.art/blog/everything-about-pki-zh/

Related JIRA Task: https://jira.tools.sap/browse/MACOMMT-17239

### 查看某域名使用的certificate

参考：[check-vertificate-openssl-linux](https://www.ssldragon.com/zh/blog/check-certificate-openssl-linux/#View-the-SSL-Certificate)

```bash
echo | openssl s_client -connect yourplc.com:443 2>/dev/null | opensssl x509
```

这个命令中的echo作用不太好理解，只能认为`s_client`需要接受一个输入流参数，即使是空，所以这里用管道代替。

上边的解释是很合理的，如果只输入这个命令：

```bash
openssl s_client -connect baidu.com:443
```

会进入一个新的session（类似`docker run <image> -it sh` 进入的，不会主动终止的session），这类session当然默认的输入流是STDIN！

#### 如何用CA证书给csr文件签名？

参考：[OpenSSL生成CA自签名根证书和颁发证书_openssl ca证书-CSDN博客](https://blog.csdn.net/qq_44734154/article/details/126167945)

```bash
# 分别生成CA的私钥和client私钥
openssl genrsa -out ca_private.key 2048
openssl genrsa -out rsa_pricate.key 2048

# 给ca创建自签名的证书 cert
openssl req -new -key ca_pricate.key -out ca_csr.csr
openssl x509 -req -in ca_csr.csr -out ca_cert.crt -signkey ca_pricate.key  #自签名

# 生成client的csr
openssl req -new -key rsa_private.key -out chengdong_csr.csr

# 用CA证书给client 签名。关键是 -CAkey -CA option，否则生成的证书是自签名的
openssl x509 -req -in chengdong_csr.csr -CAkey ca_pricate.key -CA ca_cert.crt -out chengd_test.crt
```

### Java Keystore 位置

一般在`$JAVA_HOME/lib/security/cacerts`，保存了trust store。

参考：[JAVA 导入信任证书 (Keytool 的使用)_java如何将第三方证书设置为可信任-CSDN博客](https://blog.csdn.net/ljskr/article/details/84570573)

```bash
keytool -list -v -keystore $JAVA_HOME/lib/security/cacerts
```

### 查看和更新openssl的Root CA

[查看linux根证书信任机构_linux查看已信任证书-CSDN博客](https://blog.csdn.net/u011238098/article/details/117848648)

```bash
awk -v cmd='openssl x509 -noout -subject' ' /BEGIN/{close(cmd)};{print | cmd}' < /etc/ssl/certs/ca-certificates.crt
```

关注文件`/etc/ssl/certs/ca-certificates.crt` :arrow_right: 证书列表文件，可以用`cat`查看（[List All Available Certificate Authority (CA) SSL Certificates](http://baeldung.com/linux/list-ca-ssl-certificates)）：

```
-----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
...
-----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
...
```

### 参考：

[OpenSSL的基本使用 - 魔幻小生 - 博客园](https://www.cnblogs.com/werr370/p/16385010.html)

[使用 openssl 生成证书 - 美码师 - 博客园](https://www.cnblogs.com/littleatp/p/5878763.html)

#### TIPS: 对重定向`&`符号的理解

[Shell重定向 ＆＞file、2＞&1、1＞&2 、/dev/null的区别_2>&1-CSDN博客](https://blog.csdn.net/u011630575/article/details/52151995)
