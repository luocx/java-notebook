## 核心概念

### 镜像 image
镜像是一个只读的模板，带有创建Docker容器的说明。通常情况下，一个镜像是基于另一个镜像的，并进行了一些额外的定制。

### 容器 container
容器是一个镜像的可运行实例。

### 仓库 registry
Docker注册中心存储Docker镜像。可使用pull 命令拉取，push 推送镜像。

## 安装&配置
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

2. 通过 yum 安装
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

> 版本号 VERSION_STRING 可通过 yum list docker-ce --showduplicates | sort -r 命令获取

## 实战
1. 从仓库拉取并运行一个镜像
2. 为容器指定端口映射
3. 为容器指定数据卷映射
4. 从Dockerfile构建一个容器