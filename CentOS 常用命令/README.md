# 常用命令

## 查看所有端口

```bash
netstat -ntlp
```

## 查看端口占用

比如查看80端口的占用情况

```bash
lsof -i tcp:80
```
