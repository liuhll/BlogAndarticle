# 进入Docker容器

- 在使用 `-d` 参数时，`容器`启动后会**进入后台**

- 某些时候需要进入`容器`进行操作
  - 使用 `docker attach` 命令
  - 使用 `nsenter` 工具

## attach 命令
- `docker attach` 是`Docker`自带的命令

- **Demo**

```bash
$ sudo docker run -idt ubuntu #守护态运行容器，容器自动转到后台运行，不会进入到容器中，返回容器id
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps #查看运行中的docker容器
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$ sudo docker attach nostalgic_hypatia #使用 docker attach命令进入到docker容器中
root@243c32535da7:/#
```

- 使用 `attach` 命令有时候并**不方便**
   - 当多个窗口同时 `attach` 到同一个`容器`的时候，**所有窗口都会同步显示**。
   - 当*某个窗口因命令阻塞*时,其他窗口也**无法执行操作**了

## nsenter 命令

### 安装

- `nsenter` 工具在 `util-linux` 包*2.23版本*后包含。 如果系统中 `util-linux` 包没有该命令，可以按照下面的方法从**源码安装**。
```bash
$ cd /tmp; curl https://www.kernel.org/pub/linux/utils/util-linux/v2.24/util-linux-2.24.tar.gz | tar -zxf-; cd util-linux-2.24;
$ ./configure --without-ncurses
$ make nsenter && sudo cp nsenter /usr/local/bin
```

### 使用

- `nsenter` 可以访问另一个进程的命名空间。
- `nsenter` 要正常工作需要有 `root权限`。 
   - *很不幸*，`Ubuntu 14.04` 仍然使用的是 `util-linux 2.20`。
   - 安装最新版本的 `util-linux（2.24`）版，请按照以下步骤：
```bash
$ wget https://www.kernel.org/pub/linux/utils/util-linux/v2.24/util-linux-2.24.tar.gz; tar xzvf util-linux-2.24.tar.gz
$ cd util-linux-2.24
$ ./configure --without-ncurses && make nsenter
$ sudo cp nsenter /usr/local/bin
```

- 为了连接到`容器`，你还需要找到容器的第一个进程的 `PID`，可以通过下面的命令获取。
```bash
PID=$(docker inspect --format "{{ .State.Pid }}" <container>)
```

> 通过这个 `PID`，就可以连接到这个容器：

```bash
$ nsenter --target $PID --mount --uts --ipc --net --pid
```

- 下面给出一个完整的例子。

```bash
$ sudo docker run -idt ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia
$ PID=$(docker-pid 243c32535da7)
10981
$ sudo nsenter --target 10981 --mount --uts --ipc --net --pid
root@243c32535da7:/#
```

- 更简单的，建议大家下载 [`.bashrc_docker`](https://raw.githubusercontent.com/yeasy/docker_practice/master/_local/.bashrc_docker)，并将内容放到 `.bashrc` 中。

```bash
$ wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker;
$ echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc
```
> **Notes**  
> - 这个文件中定义了很多方便使用 `Docker` 的命令，
> - **例如**  
>   - `docker-pid` 可以获取某个容器的 `PID`；
>    - 而 `docker-enter` 可以进入容器或直接在容器内执行命令。

```bash
$ echo $(docker-pid <container>)
$ docker-enter <container> ls
```