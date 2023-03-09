# 全局安装 pm2

npm i pm2 -g

然后进入到项目目录，执行

pm2 start npm --name "name" -- run start

# 项目开始

```sh
npm run build 

pm2 start npm -- run start
```

修改nuxt代码后在服务器执行

1.npm run build

2.pm2 restart 进程id

```sh
pm2 list # 查看当前正在运行的进程

pm2 start all # 启动所有应用

pm2 restart all # 重启所有应用

pm2 stop all # 停止所有的应用程序

pm2 delete all # 关闭并删除所有应用

pm2 logs # 控制台显示所有日志

pm2 start 0 # 启动 id为 0的指定应用程序

pm2 restart 0 # 重启 id为 0的指定应用程序

pm2 stop 0 # 停止 id为 0的指定应用程序

pm2 delete 0 # 删除 id为 0的指定应用程序

pm2 log 0 # 控制台显示编号为0的日志

pm2 show 0 # 查看执行编号为0的进程

pm2 monit jsyfShopNuxt # 监控名称为jsyfShopNuxt的进程
```



