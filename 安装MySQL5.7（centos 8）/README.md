# 安装MySQL 5.7

## 准备工作

禁用MySQL默认的AppStream存储库

```bash
sudo dnf remove @mysql
sudo dnf module reset mysql && sudo dnf module disable mysql
```

添加仓库，由于centos 8没有MySQL的仓库，需要新建以下文件

```bash
sudo vi /etc/yum.repos.d/mysql-community.repo
```

内容

```bash
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=0

[mysql-connectors-community]
name=MySQL Connectors Community
baseurl=http://repo.mysql.com/yum/mysql-connectors-community/el/7/$basearch/
enabled=1
gpgcheck=0

[mysql-tools-community]
name=MySQL Tools Community
baseurl=http://repo.mysql.com/yum/mysql-tools-community/el/7/$basearch/
enabled=1
gpgcheck=0
```

禁用MySQL 8的存储库

```bash
sudo dnf config-manager --disable mysql80-community
```

启用MySQL 5.7的存储库

```bash
sudo dnf config-manager --enable mysql57-community
```

## 安装MySQL

```bash
sudo dnf install mysql-community-server
```

确认安装包为MySQL 5.7

```bash
rpm -qi mysql-community-server 

Name        : mysql-community-server
Version     : 5.7.35
Release     : 1.el7
Architecture: x86_64
Install Date: Thu 26 Aug 2021 03:07:09 PM CST
Group       : Applications/Databases
Size        : 800461996
License     : Copyright (c) 2000, 2021, Oracle and/or its affiliates. All rights reserved. Under GPLv2 license as shown in the Description field.
Signature   : DSA/SHA256, Tue 08 Jun 2021 05:15:58 PM CST, Key ID 8c718d3b5072e1f5
Source RPM  : mysql-community-5.7.35-1.el7.src.rpm
Build Date  : Mon 07 Jun 2021 10:00:11 PM CST
Build Host  : pb2-el7-08.appad3iad.mysql2iad.oraclevcn.com
Relocations : (not relocatable)
Packager    : MySQL Release Engineering <mysql-build@oss.oracle.com>
Vendor      : Oracle and/or its affiliates
URL         : http://www.mysql.com/
Summary     : A very fast and reliable SQL database server
Description :
The MySQL(TM) software delivers a very fast, multi-threaded, multi-user,
and robust SQL (Structured Query Language) database server. MySQL Server
is intended for mission-critical, heavy-load production systems as well
as for embedding into mass-deployed software. MySQL is a trademark of
Oracle and/or its affiliates

The MySQL software has Dual Licensing, which means you can use the MySQL
software free of charge under the GNU General Public License
(http://www.gnu.org/licenses/). You can also purchase commercial MySQL
licenses from Oracle and/or its affiliates if you do not wish to be bound by the terms of
the GPL. See the chapter "Licensing and Support" in the manual for
further info.

The MySQL web site (http://www.mysql.com/) provides the latest news and
information about the MySQL software.  Also please see the documentation
and the manual for more information.

This package includes the MySQL server binary as well as related utilities
to run and administer a MySQL server.
```

## 配置MySQL

### 开启服务

```bash
sudo systemctl enable --now mysqld.service
```

### 确认是否启动成功

```bash
sudo systemctl status mysqld
```

### 生成临时的随机密码

```bash
sudo grep 'A temporary password' /var/log/mysqld.log |tail -1
2021-08-26T07:48:00.980387Z 1 [Note] A temporary password is generated for root@localhost: u9hd*DFEirLs1
```

### 进入MySQl

```bash
mysql -uroot -p
```

输入之前生成的临时密码

### 修改登录密码

```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!（修改后的密码，注意必须包含大小写字母数字以及特殊字符并且长度不能少于8位,否则会报错）';
或者通过：mysql> set password for 'root'@'localhost'=password('MyNewPass4!');
或者通过：mysql> use mysql;
        mysql> update user set password=PASSWORD('MyNewPass5!') where user='root';
        mysql> flush privileges;
```

### 添加远程访问

```bash
mysql> GRANT ALL PRIVILEGES ON *.* TO 'zhangsan（用户名）'@'%' IDENTIFIED BY 'Zhangsan2018!（密码）' WITH GRANT OPTION;
# 或者直接将root权限修改为可以通过远程访问(但不推荐)
mysql> use mysql;
mysql> UPDATE user SET Host='%' WHERE User='root';
mysql> flush privileges;
```

### 设置默认编码为utf8mb4（mysql安装后默认不支持中文）

一开始使用了utf8，实际使用后发现不支持表情，后面花了点时间去调查，幸好数据没有影响。

```bash
vim /etc/my.cnf
# 进入文件后添加下面的配置即可
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_bin
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4

```

测试是否更改编码成功

```bash
# systemctl restart mysqld
# mysql -uroot -p
mysql> show variables like 'character%';
mysql> show variables like 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

```
