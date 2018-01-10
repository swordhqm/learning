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
yarn global add react-native-cli
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

```
安装watchman
git clone https://github.com/facebook/watchman.git
cd watchman
git checkout v4.5.0  # 这是本文发布时的最新版本，请自行选择更新的版本
./autogen.sh
./configure
make
sudo make install
```

```
将Android SDK的Tools目录添加到PATH变量中#
你可以把Android SDK的tools和platform-tools目录添加到PATH变量中，以便在终端中运行一些Android工具，
例如android avd或是adb logcat等。

在~/.bashrc或是~/.bash_profile文件中添加：

# 你的具体路径可能有所不同，请自行确认。
PATH="~/Android/Sdk/tools:~/Android/Sdk/platform-tools:${PATH}"
export PATH
```

```
react-native init MyPJ
```

# 个人经验

```
1. 当前环境 windows(host), vbox linux fedora (vboxlinux)
2. 想用android 模拟器
3. 直接在vboxlinux 启动模拟器 是不可行的，具体和虚拟化有关
4. 想在windows上再开个Hyper-V 用作android 虚拟机，但是发现 这样就和 vbox 冲突
5. vbox 创建虚拟机 用作 android 奇卡无比
6. genymotion 很顺利创建android 虚拟机并启动。但是公司环境下，android 虚拟机无法  
 境下的网络配置有关系，可能路由端用了vpn 之类或者规则之类。
7. 测试react native, 出现常见错误。
8. 添加
"scripts": {
    "android": "react-native run-android",
    "android_linux": "react-native bundle --platform android --dev false --entry-file index.js --bundle-output 
    android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res && react-native run-android"
  }
9. 发现运行时正常，但是hot-reload，也就是双击R，app无法通过dev server去热加载。
也就是每次改动都需要重新run，然后push app才能看到效果。
10. 测试很多次，发现只有在主机host端才可以执行adb reverse tcp:8081 tcp:8081.
11. 在windows 下搭建 java，nodejs，npm，yarn，android sdk。发现可以正常实现热加载了。
12. 编辑器使用的是vscode
```



