# .bash\_\_profile 或者 .bash\_rc

```
.bash_profile
每个shell 会再加载一次
===================================================================
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

# User specific environment and startup programs
PATH=$PATH:$HOME/.local/bin:$HOME/bin:$(yarn global bin)

#export PATH=$PATH:~/node_modules/phantomjs/bin/
#export PATH=$PATH:~/node_modules/casperjs/bin/
export ANDROID_HOME=/home/kevin/android/android_tool

PATH=$ANDROID_HOME/platform-tools/:$PATH
export PATH=$PATH

# https://www.cnblogs.com/trying/archive/2013/06/07/3123577.html
# http://blog.csdn.net/jlnuboy/article/details/5290859?readlog
# https://www.cnblogs.com/ayanmw/p/3434028.html
# rpm -ql xx, 可以查看xx里含有的共享库
# 解决问题，比如libmysqlclient.so 找不到，添加共享库路径
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib64/mysql
```

```
.bashrc
登陆时加载一次
====================================================================
```



