## Docker 介绍

### Docker是什么
```
容器技术
基于容器技术的产品
容器、镜像分发平台
一个社区
未来 产品部署的趋势
```

docker 不是`虚拟机`，顶多称得上`虚拟环境`。

虚拟机是完全独立操作系统内核、程序运行时、应用程序。
docker只有程序运行时、应用程序。
`docker容器内的进程`和`普通host 进程`共享一个内核，一套内核资源设施。但是在运行环境层面是隔离的，具体什么隔离见下面。

--------
### Docker基本原理

主要有`资源隔离`、`资源控制`、`文件系统的分层读写`

#### namespace的隔离

主要涉及 `clone` `setns` `unshare`三个系统调用

##### fork，clone的区别

linux 并不严格区分进程、线程（windows中倾向于使用`进程是资源组织的概念，线程是执行者的概念`区分）。

fork的过程是fork调用进程（父进程），返回非0值，表示fork的子进程PID，而子进程中fork返回值是0，通过这种方式区分父子进程（返回值<0，fork失败）。

但这个系统调用的问题在于，不能很好控制父子进程的隔离，比如打开的文件、使用的IPC、权限问题，并没有明确指定子进程如何处理这些资源，fork接口中没有提供控制入口，不得不让子进程对这些控制（但不应该由子进程控制）。
linux 在2.6版本引入了clone系统调用，对于父子进程的环境隔离有了更为细致的控制。linux 内核线程也是通过clone创建。


|分类|系统调用参数|相关内核版本|备注|
|--|-----|---------|-----------|
|Mount namespaces|CLONE_NEWNS|Linux 2.4.19|各种文件类型的隔离/proc /sys /tmp
|UTS namespaces|CLONE_NEWUTS|Linux 2.6.19| hostname的隔离 `Hostname and NIS domain name`
|IPC namespaces|CLONE_NEWIPC|Linux 2.6.19|IPC的隔离，隔离不同空间的通信不会发生混乱
|PID namespaces|CLONE_NEWPID|Linux 2.6.24|进程号的隔离，linux很多机制、进程管理都依赖于pid=1的父进程|
|Network namespaces|CLONE_NEWNET|始于Linux 2.6.24 完成于 Linux 2.6.29|网络的隔离，网络协议栈的隔离，不同的网络栈|
|User namespaces|CLONE_NEWUSER|始于 Linux 2.6.23 完成于 Linux 3.8)|user空间，进程的uid,gid,euid,egid|

[参考来源]()

