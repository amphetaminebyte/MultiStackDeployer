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
            复制代码
            docker run --annotation "organization=com.example" ubuntu
            # 这为容器设置了一个注解，指定组织为 com.example。
            ```

            - ***-a, --attach***

            **作用:** 附加到容器的 STDIN、STDOUT 或 STDERR。

            ```bash
            复制代码
            docker run -a stdout -a stderr -i ubuntu /bin/bash
            # 这个命令将启动一个 Ubuntu 容器，并附加到容器的 STDOUT 和 STDERR。
            ```

            - ***--blkio-weight***

            **作用:** 设置块 I/O 操作的相对权重（在 10 到 1000 之间，0 为禁用）。

            ```bash
            复制代码
            docker run --blkio-weight 300 ubuntu
            # 这将为容器的块 I/O 操作设置权重为 300。
            ```

            - ***--blkio-weight-device***

            **作用:** 设置特定设备的块 I/O 权重。

            ```bash
            复制代码
            docker run --blkio-weight-device "/dev/sda:200" ubuntu
            # 这将设置 /dev/sda 设备的块 I/O 权重为 200。
            ```

            - ***--cap-add***

            **作用:** 向容器添加 Linux capabilities。

            ```bash
            复制代码
            docker run --cap-add NET_ADMIN ubuntu
            # 这个命令向容器添加了 NET_ADMIN 权限，允许它执行网络管理任务。
            ```

            - ***--cap-drop***

            **作用:** 从容器中删除特定的 Linux capabilities。

            ```bash
            复制代码
            docker run --cap-drop ALL --cap-add NET_BIND_SERVICE ubuntu
            # 这个命令删除了容器的所有默认权限，只添加了 NET_BIND_SERVICE 权限，允许它绑定到系统端口（端口号小于 1024）。
            ```

            - ***--cgroup-parent***

            **作用:** 设置容器的父 cgroup。

            ```bash
            复制代码
            docker run --cgroup-parent=/parent_cgroup ubuntu
            # 这将容器放在 /parent_cgroup 下作为一个子 cgroup 运行。
            ```

            - ***--cgroupns***

            **作用:** 设置容器使用的 cgroup 命名空间。

            1. **host**: 在 Docker 主机的 cgroup 命名空间中运行容器。
            2. **private**: 在它自己的私有 cgroup 命名空间中运行容器。
            3. **空字符串**: 使用由守护进程的 default-cgroupns-mode 选项配置的 cgroup 命名空间。

            ```bash
            复制代码
            docker run --cgroupns=host ubuntu
            # 这个命令将容器运行在 Docker 主机的 cgroup 命名空间中，共享主机的资源限制。
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
