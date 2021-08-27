# 安装Ghost

## 前期准备

1.MySQL 5.7 或者 8（本机安装5.7）
2.Nodejs 不能通过nvm安装

升级系统安装包

```bash
sudo dnf -y update
sudo reboot
```

增加存储库

```bash
sudo dnf -y install epel-release
```

创建Ghost CLI 用户

```bash
sudo useradd ghostadmin
sudo passwd ghostadmin
sudo usermod -aG wheel ghostadmin
```

新建Ghost存放目录（必须安装在/var/www/{folder} ，其他地方会没有权限）

```bash
sudo mkdir -p /var/www/ghost
sudo chown ghostadmin:ghostadmin /var/www/ghost
sudo chmod 775 /var/www/ghost
```

切换新建的ghostadmin登录

```bash
sudo su - ghostadmin
cd /var/www/ghost
mkdir blog.example.com
cd blog.example.com
```

安装

```bash
ghost install
```

## 坑

以下操作需要切换到登录ghost的用户

ghost install 安装时 node版本检查不过，原来带的版本为node10
先通过以下命令卸载老版本的node10

```bash
yum remove nodejs npm -y
```

安装最新稳定版的node 14 (LTS)

```bash
dnf install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash - 
```

执行完后按照提示安装node

```bash
sudo dnf install nodejs 
或者sudo yum install -y nodejs
```

安装yarn

```bash
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
sudo yum install yarn
```

安装最新版本的ghost-cli

```bash
sudo yarn global add ghost-cli@latest
```

执行安装

```bash
ghost install
```
