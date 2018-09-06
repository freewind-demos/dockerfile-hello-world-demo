Dockerfile Hello World Demo
===========================

首先安装好docker，然后执行：

```
docker build -t my-hello-world-docker .
```

由于我们例子中使用了`nginx`这个基础image，所以会先下载nginx image，在国内的话，时间可能会有点久。

如果已经下载过，则输出是这样的：

```
Sending build context to Docker daemon  22.53kB
Step 1/2 : FROM nginx
 ---> 06144b287844
Step 2/2 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
 ---> Using cache
 ---> 06a2a5145cd4
Successfully built 06a2a5145cd4
Successfully tagged my-hello-world-docker:latest
```

创建了一个名为`my-hello-world-docker:latest`的image。

运行
---

```
docker run -p 8080:80 my-hello-world-docker
```

其中`-p 8080:80`意思是，把`my-hello-world-docker`这个image启动之后的container的`8080`端口（即nginx的端口），映射到宿主机的`80`端口上。

在Ubuntu下，宿主机就是Ubuntu自己，所以访问`curl http://localhost:80`，就能得到回复`<h1>Hello, Docker!</h1>`。

而在Mac中，宿主机是docker-machine创建的虚拟机。如果当前使用的是`default`虚拟机，我们需要先运行`docker-machine env default`，得到的结果如：

```
set -gx DOCKER_TLS_VERIFY "1";
set -gx DOCKER_HOST "tcp://192.168.99.100:2376";
...
```

其中的`192.168.99.100`就是宿主机的ip地址。我们访问`curl http://192.168.99.100:80`，也能得到正确的回复`<h1>Hello, Docker!</h1>`