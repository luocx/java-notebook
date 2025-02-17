## 核心概念
### Docker-Desktop 跟 Docker-Engine 的区别是什么？
Docker-Desktop 是一个跨平台的一键安装程序，它包含了 Docker-Engine, 并提供了一套GUI来操作容器、应用跟镜像，更适合有图形界面的操作系统安装。

### 镜像 image
镜像是一个只读的模板，带有创建Docker容器的说明。通常情况下，一个镜像是基于另一个镜像的，并进行了一些额外的定制。

### 容器 container
容器是一个镜像的可运行实例，可以用docker cli来操作运行、停止、删除，简单来说就是一个沙箱进程，和主机所有其他进程隔离。

### 仓库 registry
Docker注册中心存储Docker镜像。可使用pull 命令拉取，push 推送镜像。

## CentOS 安装&配置
> 官方教程：https://docs.docker.com/engine/install/centos/  
  
> 仓库源  
> > 阿里云：http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo  
> > 清华大学镜像站：https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo  
> 
> 版本号 VERSION_STRING 可通过 yum list docker-ce --showduplicates | sort -r 命令获取

1. 删除旧版本docker
`sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
`

2. 通过 yum 设置仓库安装
	- `sudo yum install -y yum-utils`
	- `sudo yum-config-manager \
    	--add-repo \
    	https://download.docker.com/linux/centos/docker-ce.repo	
	   `
	- `sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io` 


	
3. 配置开机启动
	- `sudo systemctl enable docker.service`
	- `sudo systemctl enable containerd.service`

4. 配置远程访问



## 实战
1. 从仓库拉取并运行一个镜像
	- 拉取一个镜像 `docker pull nginx`
	- 启动容器 `docker run nginx`
	
2. 为容器指定端口映射
	- 将主机的`8080`端口映射到容器的`80`端口 `docker run -d --name dev-learn --publish 8080:80 nginx:latest`

3. 为容器指定数据卷映射
	- 运行一个nginx容器命名为<dev-test>，并指定一个volume 命名为<vol_nginx_app> `docker run -d --name dev-learn-2 --mount source=vol_nginx_app,target=/app nginx:latest `
	- 执行查看容器信息，可看到mount信息 `docker inspect dev-learn-2`

4. 从 Dockerfile 构建镜像

5. 从 GitHub 仓库构建镜像并上传到仓库