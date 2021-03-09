# Centos 7 安装easy monitor

## 1 安装 git(如果没有git请安装)

```bash
# yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
###以下命令可以不需要
# yum -y install git-core

# git --version
git version 1.8.3.1
```

## 2 利用nvm安装node

```bash
# curl https://raw.githubusercontent.com/creationix/nvm/v0.13.1/install.sh | bash
```


当出现下面错误时：
```bash
# nvm ls-remote
/usr/bin/which: no node in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
         N/A

```

由于墙的原因，请使用淘宝源

```bash
# export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/dist
```

### 安装node 14.16.0

```bash
# nvm install 14.16.0
# nvm use v14.16.0
```
## 3 安装MySQL 5.7

### 3.1 安装MySQL (注意安装MySQL8会出现异常问题)
参考网站 https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-centos-7


```bash
# yum -y install wget
# wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```

安装
```bash
# sudo rpm -ivh mysql57-community-release-el7-9.noarch.rpm
```

```bash
# sudo yum install mysql-server
```
### 3.2 启动MySQL

```bash
# sudo systemctl start mysqld
```

查看MySQL运行状态

```bash
# sudo systemctl status mysqld
```

### 3.3 配置MySQL

 生成临时密码，以备后用

```bash
# sudo grep 'temporary password' /var/log/mysqld.log
```
重新设置密码 & 其他配置（可以全选Y，或者根据自己需求更改,开启远程的忽略disallow remote root login)
```bash
# sudo mysql_secure_installation
```
***密码需要12位以上 包含数字，大小写字母和特殊字符***

### 3.4 测试MySQL

```bash
# mysqladmin -u root -p version
```

### 3.5 初始化 

1.新建数据库xprofiler_console,使用 [xprofiler-console/db/init.sql](https://github.com/X-Profiler/xprofiler-console/blob/master/db/init.sql)初始化
2.新建数据库xprofiler_logs,使用 [xtransit-manager/db/init.sql](https://github.com/X-Profiler/xtransit-manager/blob/master/db/init.sql)和[xtransit-manager/db/date.sql](https://github.com/X-Profiler/xtransit-manager/blob/master/db/date.sql)初始化

***Mysql 的日志数据会定期清理，默认保留 7 天，200G 数据盘约能承载 2000 个实例的接入***

## 4 安装Redis
参考 https://zhuanlan.zhihu.com/p/34527270
首先添加EPEL仓库，然后更新yum源

```bash
# sudo yum install epel-release
# sudo yum update
```

### 4.1 安装redis

```bash
# sudo yum -y install redis
```

### 4.2 启动redis

```bash
# sudo systemctl start redis
```

### 4.3 开机自启 

```bash
# systemctl enable redis.service
```

## 5 部署Easy Monitor

### 5.1 部署控制台

执行如下命令克隆控制台仓库：

```bash
# git clone https://github.com/X-Profiler/xprofiler-console
```

在 config 目录下新建 config.local.js ，添加如下配置内容：
```javascript
// xprofiler-console/config/config.local.js
'use strict';

module.exports = () => {
  const config = {};

  config.mysql = {
    app: true,
    agent: false,
    clients: {
      xprofiler_console: {
        host: '127.0.0.1',
        port: 3306,
        user: '****',
        password: '********',
        database: 'xprofiler_console',
      },
      xprofiler_logs: {
        host: '127.0.0.1',
        port: 3306,
        user: '****',
        password: '********',
        database: 'xprofiler_logs',
      },
    },
  };

  config.redis = {
    client: {
      sentinels: null,
      port: 6379,
      host: '127.0.0.1',
      password: '',
      db: 0,
    },
  };

  config.xprofilerConsole = 'http://127.0.0.1:8443';

  config.xtransitManager = 'http://127.0.0.1:8543';

  return config;
};
```
MySQL 的用户名和密码按照实际填写更改，
线上运行新建 config.prod.js文件，config.xprofilerConsole 端口改为http://127.0.0.1:7443，
config.xtransitManager 端口改为http://127.0.0.1:7543，
本地运行命令 npm run dev ,线上运行执行 npm run start ,线上关闭执行 npm run stop

### 5.2 部署采集器管理服务

执行如下命令克隆控制台仓库：

```bash
# git clone https://github.com/X-Profiler/xtransit-manager
```

在 config 目录下新建 config.local.js ，添加如下配置内容：
```javascript
// xtransit-manager/config/config.local.js
'use strict';

module.exports = () => {
  const config = {};

  config.mysql = {
    app: true,
    agent: false,
    clients: {
      xprofiler_console: {
        host: '127.0.0.1',
        port: 3306,
        user: '****',
        password: '********',
        database: 'xprofiler_console',
      },
      xprofiler_logs: {
        host: '127.0.0.1',
        port: 3306,
        user: '****',
        password: '********',
        database: 'xprofiler_logs',
      },
    },
  };

  config.redis = {
    client: {
      sentinels: null,
      port: 6379,
      host: '127.0.0.1',
      password: '',
      db: 0,
    },
  };

  config.mailer = {
    host: 'smtp.**.com',
    port: 25,
    secure: false,
    auth: {
      user: 'test@mail.com',
      pass: '********',
    },
  };

  config.xprofilerConsole = 'http://127.0.0.1:8443';

  return config;
};
```
MySQL 的用户名和密码按照实际填写更改，
线上运行新建 config.prod.js文件，config.xprofilerConsole 端口改为http://127.0.0.1:7443，
本地运行命令 npm run dev ,线上运行执行 npm run start ,线上关闭执行 npm run stop

### 5.3 部署采集器长连接服务

执行如下命令克隆控制台仓库：

```bash
# git clone https://github.com/X-Profiler/xtransit-server
```
在config目录下新建env文件,文件内容为

```bash
local
```

***线上环境是把env内容改为prod***

在 config 目录下新建 config.local.js ，添加如下配置内容：
```javascript
// xtransit-server/config/config.local.js
'use strict';

module.exports = () => {
  const config = {};

  config.xtransitManager = 'http://127.0.0.1:8543';

  return config;
};
```
MySQL 的用户名和密码按照实际填写更改，
线上运行新建 config.prod.js文件，config.xtransitManager 端口改为http://127.0.0.1:7543，
本地运行命令 npm run dev ,线上运行执行 npm run start ,线上关闭执行 npm run stop
***线上运行需要安装pm2***

安装pm2

```bash
# npm install -g pm2
```

查看log
```bash
# pm2 logs
```

## 遇到的问题

### 时间戳过期
需要同步网络时间，以下时间服务器如果不行，就更改其他的再试试
```bash
# ntpdate -u  pool.ntp.org

#中国
cn.ntp.org.cn
#中国香港
hk.ntp.org.cn
#美国
us.ntp.org.cn
```

