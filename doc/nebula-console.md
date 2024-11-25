# 安装nebula-consle

1.构建nebula-console
    在构建nebula-console之前首先确保已经安装好了[Go](https://go.dev/doc/install)

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
### From Source Code

1. Build NebulaGraph Console

    To build NebulaGraph Console, make sure that you have installed [Go](https://golang.org/doc/install).

    > NOTE: Go version provided with apt on ubuntu is usually "outdated".

    Run the following command to examine if Go is installed on your machine.

    ```bash
    $ go version
    ```

    The version should be newer than 1.13.

    Use Git to clone the source code of NebulaGraph Console to your host.

    ```bash
    $ git clone https://github.com/vesoft-inc/nebula-console
    ```

    Run the following command to build NebulaGraph Console.

    ```bash
    $ cd nebula-console
    $ make
    ```
    You can find a binary named `nebula-console`.

2. Connect to NebulaGraph

    To connect to your Nebula Graph services, use the following command.

    ```bash
    $ ./nebula-console -addr <ip> -port <port> -u <username> -p <password>
        [-t 120] [-e "nGQL_statement" | -f filename.nGQL]
    ```
