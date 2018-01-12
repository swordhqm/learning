
---

http 代理 参见gost

目前知道 透明http代理国内使用会出现 connection reset

[https://github.com/ginuerzh/gost](https://github.com/ginuerzh/gost)

[https://imququ.com/post/web-proxy.html](https://imququ.com/post/web-proxy.html)

---

```
root@PlayfulAliceblue-VM:/etc/v2ray# openssl genrsa -des3 -out v2ray.key 2048
密码12345

root@PlayfulAliceblue-VM:/etc/v2ray# openssl rsa -in v2ray.key -out v2ray.key.nopassword
Enter pass phrase for v2ray.key:
writing RSA key
root@PlayfulAliceblue-VM:/etc/v2ray# ls
config.json  v2ray.key  v2ray.key.nopassword
root@PlayfulAliceblue-VM:/etc/v2ray# vim v2ray.key.nopassword 
root@PlayfulAliceblue-VM:/etc/v2ray# openssl req -new -key v2ray.key.nopassword -out v2ray.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
root@PlayfulAliceblue-VM:/etc/v2ray# 
root@PlayfulAliceblue-VM:/etc/v2ray# openssl x509 -req -days 3650 -in v2ray.csr -signkey v2ray.key.nopassword -out v2ray.crt
Signature ok
subject=/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd
Getting Private key
```
---

```
#sel
#headless chrome
#https://www.jianshu.com/p/aec4b1216011
http://blog.csdn.net/offbye/article/details/52235139
```



