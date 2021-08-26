# 安装Nginx

## 安装

1.执行以下命令

```bash
sudo yum install nginx
```

2.启动相关服务

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

3.查看运行状态

```bash
sudo systemctl status nginx
```

## Nginx配置文件的结构说明

所有Nginx配置文件都位于/etc/nginx/目录中。
Nginx的主要配置文件是/etc/nginx/nginx.conf。
为每个域创建一个单独的配置文件使服务器易于维护。
Nginx服务器阻止文件必须以结尾.conf并存储在/etc/nginx/conf.d目录中。您可以根据需要拥有任意数量的服务器块。
遵循标准命名约定是一个好习惯。例如，如果域名是，mydomain.com则配置文件应命名为mydomain.com.conf
如果在域服务器块中使用可重复的配置段，则最好将这些段重构为片段。
Nginx日志文件（access.log和error.log）位于/var/log/nginx/目录中。建议有不同access和error日志文件每个服务器模块。
您可以将域文档的根目录设置为所需的任何位置。webroot的最常见位置包括：
/home/<user_name>/<site_name>
/var/www/<site_name>
/var/www/html/<site_name>
/opt/<site_name>
/usr/share/nginx/html
