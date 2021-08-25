# 安装NPM 私库

## 系统环境

CentOS 8.2 64位(腾讯云)

## 前期准备

### 安装 Yarn（已安装Node)

1.设定yarn 的仓库

```bash
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
sudo rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg
```

2.安装yarn

```bash
sudo dnf install yarn
```

3.验证是否安装成功

```bash
yarn --version
```

### 安装 PM2

```bash
yarn global add pm2
```

## 全局安装verdaccio

```bash
yarn global add verdaccio
```

## 启动verdaccio

常规启动

```bash
[root@VM-0-6-centos ~]# verdaccio
 info --- Creating default config file in /root/.config/verdaccio/config.yaml
 warn --- config file  - /root/.config/verdaccio/config.yaml
(node:563979) Warning: Verdaccio doesn't need superuser privileges. don't run it under root
(Use `node --trace-warnings ...` to show where the warning was created)
(node:563979) Warning: Verdaccio doesn't need superuser privileges. don't run it under root
 warn --- Plugin successfully loaded: verdaccio-htpasswd
 warn --- Plugin successfully loaded: verdaccio-audit
 warn --- http address - http://localhost:4873/ - verdaccio/5.1.3

```

PM2启动

```bash
yarn global add verdaccio
```

### 修改配置

/root/.config/verdaccio/config.yaml

增加

```yaml
# 监听的端口 ,重点, 不配置这个,只能本机能访问
listen: 0.0.0.0:4873
```
