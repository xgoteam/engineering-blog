---
title: Docker入门
categories:
  - 容器
tags:
  - 虚拟化
  - 容器
date: 2019-03-31 15:29:20
---

# Docker是什么

Docker 起初是 dotCloud 公司创始人 Solomon Hykes 在法国的时候发起的一项公司内部项目，Docker是基于 dotCloud 公司多年云服务技术的一次革新，在 2013 年 3 月以 Apache 2.0 授权协议进行开源，其项目主要代码在 GitHub 上进行维护，自从Docker 开源之后，就一直受到了广泛讨论和关注。

![](https://user-gold-cdn.xitu.io/2018/8/29/16584c8b4d7dc120?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Docker 进行开发实现使用的是Google 公司推出的 Go 语言，对进程进行封装隔离是基于 Linux 内核的 cgroup，namespace，以及 AUFS 类的 Union FS 等技术，这属于操作系统层面的虚拟化技术。因为隔离的进程独立于宿主与其它隔离的进程，所以也称其为容器（后文会对“容器”的概念进行详细介绍）。Docker 在容器的基础上，进行了进一步的封装，从网络互联、文件系统到进程隔离等，大大地简化了容器的创建和维护，让 Docker 技术比虚拟机技术更加轻便、快捷。

以下两张图片对比了 Docker 与传统虚拟化方式的不同之处。Docker 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，没有进行硬件虚拟；而传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程。因此容器要比传统虚拟机更为轻便。

![](https://user-gold-cdn.xitu.io/2018/8/29/16584c95d9cc4367?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

*传统虚拟化*

![](https://user-gold-cdn.xitu.io/2018/8/29/16584c994af8544e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

*Docker*

# 为什么要使用 Docker？

Docker是一种新兴的虚拟化方式，跟传统的虚拟化方式相比具有众多优势。

## 系统资源利用更高效

因为容器不需要进行硬件虚拟以及运行完整操作系统等额外开销，所以Docker 对系统资源的利用率更高。

## 启动时间更快速

Docker 容器应用由于直接运行于宿主内核，无需启动完整的操作系统，因此可以做到秒级、甚至毫秒级的启动时间。极大地节省了开发、测试，部署的时间。

## 运行环境一致性

开发过程中比较常见的问题就是环境一致性问题。因为开发环境、测试环境、生产环境不一致，导致有些 bug 并未在开发过程中被发现。而 Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性，从而不会再出现 「这段代码在我机器上没问题啊」 这类问题。

## 持续交付与部署

对开发和运维人员来说，最希望的就是一次创建或配置，可以在任意地方正常运行。使用 Docker 可以通过定制应用镜像来实现持续集成、持续交付、部署。开发人员可以通过 Dockerfile 来进行镜像构建，并结合持续集成(Continuous Integration)系统进行集成测试，而运维人员则可以直接在各种环境中快速部署该镜像，甚至结合持续部署(Continuous Delivery/Deployment) 系统进行自动部署。

而且使用 Dockerfile 使镜像构建透明化，不仅开发团队可以理解应用运行环境，也方便运维团队理解应用运行所需条件，帮助更好地在生产环境中部署该镜像。

## 迁移更轻松

由于 Docker 确保了执行环境的一致性，使得应用的迁移更加容易。Docker 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。因此用户可以很轻松地将在一个平台上运行的应用，迁移到另一个平台上，而不用担心运行环境变化导致应用无法正常运行的情况。

## 维护和扩展更轻松

Docker 使用的分层存储以及镜像技术，使得应用重复部分的复用更为容易，也使得应用的维护更新和基于基础镜像进一步扩展镜像变得非常简单。此外，Docker 团队同各个开源项目团队一起维护了一大批高质量的官方镜像，既可以直接在生产环境使用，又可以作为基础进一步定制，大大降低了应用服务的镜像制作成本。

# Docker的镜像和容器

Docker的口号是“Build, Ship and Run Any App, Anywhere.”，大意是编译好一个应用后，可以在任何地方运行，不会像传统的程序一样，一旦换了运行环境，往往就会出现缺这个库，少那个包的问题。那么Docker是怎么做到这点的呢？

简单说就是它在编译应用的时候把这个应用依赖的所有东西都构建到镜像里面（有点像程序的静态编译——只是像而已）。我们把这个编译构建好的东西叫Docker镜像（Image），然后当Docker deamon（Docker的守护进程/服务进程）运行这个镜像的时候，我们称其为Docker容器（Container）。可以简单理解Docker镜像和Docker容器的关系就像是程序和进程的关系一样(当然实质是不一样的)。

## Images和Layers

每个Docker镜像（Image）都引用了一些只读的（read\-only）层（layer），不同的文件系统layer也不同。这些layer堆叠在一起构成了容器（Container）的根文件系统（root filesystem）。下图是Ubuntu 15.04的镜像，共由4个镜像层（image layer）组成：

![](https://user-gold-cdn.xitu.io/2018/8/29/16584ca47d6c797f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## Container和Layers

容器和镜像的主要区别就是顶部的那个可写层（即之前说的那个“container layer”）。容器运行时做的所有操作都会写到这个可写层里面，当容器删除的时候，这个可写层也会被删掉，但底层的镜像依旧保持不变。所以，不同的容器都有自己的可写层，但可以共享同一个底层镜像。下图展示了多个容器共享同一个Ubuntu 15.04镜像。

![](https://user-gold-cdn.xitu.io/2018/8/29/16584cab0e859909?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Docker的storage driver负责管理只读的镜像层和可写的容器层，当然不同的driver实现的方式也不同，但其后都有两项关键技术：**可堆叠的镜像层**（stackable image layer）和**写时拷贝技术**（copy\-on\-write, CoW）。

## Docker数据持久化

刚开始的时候，Docker一般只适用于无状态的计算场景使用。但随着发展，Docker通过data volume技术也可以做到数据持久化了。Data volume就是我们将主机的某个目录挂载到容器里面，这个data volume不受storage driver的控制，所有对这个data volume的操作会绕过storage driver直接其操作，其性能也只受本地主机的限制。而且我们可以挂载任意多个data volume到容器中，不同容器也可以共享同一个data volume。

下图展示了一个Docker主机上面运行着两个容器.每一个容器在主机上面都有着自己的地址空间（/var/lib/docker/...），除此以外，它们还共享着主机上面的同一个/data目录。

![](https://user-gold-cdn.xitu.io/2018/8/29/16584cb3b1967b51?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

参考文档：

[yeasy.gitbooks.io/docker\_prac…](https://link.juejin.im?target=https%3A%2F%2Fyeasy.gitbooks.io%2Fdocker_practice%2Fcontent%2Fintroduction%2Fwhat.html) [yeasy.gitbooks.io/docker\_prac…](https://link.juejin.im?target=https%3A%2F%2Fyeasy.gitbooks.io%2Fdocker_practice%2Fcontent%2Fintroduction%2Fwhy.html) [docs.docker.com/storage/sto…](https://link.juejin.im?target=https%3A%2F%2Fdocs.docker.com%2Fstorage%2Fstoragedriver%2F)




# 安装Docker

Docker 是一个开源的商业产品，有两个版本：社区版（Community Edition，缩写为 CE）和企业版（Enterprise Edition，缩写为 EE）。企业版包含了一些收费服务，个人开发者一般用不到。下面的介绍都针对社区版。

Docker CE 的安装请参考官方文档。

> *   [Mac](https://docs.docker.com/docker-for-mac/install/)
> *   [Windows](https://docs.docker.com/docker-for-windows/install/)
> *   [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
> *   [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
> *   [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
> *   [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
> *   [其他 Linux 发行版](https://docs.docker.com/install/linux/docker-ce/binaries/)

安装完成后，运行下面的命令，验证是否安装成功。

> ```bash
>
> $ docker version
> # 或者
> $ docker info
>
> ```

Docker 需要用户具有 sudo 权限，为了避免每次命令都输入`sudo`，可以把用户加入 Docker 用户组（[官方文档](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user)）。

> ```bash
>
> $ sudo usermod -aG docker $USER
>
> ```

Docker 是服务器\-\-\-\-客户端架构。命令行运行`docker`命令的时候，需要本机有 Docker 服务。如果这项服务没有启动，可以用下面的命令启动（[官方文档](https://docs.docker.com/config/daemon/systemd/)）。

> ```bash
>
> # service 命令的用法
> $ sudo service docker start
>
> # systemctl 命令的用法
> $ sudo systemctl start docker
>
> ```

# image 文件

**Docker 把应用程序及其依赖，打包在 image 文件里面。**只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

> ```bash
>
> # 列出本机的所有 image 文件。
> $ docker image ls
>
> # 删除 image 文件
> $ docker image rm [imageName]
>
> ```

image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。Docker 的官方仓库 [Docker Hub](https://hub.docker.com/) 是最重要、最常用的 image 仓库。此外，出售自己制作的 image 文件也是可以的。

# 实例：hello world

下面，我们通过最简单的 image 文件"[hello world"](https://hub.docker.com/r/library/hello-world/)，感受一下 Docker。

需要说明的是，国内连接 Docker 的官方仓库很慢，还会断线，需要将默认仓库改成国内的镜像网站，具体的修改方法


首先，运行下面的命令，将 image 文件从仓库抓取到本地。

> ```bash
>
> $ docker image pull library/hello-world
>
> ```

上面代码中，`docker image pull`是抓取 image 文件的命令。`library/hello-world`是 image 文件在仓库里面的位置，其中`library`是 image 文件所在的组，`hello-world`是 image 文件的名字。

由于 Docker 官方提供的 image 文件，都放在[`library`](https://hub.docker.com/r/library/)组里面，所以它的是默认组，可以省略。因此，上面的命令可以写成下面这样。

> ```bash
>
> $ docker image pull hello-world
>
> ```

抓取成功以后，就可以在本机看到这个 image 文件了。

> ```bash
>
> $ docker image ls
>
> ```

现在，运行这个 image 文件。

```bash

$ docker container run hello-world

```

`docker container run`命令会从 image 文件，生成一个正在运行的容器实例。

注意，`docker container run`命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的`docker image pull`命令并不是必需的步骤。

如果运行成功，你会在屏幕上读到下面的输出。

> ```bash
>
> $ docker container run hello-world
>
> Hello from Docker!
> This message shows that your installation appears to be working correctly.
>
> ... ...
>
> ```

输出这段提示以后，`hello world`就会停止运行，容器自动终止。

有些容器不会自动终止，因为提供的是服务。比如，安装运行 Ubuntu 的 image，就可以在命令行体验 Ubuntu 系统。

> ```bash
>
> $ docker container run -it ubuntu bash
>
> ```

对于那些不会自动终止的容器，必须使用[`docker container kill`](https://docs.docker.com/engine/reference/commandline/container_kill/) 命令手动终止。

> ```bash
>
> $ docker container kill [containID]
>
> ```

# 容器文件

** image 文件生成的容器实例，本身也是一个文件，称为容器文件。**也就是说，一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已。

> ```bash
>
> # 列出本机正在运行的容器
> $ docker container ls
>
> # 列出本机所有容器，包括终止运行的容器
> $ docker container ls --all
>
> ```

上面命令的输出结果之中，包括容器的 ID。很多地方都需要提供这个 ID，比如上一节终止容器运行的`docker container kill`命令。

终止运行的容器文件，依然会占据硬盘空间，可以使用[`docker container rm`](https://docs.docker.com/engine/reference/commandline/container_rm/)命令删除。

> ```bash
>
> $ docker container rm [containerID]
>
> ```

运行上面的命令之后，再使用`docker container ls --all`命令，就会发现被删除的容器文件已经消失了。

# Dockerfile 文件

学会使用 image 文件以后，接下来的问题就是，如何可以生成 image 文件？如果你要推广自己的软件，势必要自己制作 image 文件。

这就需要用到 Dockerfile 文件。它是一个文本文件，用来配置 image。Docker 根据 该文件生成二进制的 image 文件。

下面通过一个实例，演示如何编写 Dockerfile 文件。

# 实例：制作自己的 Docker 容器

下面我以 [koa\-demos](http://www.ruanyifeng.com/blog/2017/08/koa.html) 项目为例，介绍怎么写 Dockerfile 文件，实现让用户在 Docker 容器里面运行 Koa 框架。

作为准备工作，请先[下载源码](https://github.com/ruanyf/koa-demos/archive/master.zip)。

> ```bash
>
> $ git clone https://github.com/ruanyf/koa-demos.git
> $ cd koa-demos
>
> ```

## 编写 Dockerfile 文件

首先，在项目的根目录下，新建一个文本文件`.dockerignore`，写入下面的[内容](https://github.com/ruanyf/koa-demos/blob/master/.dockerignore)。

> ```bash
>
> .git
> node_modules
> npm-debug.log
>
> ```

上面代码表示，这三个路径要排除，不要打包进入 image 文件。如果你没有路径要排除，这个文件可以不新建。

然后，在项目的根目录下，新建一个文本文件 Dockerfile，写入下面的[内容](https://github.com/ruanyf/koa-demos/blob/master/Dockerfile)。


```bash

 FROM node:8.4
 COPY . /app
 WORKDIR /app
 RUN npm install --registry=https://registry.npm.taobao.org
 EXPOSE 3000

```

上面代码一共五行，含义如下。

> *   `FROM node:8.4`：该 image 文件继承官方的 node image，冒号表示标签，这里标签是`8.4`，即8.4版本的 node。
> *   `COPY . /app`：将当前目录下的所有文件（除了`.dockerignore`排除的路径），都拷贝进入 image 文件的`/app`目录。
> *   `WORKDIR /app`：指定接下来的工作路径为`/app`。
> *   `RUN npm install`：在`/app`目录下，运行`npm install`命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
> *   `EXPOSE 3000`：将容器 3000 端口暴露出来， 允许外部连接这个端口。

## 创建 image 文件

有了 Dockerfile 文件以后，就可以使用`docker image build`命令创建 image 文件了。

> ```bash
>
> $ docker image build -t koa-demo .
> # 或者
> $ docker image build -t koa-demo:0.0.1 .
>
> ```

上面代码中，`-t`参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是`latest`。最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。

如果运行成功，就可以看到新生成的 image 文件`koa-demo`了。

> ```bash
>
> $ docker image ls
>
> ```

## 生成容器

`docker container run`命令会从 image 文件生成容器。

> ```bash
>
> $ docker container run -p 8000:3000 -it koa-demo /bin/bash
> # 或者
> $ docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
>
> ```

上面命令的各个参数含义如下：

> *   `-p`参数：容器的 3000 端口映射到本机的 8000 端口。
> *   `-it`参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
> *   `koa-demo:0.0.1`：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
> *   `/bin/bash`：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。

如果一切正常，运行上面的命令以后，就会返回一个命令行提示符。

> ```bash
>
> root@66d80f4aaf1e:/app#
>
> ```

这表示你已经在容器里面了，返回的提示符就是容器内部的 Shell 提示符。执行下面的命令。

> ```bash
>
> root@66d80f4aaf1e:/app# node demos/01.js
>
> ```

这时，Koa 框架已经运行起来了。打开本机的浏览器，访问 http://127.0.0.1:8000，网页显示"Not Found"，这是因为这个 [demo](https://github.com/ruanyf/koa-demos/blob/master/demos/01.js) 没有写路由。

这个例子中，Node 进程运行在 Docker 容器的虚拟环境里面，进程接触到的文件系统和网络接口都是虚拟的，与本机的文件系统和网络接口是隔离的，因此需要定义容器与物理机的端口映射（map）。

现在，在容器的命令行，按下 Ctrl + c 停止 Node 进程，然后按下 Ctrl + d （或者输入 exit）退出容器。此外，也可以用`docker container kill`终止容器运行。

> ```bash
>
> # 在本机的另一个终端窗口，查出容器的 ID
> $ docker container ls
>
> # 停止指定的容器运行
> $ docker container kill [containerID]
>
> ```

容器停止运行之后，并不会消失，用下面的命令删除容器文件。

> ```bash
>
> # 查出容器的 ID
> $ docker container ls --all
>
> # 删除指定的容器文件
> $ docker container rm [containerID]
>
> ```

也可以使用`docker container run`命令的`--rm`参数，在容器终止运行后自动删除容器文件。

> ```bash
>
> $ docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
>
> ```

## CMD 命令

上一节的例子里面，容器启动以后，需要手动输入命令`node demos/01.js`。我们可以把这个命令写在 Dockerfile 里面，这样容器启动以后，这个命令就已经执行了，不用再手动输入了。

> ```bash
>
> FROM node:8.4
> COPY . /app
> WORKDIR /app
> RUN npm install --registry=https://registry.npm.taobao.org
> EXPOSE 3000
> CMD node demos/01.js
>
> ```

上面的 Dockerfile 里面，多了最后一行`CMD node demos/01.js`，它表示容器启动后自动执行`node demos/01.js`。

你可能会问，`RUN`命令与`CMD`命令的区别在哪里？简单说，`RUN`命令在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件；`CMD`命令则是在容器启动后执行。另外，一个 Dockerfile 可以包含多个`RUN`命令，但是只能有一个`CMD`命令。

注意，指定了`CMD`命令以后，`docker container run`命令就不能附加命令了（比如前面的`/bin/bash`），否则它会覆盖`CMD`命令。现在，启动容器可以使用下面的命令。

> ```bash
>
> $ docker container run --rm -p 8000:3000 -it koa-demo:0.0.1
>
> ```

## 发布 image 文件

容器运行成功后，就确认了 image 文件的有效性。这时，我们就可以考虑把 image 文件分享到网上，让其他人使用。

首先，去 [hub.docker.com](https://hub.docker.com/) 或 [cloud.docker.com](https://cloud.docker.com) 注册一个账户。然后，用下面的命令登录。

> ```bash
>
> $ docker login
>
> ```

接着，为本地的 image 标注用户名和版本。

> ```bash
>
> $ docker image tag [imageName] [username]/[repository]:[tag]
> # 实例
> $ docker image tag koa-demos:0.0.1 ruanyf/koa-demos:0.0.1
>
> ```

也可以不标注用户名，重新构建一下 image 文件。

> ```bash
>
> $ docker image build -t [username]/[repository]:[tag] .
>
> ```

最后，发布 image 文件。

> ```bash
>
> $ docker image push [username]/[repository]:[tag]
>
> ```

发布成功以后，登录 hub.docker.com，就可以看到已经发布的 image 文件。

## 其他有用的命令

docker 的主要用法就是上面这些，此外还有几个命令，也非常有用。

**（1）docker container start**

前面的`docker container run`命令是新建容器，每运行一次，就会新建一个容器。同样的命令运行两次，就会生成两个一模一样的容器文件。如果希望重复使用容器，就要使用`docker container start`命令，它用来启动已经生成、已经停止运行的容器文件。

> ```bash
>
> $ docker container start [containerID]
>
> ```

**（2）docker container stop**

前面的`docker container kill`命令终止容器运行，相当于向容器里面的主进程发出 SIGKILL 信号。而`docker container stop`命令也是用来终止容器运行，相当于向容器里面的主进程发出 SIGTERM 信号，然后过一段时间再发出 SIGKILL 信号。

> ```bash
>
> $ bash container stop [containerID]
>
> ```

这两个信号的差别是，应用程序收到 SIGTERM 信号以后，可以自行进行收尾清理工作，但也可以不理会这个信号。如果收到 SIGKILL 信号，就会强行立即终止，那些正在进行中的操作会全部丢失。

**（3）docker container logs**

`docker container logs`命令用来查看 docker 容器的输出，即容器里面 Shell 的标准输出。如果`docker run`命令运行容器的时候，没有使用`-it`参数，就要用这个命令查看输出。

> ```bash
>
> $ docker container logs [containerID]
>
> ```

**（4）docker container exec**

`docker container exec`命令用于进入一个正在运行的 docker 容器。如果`docker run`命令运行容器的时候，没有使用`-it`参数，就要用这个命令进入容器。一旦进入了容器，就可以在容器的 Shell 执行命令了。

> ```bash
>
> $ docker container exec -it [containerID] /bin/bash
>
> ```

**（5）docker container cp**

`docker container cp`命令用于从正在运行的 Docker 容器里面，将文件拷贝到本机。下面是拷贝到当前目录的写法。

```bash

$ docker container cp [containID]:[/path/to/file] .
```