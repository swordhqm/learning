# php

```
sudo dnf install php
sudo dnf install php-cli
sudo dnf install php-devel

安装pear
sudo dnf install php-pear

安装xdebug

https://xdebug.org/wizard.php

Download xdebug-2.6.0beta1.tgz
Unpack the downloaded file with tar -xvzf xdebug-2.6.0beta1.tgz
Run: cd xdebug-2.6.0beta1
Run: phpize (See the FAQ if you don't have phpize.

As part of its output it should show:

Configuring for:
...
Zend Module Api No:      20160303
Zend Extension Api No:   320160303
If it does not, you are using the wrong phpize. Please follow this FAQ entry and skip the next step.

Run: ./configure
Run: make
Run: cp modules/xdebug.so /usr/lib64/php/modules
Edit /etc/php.ini and add the line
zend_extension = /usr/lib64/php/modules/xdebug.so
Restart the webserver
```



