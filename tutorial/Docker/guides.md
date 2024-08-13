<div align="center">
 <h1>Docker</h1>
</div>

---

> [!NOTE]
> Docker相关文档,以及常见容器编排

## 基础

1. **第一个容器** 防劝退

    ```bash
    docker run -d -p 8080:80 docker/welcome-to-docker
    ```

2. **Docker基础命令**

    1. **Commands**

        - **run**
            - ***--add-host***

            **作用:** 向容器添加一个自定义的主机名到 IP 的映射。

            ```bash
            docker run --add-host database:192.168.1.100 ubuntu
            # 在此示例中，任何从容器内尝试访问 database 主机名的请求都将被解析到 192.168.1.100。
            ```

            - ***--annotation***

            **作用:** 向容器添加注解，这些注解会被传递给 OCI 运行时。

            ```bash
            docker run --annotation "organization=com.example" ubuntu
            # 这为容器设置了一个注解，指定组织为 com.example。
            ```

            - ***-a, --attach***

            **作用:** 附加到容器的 STDIN、STDOUT 或 STDERR。

            ```bash
            docker run -a stdout -a stderr -i ubuntu /bin/bash
            # 这个命令将启动一个 Ubuntu 容器，并附加到容器的 STDOUT 和 STDERR。
            ```

            - ***--blkio-weight***

            **作用:** 设置块 I/O 操作的相对权重（在 10 到 1000 之间，0 为禁用）。

            ```bash
            docker run --blkio-weight 300 ubuntu
            # 这将为容器的块 I/O 操作设置权重为 300。
            ```

            - ***--blkio-weight-device***

            **作用:** 设置特定设备的块 I/O 权重。

            ```bash
            docker run --blkio-weight-device "/dev/sda:200" ubuntu
            # 这将设置 /dev/sda 设备的块 I/O 权重为 200。
            ```

            - ***--cap-add***

            **作用:** 向容器添加 Linux capabilities。

            ```bash
            docker run --cap-add NET_ADMIN ubuntu
            # 这个命令向容器添加了 NET_ADMIN 权限，允许它执行网络管理任务。
            ```

            - ***--cap-drop***

            **作用:** 从容器中删除特定的 Linux capabilities。

            ```bash
            docker run --cap-drop ALL --cap-add NET_BIND_SERVICE ubuntu
            # 这个命令删除了容器的所有默认权限，只添加了 NET_BIND_SERVICE 权限，允许它绑定到系统端口（端口号小于 1024）。
            ```

            - ***--cgroup-parent***

            **作用:** 设置容器的父 cgroup。

            ```bash
            docker run --cgroup-parent=/parent_cgroup ubuntu
            # 这将容器放在 /parent_cgroup 下作为一个子 cgroup 运行。
            ```

            - ***--cgroupns***

            **作用:** 设置容器使用的 cgroup 命名空间。

            1. **host**: 在 Docker 主机的 cgroup 命名空间中运行容器。
            2. **private**: 在它自己的私有 cgroup 命名空间中运行容器。
            3. **空字符串**: 使用由守护进程的 default-cgroupns-mode 选项配置的 cgroup 命名空间。

            ```bash
            docker run --cgroupns=host ubuntu
            # 这个命令将容器运行在 Docker 主机的 cgroup 命名空间中，共享主机的资源限制。
            ```

            - ***--cidfile***

            **作用:** 将容器的 ID 写入到指定的文件中。

            ```bash
            docker run --cidfile /path/to/cidfile.txt ubuntu
            # 这将启动一个基于 ubuntu 镜像的容器，并将容器的 ID 写入 /path/to/cidfile.txt 文件。
            ```

            - ***--cpu-period***

            **作用:** 设置 CPU CFS（完全公平调度器）的周期，以微秒为单位。

            ```bash
            docker run --cpu-period 100000 ubuntu
            # 此设置将 CPU CFS 周期限制为 100000 微秒，与 --cpu-quota 一起使用可以定义容器的 CPU 使用上限。
            ```

            - ***--cpu-quota***

            **作用:** 限制 CPU CFS 的配额，以微秒为单位。

            ```bash
            docker run --cpu-quota 50000 ubuntu
            # 这意味着在每个 100000 微秒的周期内，容器可以使用 50000 微秒的 CPU 时间。
            ```

            - ***--cpu-rt-period***

            **作用:** 设置 CPU 实时周期，以微秒为单位。

            ```bash
            docker run --cpu-rt-period 1000000 ubuntu
            # 此命令设置容器的实时 CPU 周期为 1000000 微秒。
            ```

            - ***--cpu-rt-runtime***

            **作用:** 限制 CPU 实时运行时间，以微秒为单位。

            ```bash
            docker run --cpu-rt-runtime 950000 ubuntu
            # 这设置容器在每个实时周期内可用的 CPU 实时运行时间为 950000 微秒。
            ```

            - ***-c, --cpu-shares***

            **作用:** 设置 CPU 分享的相对权重。

            ```bash
            docker run -c 512 ubuntu
            # 容器的 CPU 权重被设置为 512，相对于系统中其他容器的权重。
            ```

            - ***--cpus***

            **作用:** 设置容器可以使用的 CPU 数量。

            ```bash
            docker run --cpus 1.5 ubuntu
            # 此命令允许容器使用最多 1.5 个 CPU。
            ```

            - ***--cpuset-cpus***

            **作用:** 指定容器可以执行的 CPU。

            ```bash
            docker run --cpuset-cpus 0-2 ubuntu
            # 此设置允许容器使用 CPU 0、1 和 2。
            ```

            - ***--cpuset-mems***

            **作用:** 指定容器可以使用的内存节点（NUMA 节点）。

            ```bash
            docker run --cpuset-mems 0,1 ubuntu
            # 这允许容器使用节点 0 和 1 上的内存。
            ```

            - ***-d, --detach***

            **作用:** 在后台运行容器，并打印容器 ID。

            ```bash
            docker run -d nginx
            # 这将在后台启动一个 Nginx 服务器容器，并输出容器的 ID。
            ```

            - ***--detach-keys***

            **作用:** 覆盖用于从容器附加状态分离的键序列。

            ```bash
            docker run -it --detach-keys="ctrl-x" ubuntu /bin/bash
            # 这允许用户通过按 "Ctrl-x" 而不是默认的 "Ctrl-p Ctrl-q" 来从容器的交互式会话中分离。
            ```

            - ***--device***

            **作用:** 向容器添加主机设备。

            ```bash
            docker run --device /dev/sda:/dev/sda ubuntu
            # 这将允许容器直接访问主机的 /dev/sda 设备。
            ```

            - ***--device-cgroup-rule***

            **作用:** 向 cgroup 允许的设备列表中添加规则。

            ```bash
            docker run --device-cgroup-rule='c 13:* rmw' ubuntu
            # 这允许容器访问字符设备 13 的所有设备，且有读、写和创建设备节点的权限。
            ```

            - ***--device-read-bps***

            **作用:** 限制从设备的读速率（每秒字节数）。

            ```bash
            docker run --device-read-bps /dev/sda:1mb ubuntu
            # 这限制从 /dev/sda 设备的读速率不超过每秒 1MB。
            ```

            - ***--device-read-iops***

            **作用:** 限制从设备的读 IOPS（每秒输入/输出操作数）。

            ```bash
            docker run --device-read-iops /dev/sda:1000 ubuntu
            # 这限制从 /dev/sda 的读 IOPS 不超过每秒 1000。
            ```

            - ***--device-write-bps***

            **作用:** 限制写入设备的速率（每秒字节数）。

            ```bash
            docker run --device-write-bps /dev/sda:1mb ubuntu
            # 这限制向 /dev/sda 写入的速率不超过每秒 1MB。
            ```

            - ***--device-write-iops***

            **作用:** 限制向设备写入的 IOPS（每秒输入/输出操作数）。

            ```bash
            docker run --device-write-iops /dev/sda:500 ubuntu
            # 这限制向 /dev/sda 的写 IOPS 不超过每秒 500。
            ```

            - ***--disable-content-trust***

            **作用:** 跳过镜像验证。

            ```bash
            docker run --disable-content-trust=true nginx
            # 这在拉取和运行镜像时禁用内容信任检查。
            ```

            - ***--dns***

            **作用:** 设置自定义 DNS 服务器。

            ```bash
            docker run --dns 8.8.8.8 ubuntu
            # 这将 Google 的 DNS 服务器 8.8.8.8 设置为容器的 DNS 服务器。
            ```

            - ***--dns-option***

            **作用:** 设置 DNS 选项。

            ```bash
            docker run --dns-option=timeout:10 ubuntu
            # 这为 DNS 解析设置超时时间为 10 秒。
            ```

            - ***--dns-search***

            **作用:** 设置自定义 DNS 搜索域。

            ```bash
            docker run --dns-search example.com ubuntu
            # 这将 example.com 添加为 DNS 搜索域，允许容器在解析未完全限定的主机名时使用它。
            ```

            - ***--domainname***

            **作用:** 设置容器的 NIS 域名。

            ```bash
            docker run --domainname internal.example.com ubuntu
            # 这设置容器的域名为 internal.example.com。
            ```

            - ***--entrypoint***

            **作用:** 覆盖镜像的默认 ENTRYPOINT。

            ```bash
            docker run --entrypoint /bin/bash ubuntu
            # 这启动 Ubuntu 容器并覆盖 ENTRYPOINT 为 /bin/bash，允许直接进入 Bash shell。
            ```

            - ***-e, --env***

            **作用:** 设置环境变量。

            ```bash
            docker run -e USERNAME=user -e PASSWORD=secret nginx
            # 这个命令在启动 Nginx 容器时设置了 USERNAME 和 PASSWORD 两个环境变量。
            ```

            - ***--env-file***

            **作用:** 从文件中读取环境变量。

            ```bash
            docker run --env-file ./env.list ubuntu
            # 这将从 env.list 文件中加载环境变量，文件中每行格式为 VAR=value。
            ```

            - ***--expose***

            **作用:** 暴露一个端口或一段端口范围，但不将其发布到主机。

            ```bash
            docker run --expose 3000-3005 ubuntu
            # 这个命令将暴露容器的 3000 到 3005 端口，仅对 Docker 内部网络可见。
            ```

            - ***--gpus***

            **作用:** 分配 GPU 设备给容器。

            ```bash
            docker run --gpus all nvidia/cuda:10.0-base nvidia-smi
            # 这将所有可用的 GPU 设备分配给运行 nvidia-smi 命令的容器。
            ```

            - ***--group-add***

            **作用:** 添加额外的组给容器的主用户。

            ```bash
            docker run --group-add audio ubuntu
            # 这会将容器的默认用户添加到主机的 audio 组，允许访问音频设备。
            ```

            - ***--health-cmd***

            **作用:** 设置运行以检查容器健康的命令。

            ```bash
            docker run --health-cmd "curl -f http://localhost/ || exit 1" nginx
            # 这将 nginx 容器的健康检查命令设置为访问其首页，若失败则标记为不健康。
            ```

            - ***--health-interval***

            **作用:** 设置健康检查的间隔时间。

            ```bash
            docker run --health-interval=2m nginx
            # 这设置每 2 分钟执行一次 nginx 容器的健康检查。
            ```

            - ***--health-retries***

            **作用:** 设置连续失败次数，达到此次数后报告容器不健康。

            ```bash
            docker run --health-retries=3 nginx
            # 连续三次健康检查失败后，容器被标记为不健康。
            ```

            - ***--health-start-interval***

            **作用:** 设置容器启动后进行健康检查前的等待时间。

            ```bash
            docker run --health-start-interval=30s nginx
            # 容器启动后 30 秒内不进行健康检查。
            ```

            - ***--health-start-period***

            **作用:** 容器在开始健康检查重试计数前的初始化时间。

            ```bash
            docker run --health-start-period=5m nginx
            # 容器在前 5 分钟内的健康检查失败不会计入失败重试次数。
            ```

            - ***--health-timeout***

            **作用:** 设置单次健康检查的最大运行时间。

            ```bash
            docker run --health-timeout=5s nginx
            # 单次健康检查超过 5 秒未完成则视为失败。
            ```

            - ***-h, --hostname***

            **作用:** 设置容器内的主机名。

            ```bash
            docker run --hostname mycontainer myimage
            # 这个命令将容器的主机名设置为 mycontainer。
            ```

            - ***--init***

            **作用:** 在容器内运行一个 init 进程，该进程负责转发信号并回收进程。

            ```bash
            docker run --init myimage
            # 这将在容器中启动一个 init 系统，可以更好地管理信号和进程。
            ```

            - ***-i, --interactive***

            **作用:** 保持标准输入(STDIN)开放，即使没有附加也是如此。

            ```bash
            docker run -i myimage
            # 这使得容器保持交互式，允许用户通过命令行与之交互。
            ```

            - ***--io-maxbandwidth (Windows only)***

            **作用:** 为系统驱动设定最大 IO 带宽限制。

            ```bash
            docker run --io-maxbandwidth 100MB myimage
            # 限制容器在系统驱动上的 IO 带宽不超过每秒 100MB。
            ```

            - ***--io-maxiops (Windows only)***

            **作用:** 为系统驱动设定最大 IOps (输入/输出操作每秒)限制。

            ```bash
            docker run --io-maxiops 300 myimage
            # 限制容器在系统驱动上的 IOps 不超过每秒 300次。
            ```

            - ***--ip***

            **作用:** 分配一个 IPv4 地址给容器。

            ```bash
            docker run --ip 172.30.100.104 myimage
            # 这将为容器指定一个特定的 IPv4 地址。
            ```

            - ***--ip6***

            **作用:** 分配一个 IPv6 地址给容器。

            ```bash
            docker run --ip6 2001:db8::33 myimage
            # 这将为容器指定一个特定的 IPv6 地址。
            ```

            - ***--ipc***

            **作用:** 设置容器的 IPC (进程间通信) 模式。

            ```bash
            docker run --ipc host myimage
            # 允许容器使用宿主机的 IPC 命名空间。
            ```

            - ***--isolation (Windows only)***

            **作用:** 设置容器的隔离技术。

            ```bash
            docker run --isolation hyperv myimage
            # 在 Windows 上为容器使用 Hyper-V 隔离。
            ```

            - ***--kernel-memory***

            **作用:** 为容器设置内核内存限制。

            ```bash
            docker run --kernel-memory 200M myimage
            # 限制容器内核内存使用量为 200MB。
            ```

            - ***-l, --label***

            **作用:** 设置容器的元数据标签。

            ```bash
            docker run --label "name=webserver" myimage
            # 为容器添加一个标签，标签名为 name，值为 webserver。
            ```

            - ***--label-file***

            **作用:** 从文件中读入标签。

            ```bash
            docker run --label-file labels.txt myimage
            # 从 labels.txt 文件中读取标签，并应用于容器。
            ```

            - ***--link***

            **作用:** 添加链接到另一个容器。

            ```bash
            docker run --link mydb:db myimage
            # 链接到名为 mydb 的容器，别名为 db。
            ```

            - ***--link-local-ip***

            **作用:** 为容器设置 IPv4/IPv6 链路本地地址。

            ```bash
            docker run --link-local-ip 169.254.34.12 myimage
            # 设置容器的链路本地 IPv4 地址。
            ```

            - ***--log-driver***

            **作用:** 为容器指定日志驱动程序。

            ```bash
            docker run --log-driver json-file myimage
            # 设置容器使用 json-file 日志驱动记录日志。
            ```

            - ***--log-opt***

            **作用:** 设置日志驱动的选项。

            ```bash
            docker run --log-driver json-file --log-opt max-size=10m myimage
            # 设置日志文件最大为 10MB。
            ```

            - ***--mac-address***

            **作用:** 为容器设置 MAC 地址。

            ```bash
            docker run --mac-address 92:d0:c6:0a:29:33 myimage
            # 为容器指定一个特定的 MAC 地址。
            ```

        - **exec**
        - **ps**
        - **build**
        - **pull**
        - **push**
        - **images**
        - **login**
        - **logout**
        - **search**
        - **version**
        - **info**

3. **Dockerfile基础**