[coolshell](http://coolshell.cn/articles/17010.html)

[man page](http://man7.org/linux/man-pages/man7/namespaces.7.html)

#### cgroup 资源控制

cgroup是linux 系统的自身功能，对进程的资源控制，包括`限制资源使用`，`优先级控制`，`审计`，`Control挂起、恢复`

`/sys/fs/cgroup` 下面是对进程的资源控制策略


#### AUFS 文件系统的隔离

docker 容器内的进程的文件系统和host os的文件系统是隔离的（不包括使用各种数据共享）。而且docker提出`文件系统layer`的概念。
container只对最上层的文件layer有读写权限（container拥有），下层的layer(即image的文件layer)只有读权限。这样可以控制各自container文件系统的独立性（同一个image可以启动
很多不同的container），各自container对文件系统的修改互不可见。

当然这是AUFS技术，不是docker提出的，他加工了一下。

docker的几大优势（见下面）都可以体现在这里。
持续集成、版本控制、便于分发、隔离性、安全性

container的修改不影响`base image`的文件系统，`base image` 可以继续作为其他的container的底层文件系统。
实际部署、分发只需要分发上层的修改即可，不需要完整的文件系统。

-----
### Docker的优势
docker是对容器技术的包装，就如将技术包装成产品一样。容器技术发展了很久，但是一直不为大众（普通开发者）熟知。
但是docker将这些容器技术友好地展示给了开发者

* 持续集成
* 版本控制
* 可移植性
* 隔离性
* 安全性

[参考]
* [docker的优势](http://dockone.io/article/389)
* [docker vs chef vs lxc](http://code.csdn.net/news/2819773)

#### 容器技术的优势
*  性能损耗低，远比KVM一类的硬件模拟来的高效[参考](http://www.infoq.com/cn/articles/docker-cloud-environment-application-practice)
* 比较好的隔离、控制，虽然不及KVM这类的硬件模拟的完全隔离
* 便于分发、部署，将应用所需的各种依赖、环境打包在一起（自满足，不再受限于host os）

#### docker自身的优势
* image概念，固化环境
* 类似于 git的版本控制
* 类似于 github的image仓库，下载、上传、分享
* 镜像的编排工具 Dockerfile，不需要了解底层复杂的技术原理，就可很好控制容器的环境
* docker 各种命令，也是对镜像、容器的使用过程的很好提炼（这些过程使用的各种底层复杂技术，并不需要用户care）。比如 `run`,`stop`,`start`
* docker 虚拟化，或者容器化，普及了容器技术的应用，容器化成为一种潮流，windows也提出了相应的容器化计划。

题外话：
docker最大贡献，可能就是普及了容器技术，并推动其发展（有钱好办事），使之成为一种潮流。
docker化，就是容器化。

### 基于容器技术的隔离性能
类似于`KVM`，模拟硬件的虚拟化技术，他们模拟底层硬件，guest os甚至无法察觉自己运行在一个虚拟机之中。但由于基于硬件的模拟，硬件->软件->硬件的过程自然无法和

-----
### 边学边用
#### 简单安装
* windows
* mac
* linux 64位 apt-get install docker.io

#### docker命令表








---
#### questions

* [KVM vs LXC](http://stackoverflow.com/questions/20578039/difference-between-kvm-and-lxc)

[KVM](https://zh.wikipedia.org/wiki/%E5%9F%BA%E4%BA%8E%E5%86%85%E6%A0%B8%E7%9A%84%E8%99%9A%E6%8B%9F%E6%9C%BA) 即 Kernel-based Virtual Machine，linux 2.6.20引入的可加载核心模块。本质和virtual box, vmware一样，基于硬件的模拟。
LXC 是host os将自身的内核与guest os共享，而各自对资源的处理是独立、隔离的。所以LXC启动进程不需要想KVM那样启动一套kernel(冷启动)，各种环境、资源准备好了，\
再加载、启动所需的进程；而是直接使用host os的、已经在运行的kernal，只需要加载、准备一些自己依赖的特殊环境，自己的二进制文件，就可以启动了。
容器内的进程在host os就如普通进程一样（namespace，cgroup，文件系统不同而已）。

在这里一直强调的kernel，不是指`initrd.img-3.13.0-24-generic`这样的文件，而是指操作系统运行的各种内核资源（进程表、各种计数器），以及现有的各种设施，比如对内存、文件、进程管理的
内核资源比较宝贵，创建、销毁都是需要很高代价的。

Docker对LXC基础（0.9以后使用libcontainer替代了LXC）

* [Docker对host有要求吗]
 * docker容器内的进程使用的内核版本，需要兼容host os的内核。这就对 host有要求
 * [RedHat 安装Docker](https://docs.docker.com/engine/installation/rhel/)，内核版本有要求、64位架构
 ```
 Docker requires a 64-bit installation regardless of your Red Hat version. Docker requires that your kernel must be 3.10 at minimum, which Red Hat 7 runs.
 ```
 * [Ubuntu 安装Docker](https://docs.docker.com/engine/installation/ubuntulinux/)
 ```
Docker requires a 64-bit installation regardless of your Ubuntu version. Additionally, your kernel must be 3.10 at minimum. The latest 3.10 minor version or a newer maintained version are also acceptable.
 ```
 * [CentOs 安装Docker](https://docs.docker.com/engine/installation/centos/)
```
Docker requires a 64-bit installation regardless of your CentOS version. Also, your kernel must be 3.10 at minimum, which CentOS 7 runs.
```

* [image vs container](http://stackoverflow.com/questions/23735149/docker-image-vs-container)

```
image 是一个类似于归档文件，它将我们需要运行的程序（即容器），它所依赖的底层依赖、程序二进制文件、配置打包成一个image。然后以image 的形式分发出去。这样大家就\
不用关系各种环境的差异，反正程序的各种依赖都在 image中。image更像是一个运行环境的包装器。
container 相对来说是一个动态的概念。通过`docker run $image $command` 在一个指定的环境$image运行某个或几个程序。当container运行之后，它就会有自己独立的环境，\
基于原来的环境（并不会破坏$image中的环境，详见docker底层文件系统的隔离机制）。这样$container对文件系统做的一切修改，仅在自己的环境可见（不包括各种数据共享）。

自然，$container 运行之后，自己独立的环境可以固化下来，变成一个image，通过`commit $container $respository:[$tag]`可以将$container的修改提交到一个image中
$container 是一个相对动态的环境，当有进程在运行时，容器的状态是running
$image 相对静态的环境，没有进程。用于固化环境、分发环境、构建新环境
```

* [container之间、主机之间如何共享数据]()

```
-v, --volume=[] 来创建一个或多个 volume。
或者 --volumes-from=[]

```

* [stop vs kill](http://superuser.com/questions/756999/whats-the-difference-between-docker-stop-and-docker-kill)

```
stop 是停止容器（停止指定的进程），其中过程是`Stop a running container by sending SIGTERM and then SIGKILL after a grace period`
kill 是向容器发送信号，就像`kill`命令一样。默认发送SIGKILL信号，可以通过 `-s SIGNAL`指定发送的信号
```

* [ENTRYPOINT vs CMD](http://stackoverflow.com/questions/21553353/what-is-the-difference-between-cmd-and-entrypoint-in-a-dockerfile)

```
`entrypoint` 是容器的入口点，进入容器要执行的进程，毕竟docker是进程级别的隔离，进入容器（即要启动一个隔离的进程）需要执行一个程序，默认是 `/bin/sh`
可以使用 --entrypoint=="$entrypoint"覆盖
`CMD` 或者通过`参数传入`的是传入的执行命令，或者参数
比如 运行`docker run --entrypoint="/bin/cat" notebook:latest /etc/passwd`
```

* [pause unpause] 发生了什么(https://www.kernel.org/doc/Documentation/cgroups/freezer-subsystem.txt)

```
pause 使用`cgroups freezer`技术
The docker pause command uses the cgroups freezer to suspend all processes in a container. Traditionally, when suspending a process the SIGSTOP signal is used, which is observable by the process being suspended. With the cgroups freezer the process is unaware, and unable to capture, that it is being suspended, and subsequently resumed.
```

* [Docker vs Vagrant](http://www.csdn.net/article/2014-05-14/2819773/3)
```
Vagrant 基于行业技术标准提供了易于配置、可量产、并且可移植的工作环境，并且由一个统一的工作流程进行控制，从而使你和团队的工作效率和灵活性得以最大化。
为了实现这些特性， Vagrant 站在巨人的肩膀上，诸如 VirtualBox 、 VMware 、 AWS 等顶级供应商以及其他供应商。
同时业界标准的供应工具，诸如 shell scripts 、 Chef 或者 Puppet 也都能在机器上自动安装和配置软件。
```
Vagrant是一个`好用`的虚拟机管理、配置软件
Docker是一个`虚拟环境`封装软件

##### 参考链接
```
下面链接只是参考链接的一部分，可能某些引用并没有提到它的参考。
感谢各位大神的分享
```
* ATA 链接
 * [Docker初体验](http://www.atatech.org/articles/27404)
 * [Docker学习笔记](http://www.atatech.org/articles/41429)
 * [Docker存储-AUFS](http://www.atatech.org/articles/30116)
* 外网链接
 * [Docker基础技术：Linux Namespace（上）](http://coolshell.cn/articles/17010.html)
 * [Docker基础技术：Linux Namespace（下）](http://coolshell.cn/articles/17029.html)
 * [Docker基础技术：Linux CGroup](http://coolshell.cn/articles/17049.html)
 * [Docker基础技术：AUFS](http://coolshell.cn/articles/17061.html)
 * [Docker基础技术：DeviceMapper](http://coolshell.cn/articles/17200.html)
 * [Docker 4 -- 总结](http://blog.tankywoo.com/docker/2014/05/08/docker-4-summary.html)
 * [Dockerpool](http://dockerpool.com/static/books/docker_practice/introduction/what.html)
