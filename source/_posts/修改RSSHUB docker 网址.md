修改RSSHUB docker 网址

1. 通过两种方式查看[container-id]的ENV中关于URL的值，

   ```
   docker exec -it [container-id] env
   ```

   ```
   docker inspect [container-id]
   ```

   [container-id]可通过

   ```
   docker ps
   ```

   2.首先关闭docker，`service docker stop`，修改`/var/lib/docker/containers/[container-id]/config.v2.json`里对应的环境变量URL，`service docker start`。

   3.修改nginx，`certbot certonly --standalone -d rss.example.com`注册新地址。`nano  /etc/nginx/conf.d/ttrss.conf `，将对应以前网址全部更换，包括`/etc/letsencrypt/live/`后面地址。然后使用 `nginx -t` 查看有无错误，没有错误后使用 `nginx -s reload` 重启 Nginx 服务

   4.

   ```
   docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
   ```

   查看 Docker 内所有容器的 IP，更换ttrss内插件IP地址

   