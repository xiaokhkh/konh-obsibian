#### 上传镜像到Docker Hub
1. 在hub.docker.io创建仓库；
2. 
``` sh
docker build --target "" -f "" -t username/imagesname:tagname '.'
docker login
docker push username/imagesname:tagname
```


Docker →笔记

```bash
sudo wget -qO- <https://get.docker.com> | sh
//安装docker -q 将wget以pipe形式流到下一个命令 O- 标准流输出 |sh 将数据以shell命令的形式运行
sudo usermod -aG docker username
//将当前用户加入docker权限组
```

```bash
docker pull //从远端服务器获取Image
docker build //创建Image
docker images //列出现有Image
docker run //运行Container
docker ps //列出正在运行的Container

docker rm //删除Container
docker rmi //删除Image
docker cp //host拷贝文件到Contaner
docker commit //提交变动产生新的Image
```

```bash
DockerFile
```