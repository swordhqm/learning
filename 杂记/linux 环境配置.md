# .bash\__profile 或者 .bash_rc

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
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib64/mysql

```

```
.bashrc
登陆时加载一次
====================================================================

```



