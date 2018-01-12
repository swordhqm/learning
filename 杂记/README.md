
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
LINUX ENVIRONMENT

###### Selenium自动化

pip install selenium

###### headless chrome 要求 version > 59
#### headless chrome 能干啥
https://www.jianshu.com/p/aec4b1216011



#### 安装chrome
1. Change to root user.
sudo -i 或者 su -
2. Enable Google YUM repository
Note: Google Chrome Linux version doesn’t get any 32-bit (x86) updates and 32-bit repo is also removed. If you have 32-bit version installed, then you can use it, but you can’t get security updates or other updates anymore.

Run following command (copy & paste all lines to console) to create /etc/yum.repos.d/google-chrome.repo file:


cat << EOF > /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome - \$basearch
baseurl=http://dl.google.com/linux/chrome/rpm/stable/\$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
EOF
3. Install Google Chrome with YUM
3.1 Install Google Chrome Stable Version

## Install Google Chrome Stable version ##
## Fedora 27/26/25/24 ##
dnf install google-chrome-stable

## CentOS/RHEL 7.4 ##
yum install google-chrome-stable
3.2 Install Google Chrome Beta Version

## Install Google Chrome Beta version ##
## Fedora 27/26/25/24 ##
dnf install google-chrome-beta

## CentOS/RHEL 7.4 ##
yum install google-chrome-beta
3.3 Install Google Chrome Unstable Version

## Install Google Chrome Unstable version ##
## Fedora 27/26/25/24 ##
dnf install google-chrome-unstable

## CentOS/RHEL 7.4 ##
yum install google-chrome-unstable
4. Note for Fedora 27/26/25 Wayland users
If you have problems, then fallback to Xorg with modifying /etc/gdm/custom.conf file:


# GDM configuration storage

[daemon]
# Uncomment the line below to force the login screen to use Xorg
WaylandEnable=false

[security]
...


#https://www.jianshu.com/p/aec4b1216011
#http://blog.csdn.net/offbye/article/details/52235139

#### 测试
.bash_profile
PATH=/opt/google/chrome/:$PATH

执行 chrome
另外如果是root 可能需要执行 chrome --no-sandbox

测试headless 特性
[kevin@localhost ~]$ chrome --headless --disable-gpu --remote-debugging-port=9222 https://www.baidu.com

DevTools listening on ws://127.0.0.1:9222/devtools/browser/2fc1cc4e-59f0-4009-8fb0-45b92ffc67c1

chrome --headless --remote-debugging-port=9222 https://www.baidu.com
chrome --headless --disable-gpu --dump-dom https://www.baidu.com
chrome --headless --disable-gpu --print-to-pdf https://www.baidu.com
chrome --headless --disable-gpu --screenshot https://www.baidu.com
chrome --headless --disable-gpu --screenshot --window-size=1280,1696 https://www.baidu.com
chrome --headless --disable-gpu --screenshot --window-size=412,732 https://www.baidu.com
把headless Chrome作为一个自动截图工具  -- 整个页面截图


###### 突破爬虫
#http://blog.csdn.net/offbye/article/details/52235139

```




