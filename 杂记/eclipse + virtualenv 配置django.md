# virtualenv

[https://virtualenv.pypa.io/en/stable/userguide/](https://virtualenv.pypa.io/en/stable/userguide/)

参加virtualenv 的user guide

```
virutalenv oscar
source oscar/bin/activate

pip install xxx
```

# 搭建oscar

oscar 目录结构

```
(oscar) [kevin@localhost oscar_shop]$ ll
总用量 16
drwxrwxr-x 9 kevin kevin 4096 2月  12 12:33 django-oscar
drwxrwxr-x 7 kevin kevin 4096 2月  12 12:20 django-oscar-accounts
drwxrwxr-x 7 kevin kevin 4096 2月  12 12:40 frobshop
drwxrwxr-x 6 kevin kevin 4096 2月   3 08:56 oscar
```

正常情况下需要

```
source oscar/bin/activate
cd frobshop
python manage.py runserver 0.0.0.0:8000
```

# 配置eclipse + virtualenv + django

* 导入oscar\_shop

这部分比较麻烦，需要先创建一个空的project

![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-1.png)

然后import from File System

选择需要的文件夹，import到project

![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-2.png)

* 配置interpeter

这里需要找到virtualenv 环境下的python作为编译器  
![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-3.png)

* 运行环境配置

![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-4.png)![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-5.png)![](/assets/img/01b6f28f-68eb-4241-8a9a-9372c2d810dc-6.png)

# TAG

01b6f28f-68eb-4241-8a9a-9372c2d810dc

