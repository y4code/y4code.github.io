+++
date = "2019-12-16T16:00:00.000Z"
title = "在 Docker 中运行 Nginx，同时 SSL/HTTPS 认证"
image =  "images/featured-post/post-5.jpg"
+++
## Docker 单服务部署
我用的是 [nginxconfig.io](nginxconfig.io) 的 服务，似乎现在被 DigitalOcean 给蓝金黄了，点击链接会跳转到 DigitalOcean 的网站，Anyway，服务是很好用的，你要做是简单填写几项，网站会自动生成完整可用的 Nginx 配置

	⚠️ 需要注意的是

生成的配置需要和原有的配置合并才可用，即覆盖配置

在参照 nginxconfig.io 的步骤都结束，并且你的网站可以通过https 正常访问时，然后

测试网络

`docker run --rm --name  mynginx -p 80:80 -d nginx`

测试没有问题后

`docker run --rm --name  mynginx -v /etc/nginx:/etc/nginx -v /etc/letsencrypt:/etc/letsencrypt -v /var/www/_letsencrypt:/var/www/_letsencrypt -v /var/www/STATIC_FOlDER:/var/www/YOUR_DOMAIN -p 80:80 -p 443:443 -d nginx`

就 OK 了 
但是涉及到 Docker 容器之间的通信就比较费劲，参考了一圈之后，我决定使用 Docker 自带的 Docker Network 来通信

## 使用 GitLab CI/CD Docker Swarm 部署

我打算在虚拟机上部署三个服务
	
	1. 静态资源
	2. Go （Gin）写的后端服务
	3. Nginx

我想要实现的效果是访问网站根路径会 定向到静态资源，访问根域加 /api/ 会转到 Go 服务中

所以我是这样做的，Docker 启两个容器，Nginx镜像 和 Go 代码打包的镜像，使用 Docker Swarm 的方式启动起来，并且分配一个共同网络

**最关键的一步，Nginx 配置的反向代理配置为**

```
/api/ => http://GO_CONTAINER_NAME:8080
```

GitLab 的 CI/CD 及其好用，在我苦索免费 Docker 私有 Registry 时候看到了 GitLab 的仓库 Registry，每个 repo 都可以有一个 公/私 有镜像库，我哭廖

贴一下过程中常用命令

```
docker container cp mynginx:/etc/nginx .

docker login -u "GITLAB_USERNAME" -p "GITLAB_PASSWORD" registry.gitlab.com

docker build -t registry.gitlab.com/GITLAB_USERNAME/REPO_NAME .

docker push registry.gitlab.com/GITLAB_USERNAME/REPO_NAME

docker swarm leave -f


docker rmi registry.gitlab.com/GITLAB_USERNAME/REPO_NAME

docker pull registry.gitlab.com/GITLAB_USERNAME/REPO_NAME

docker swarm init

docker stack deploy -c swarm.yml cluster

```


