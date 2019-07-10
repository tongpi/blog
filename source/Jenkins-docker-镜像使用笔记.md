

# 一、安装jenkins

1、创建docker volumn jenkins_home
2、创建jenkins容器

```
docker run -d --name jenkins  --restart=always -v jenkins_home:/var/jenkins_home -p 9080:8080 -p 50000:50000 jenkins/jenkins
```

```
docker run -d --name jenkins  --restart=always -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -p 9080:8080 -p 50000:50000 jenkins/jenkins
```

上面命令创建的jenkins不能执行docker命令

可以根据需要根据下面的Dockfile创建新的镜像

## 二、定制Jenkins镜像的过程

## 1、Dockerfile 

```shell
FROM jenkins/jenkins

USER root

#清除了基础镜像设置的源，切换成阿里云的jessie源
RUN echo '' > /etc/apt/sources.list.d/jessie-backports.list \
  && echo "deb http://mirrors.aliyun.com/debian jessie main contrib non-free" > /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian jessie-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian-security jessie/updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian stretch main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian stretch-proposed-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyun.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyuncs.com/debian stretch main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyuncs.com/debian stretch-proposed-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyuncs.com/debian stretch-updates main contrib non-free" >> /etc/apt/sources.list \
  && echo "deb http://mirrors.aliyuncs.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list
#更新源并安装缺少的包

RUN apt-get update && apt-get install -y build-essential gcc-6 gdb git valgrind \
    python3-pip python3-venv python-dev linux-perf google-perftools \
    zlib1g-dev lcov mc && \
    apt-get clean

RUN apt-get install -y libltdl7 \
  && apt-get install -y sudo \
  && apt-get install -y maven \
  && rm -rf /var/lib/apt/lists/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
ARG dockerGid=999

RUN echo "docker:x:${dockerGid}:jenkins" >> /etc/group \
USER jenkins
```

## 2、使用：

```shell
docker run -d --name jenkins_gds  --restart=always -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -p 9080:8080 -p 50000:50000 gds/jenkinswithgpp3
```

## 3、管理员密码

查看容器日志看管理员密码

## 4、登录http://192.168.3.69:9080
> 输入管理员密码
> 安装社区推荐的插件 （选择后会自动安装，这个过程需要10分钟左右）

## 5修改时区  
打开 【系统管理】->【脚本命令行】运行下面的命令:

```
System.setProperty('org.apache.commons.jelly.tags.fmt.timeZone', 'Asia/Shanghai')
```



## 6、配置全局工具
  JDK 和 MAVEN
## 7、配置本地maven仓库

```
docker cp /etc/maven/settings.xml jenkins:/
```

>   其中：/etc/maven/settings.xml是定制过的（使用本地nexus仓库 http://192.168.200.69:8081/nexus）
>

## 8 遇到的问题

>   执行shell脚本遇到权限不够。
>   原因：源代码中的shell脚本不是可执行的
>   解决方法一：bash ./abc.sh
>   解决方法二：git add --chmod=+x "./abc.sh"
>
>   不能发送邮件。163报554 DT:SPM 163 smtp11问题
>   原因：163的反垃圾邮件策略
>   解决：接收人中包含发件人自己  或者 使用客户端授权码替代密码

# 三、如何安装私有的镜像仓库

### 1、私有docker仓库  docker register的安装与使用

```shell
docker run -d -p 5000:5000 --name dockerregister --restart=always -v /opt/data/registry:/var/lib/registry registry

touch /opt/config.yml

docker run -d -p 5001:8080 --name registry-web  --restart=always --link dockerregister -v /opt/config.yml:/conf/config.yml:ro hyper/docker-registry-web
```

其中 /opt/config.yml的文件内容如下：

```yaml
registry:
  # Docker registry url
  url: http://dockerregister:5000/v2
  # Docker registry fqdn
  name: localhost:5000
  # To allow image delete, should be false
  readonly: false
  auth:
    # Disable authentication
    enabled: false
```

### 2、访问：

  http://192.168.200.224:5000/v2/_catalog 查看镜像清单
  http://192.168.200.224:5001 管理镜像

###   3、实验：

```shell
docker pull busybox                              #为了验证，读者可以拉取一个busybox镜像（因为体积小），进行实验。
docker tag busybox localhost:5000/bosybox:v1.0   #拉取最新的busybox镜像后，再给其打标为v1.0，准备发布到Registry中。
docker push localhost:5000/bosybox:v1.0          #发布busybox镜像的v1.0版本到本地docker仓库
docker pull localhost:5000/bosybox:v1.0          #从本地仓库获取bosybox:v1.0镜像
```

###    4、 实操：

```shell
docker tag gds/wso2is:o5.7.0 localhost:5000/gds/wso2is:o5.7.0
docker push localhost:5000/gds/wso2is:o5.7.0

docker tag gds/wso2is:5.7.0 localhost:5000/gds/wso2is:5.7.0
docker push localhost:5000/gds/wso2is:5.7.0
    
docker tag gds/wso2is:o5.7.0 localhost:5000/gds/wso2is:latest
docker push localhost:5000/gds/wso2is
```
# 后记

为了不丢失我做好的支持c++环境的jenkins镜像，我把它发布到dockerhub上了。

https://hub.docker.com/r/ten2net/jenkins

可以使用如下命令拉取：

```
docker pull ten2net/jenkins
```

发布的过程如下：

> 1、在dockerhub上注册账号或使用github账号登录dockerhub；
> 2、在你的docker服务器上构建镜像
> 3、docker login --username=ten2net
> 输入密码，看到登录成功提示后
> 4、规范化镜像标签：
> docker tag gds/jenkinswithgpp3 ten2net/jenkins
> 命令参数解释：docker tag 本地镜像 <dockerhub用户名>/镜像名
> 5、docker push ten2net/jenkins
> 6、浏览器登录dockerhub ，查看你的镜像