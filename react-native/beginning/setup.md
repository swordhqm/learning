# Refs

中文文档 ：[https://reactnative.cn/docs/0.51/getting-started.html](https://reactnative.cn/docs/0.51/getting-started.html)

yarn文档 ：[https://yarnpkg.com/en/docs/cli/config](https://yarnpkg.com/en/docs/cli/config)

# 环境搭建

```bash
[kevin@localhost ~]$ yarn config list
yarn config v1.3.2
info yarn config
{ 'version-tag-prefix': 'v',
  'version-git-tag': true,
  'version-git-sign': false,
  'version-git-message': 'v%s',
  'init-version': '1.0.0',
  'init-license': 'MIT',
  'save-prefix': '^',
  'ignore-scripts': false,
  'ignore-optional': false,
  registry: 'https://registry.yarnpkg.com',
  'strict-ssl': true,
  'user-agent': 'yarn/1.3.2 npm/? node/v6.10.2 linux x64',
  lastUpdateCheck: 1513652438112 }
info npm config
{}

上面的registry 就是yarn 镜像源
通过
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
可以改为淘宝镜像源
```

```
react-native-cli

这个建议加入package.json 作为依赖包
```

```
安装jdk，jre
安装android studio，其实是为了sdk 和 模拟器

在SDK Platforms窗口中，选择Show Package Details，
然后在Android 6.0 (Marshmallow)中勾选Google APIs、Android SDK Platform 23、
Intel x86 Atom System Image、Intel x86 Atom_64 System Image
以及Google APIs Intel x86 Atom_64 System Image
```

```
ANDROID_HOME环境变量
确保ANDROID_HOME环境变量正确地指向了你安装的Android SDK的路径。

具体的做法是把下面的命令加入到~/.bashrc、~/.bash_profile文件中。如果你使用的是其他的shell，则选择对应的配置文件:

# 如果你不是通过Android Studio安装的sdk，则其路径可能不同，请自行确定清楚。
export ANDROID_HOME=~/Library/Android/sdk
然后使用下列命令使其立即生效（否则重启后才生效）：

source ~/.bash_profile
可以使用echo $ANDROID_HOME检查此变量是否已正确设置。
```



