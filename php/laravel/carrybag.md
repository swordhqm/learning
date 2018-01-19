# 构想

# laravel 配置

搭建laravel环境

`laravel new carrybag`

创建数据表

`create database carrybag character set utf8;`

利用laravel 自带的migration 初始化数据表

``` bash
[kevin@localhost carrybag]$ php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table

```
