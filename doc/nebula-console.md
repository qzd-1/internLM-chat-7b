## 安装nebula-consle

1.在构建nebula-console之前首先确保已经安装好了[Go](https://go.dev/doc/install)

    运行以下命令检查您的计算机上是否安装了 Go。
    ```bash
    $ go version
    ```

    版本应高于 1.13。
    
    使用Git将NebulaGraph Console源代码克隆到您的主机上。

    ```bash
    $ git clone https://github.com/vesoft-inc/nebula-console
    ```

    运行以下命令构建NebulaGraph Console。

    ```bash
    $ cd nebula-console
    $ make
    ```

    您可以找到一个名为nebula-console的二进制文件。
    
2. 连接到NebulaGraph

    要连接到 Nebula Graph 服务，请使用以下命令。

    ```bash
    $./nebula-console -addr=192.168.10.111 -port 9669 -u root -p nebula
    2021/03/15 15:21:43 [INFO] connection pool is initialized successfully
    Welcome to NebulaGraph!
    ```
