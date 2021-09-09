# 安装MongoDB 5.0

## 安装MongoDB

### 配置yum

目前最新版本是5.0，建议点击以下链接查看是否有更新的版本  
[Configure the package management system](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/#configure-the-package-management-system-yum)

创建 */etc/yum.repos.d/mongodb-org-5.0.repo*文件,内容如下

```bash
[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc
```

### 安装

安装mongodb-org

```bash
sudo dnf install mongodb-org
```

期间会提示是否安装依赖的软件包以及 GPG key  一路 y 就可

### 启动

```bash
sudo systemctl enable mongod --now
```

### 验证是否安装成功

1.执行 mongo

```bash
mongo
MongoDB shell version v5.0.2
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("bca43711-aa6e-41ee-b21b-a4fb45f88ed2") }
MongoDB server version: 5.0.2
================
Warning: the "mongo" shell has been superseded by "mongosh",
which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
an upcoming release.
We recommend you begin using "mongosh".
For installation instructions, see
https://docs.mongodb.com/mongodb-shell/install/
================
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
 https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
 https://community.mongodb.com
---
The server generated these startup warnings when booting: 
        2021-09-07T17:21:24.774+08:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
        2021-09-07T17:21:25.506+08:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
        2021-09-07T17:21:25.506+08:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
>
```

2.执行 mongosh(官方目前推荐使用mongosh),后面都将使用mongosh进行操作

```bash
mongosh
Current Mongosh Log ID: 6139b8faff2bec8602461f66
Connecting to:  mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000
Using MongoDB:  5.0.2
Using Mongosh:  1.0.5

For mongosh info see: <https://docs.mongodb.com/mongodb-shell/>

Warning: Found ~/.mongorc.js, but not ~/.mongoshrc.js. ~/.mongorc.js will not be loaded.
  You may want to copy or rename ~/.mongorc.js to ~/.mongoshrc.js.
```

## 配置Mongo

配置文件在 */etc/mongod.conf*

```bash
vim /etc/mongod.conf
```

### 外网访问配置

```bash
#修改bind_ip为以下内容
bind_ip = 0.0.0.0
```

### 开启认证

```bash
security:
  authorization: enabled
```

### 配置完成

配置好后重新启动Mongo

```bash
sudo systemctl restart mongod
```

## 创建管理员

### 新增管理员账户

如果你启用了 MongoDB 用户鉴权，你需要创建一个管理员用户，可以访问并且管理 MongoDB 实例。

首先要进入mongo shell

```bash
mongosh
test> use admin
switched to db admin
admin> 

```

创建一个新用户，名称为UniDare,赋予userAdminAnyDatabase角色：

```bash
db.createUser(
  {
    user: "UniDare", 
    pwd: "<密码>", 
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
# 提示新增成功
Successfully added user: {
 "user" : "UniDare",
 "roles" : [
  {
   "role" : "userAdminAnyDatabase",
   "db" : "admin"
  }
 ]
}
```

退出Mongo Shell

```bash
exit
```

### 测试管理员账户是否可以连接

使用刚才创建的账户进行连接

```bash
mongosh -u UniDare -p --authenticationDatabase admin
Enter password: 
```

输入密码后，登录成功就可以了

## 基本操作

### 用户相关

查看当前库内已有用户

```bash
use admin
admin>show users
```

查看当前库内可用的角色，默认只有内建角色

```bash
admin>show roles
[
  {
    role: 'hostManager',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'read',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'root',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'clusterAdmin',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'readAnyDatabase',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'clusterManager',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'readWriteAnyDatabase',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'restore',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'clusterMonitor',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'userAdminAnyDatabase',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'enableSharding',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'readWrite',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'userAdmin',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'dbOwner',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'dbAdmin',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'dbAdminAnyDatabase',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: 'backup',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: '__queryableBackup',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  },
  {
    role: '__system',
    db: 'admin',
    isBuiltin: true,
    roles: [],
    inheritedRoles: []
  }
]
```

**角色说明：**

>1.数据库用户角色
read：允许用户读取指定数据库；  
readWrite：允许用户读写指定数据库；  

>2.数据库管理员角色
dbAdmin：允许用户进行索引创建、删除，查看统计或访问；  system.profile，但没有角色和用户管理的权限；  
userAdmin：提供了在当前数据库中创建和修改角色和用户的能力；  
dbOwner：提供对数据库执行任何操作的能力。这个角色组合了readWrite、dbAdmin和userAdmin角色授权的特权；

>3.集群管理角色
hostManager：提供监视和管理服务器的能力；  
clusterManager：在集群上提供管理和监视操作。可以访问配置和本地数据库，这些数据库分别用于分片和复制；  
clusterMonitor：提供对监控工具的只读访问；  
clusterAdmin：提供最强大的集群管理访问(副本集、分片、主从等)。组合了clusterManager、clusterMonitor和hostManager角色的能力，还提供了dropDatabase操作；  

>4.备份恢复角色
backup：提供备份数据所需的能力；  
restore： 提供使用mongorestore恢复数据的能力；  

>5. 所有数据库角色
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限；  
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限；  
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限；  
dbAdminAnyDataBase：只在admin数据库中可用，赋予用户所有数据库的adAdmin权限；  

>6. 超级用户角色
root：超级权限，只能针对admin库；  

>7. 内部角色
__system：提供对数据库中任何对象的任何操作的特权；  

创建用户，roles可以使用当前库内的角色，或者其他库内的角色

```bash
db.createUser({user: "root",pwd: "root",roles: [ { role: "root", db: "admin" } ]})
```

修改密码

```bash
db.changeUserPassword('root','newPassword');
```

已有用户新增和解除内建角色

```bash
db.grantRolesToUser('<用户名>', [{ role: '<内建角色>', db: 'admin' }])
db.revokeRolesFromUser( "<用户名>", [{ role: '<内建角色>', db: 'admin' }])
```

删除用户命令如下，虽然所有库的用户信息全存在admin的system.users中，删用户时还是要use <库名>才能删除

```bash
use db_name
db.dropUser("<username>")
```

注意点:创建用户时必须指定用户和数据库名，两者是绑定关系

### 创建数据库

因为开启了认证，所以此处需要新建用户

```bash
mongosh
testDB> use admin
switched to db admin
admin> db.auth('UniDare','<密码>')
{ ok: 1 }
admin> db.createUser({user: "testAdmin",pwd: "test123456",roles: [ { role: "dbOwner", db: "testDB" } ]})
{ ok: 1 }
admin> db.auth('testAdmin','test123456')
{ ok: 1 }
admin> use testDB
testDB> db.testDB.insertOne({"name":"test"})
{
  acknowledged: true,
  insertedId: ObjectId("6139d3555c092c757a48074b")
}
testDB> db.getCollection('testDB').find()
[
  { _id: ObjectId("6139d3555c092c757a48074b"), name: 'test' }
]
```

后期连接可以使用第三方的数据库可视化管理工具,比如Navicat

## 遇到的问题

1. 重启时报错

```bash
```bash
# sudo systemctl restart mongod
Job for mongod.service failed because the control process exited with error code.
See "systemctl status mongod.service" and "journalctl -xe" for details.
```

解决：我这边是/etc/mongod.conf配置错了，回复到之前的配置即可解决
