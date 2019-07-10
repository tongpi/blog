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