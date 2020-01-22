Dockerfile Hello World Demo
===========================

首先安装好docker，然后执行：

```
docker build -t my-hello-world-docker .
```

其中`-t`是`tag`的意思，表示将当前目录`.`中的Dockerfile build之后，给生成的image打个tag，
名为`my-hello-world-docker`，方便后面run的时候指定它。

不加`-t`也可以:

```
docker build .
```

但这样的话，我们就需要仔细看它的输出，它在最后会有如这样的信息：

```
Successfully built d0957ffdf8a2
```

我们在后面run的时候，就要写成`docker run d0957ffdf8a2`

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

其中`-p 8080:80`意思是，把宿主机的`8080`端口映射到`my-hello-world-docker` container的`80`端口（即nginx的端口）。

在Ubuntu下，宿主机就是Ubuntu自己，所以访问`curl http://localhost:8080`，就能得到回复`<h1>Hello, Docker!</h1>`。

而在Mac中，宿主机是docker-machine创建的虚拟机。如果当前使用的是`default`虚拟机，我们需要先运行`docker-machine ip`得到的其ip（此处为`192.168.99.100`），
然后访问`curl http://192.168.99.100:8080`，也能得到正确的回复`<h1>Hello, Docker!</h1>`
