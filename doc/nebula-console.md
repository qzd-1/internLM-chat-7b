# 安装nebula-consle

1. 构建nebula-console
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
    $./nebula-console -addr=127.0.0.1 -port 9669 -u root -p nebula
    2021/03/15 15:21:43 [INFO] connection pool is initialized successfully
    Welcome to NebulaGraph!
    ```

3. 增加 Storage 主机
    ```bash
    nebula> ADD HOSTS 127.0.0.1:9559;
    nebula> ADD HOSTS 127.0.0.1:9779;
    ```
   > NOTE:
   > - 增加 Storage 主机在下一个心跳周期之后才能生效，为确保数据同步，请等待 2 个心跳周期（20 秒），然后执行SHOW HOSTS查看是否在线。
   > 
   > - IP地址和端口请和配置文件中的设置保持一致，例如单机部署的默认为127.0.0.1:9779。
   > 
   > - 使用域名时，需要用引号包裹，例如ADD HOSTS "foo-bar":9779。
   > 
   > - 确保新增的 Storage 主机没有被其他集群使用过，否则会导致添加 Storage 节点失败。
