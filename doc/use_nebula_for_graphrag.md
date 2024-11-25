## 安装NebulaGraph

#### 下载安装包
例如要下载适用于Centos 7.5的3.8.0安装包：
```bash
wget https://oss-cdn.nebula-graph.com.cn/package/3.8.0/nebula-graph-3.8.0.el7.x86_64.rpm
wget https://oss-cdn.nebula-graph.com.cn/package/3.8.0/nebula-graph-3.8.0.el7.x86_64.rpm.sha256sum.txt
```

下载适用于ubuntu 1804的3.8.0安装包：
```bash
wget https://oss-cdn.nebula-graph.com.cn/package/3.8.0/nebula-graph-3.8.0.ubuntu1804.amd64.deb
wget https://oss-cdn.nebula-graph.com.cn/package/3.8.0/nebula-graph-3.8.0.ubuntu1804.amd64.deb.sha256sum.txt
```

#### 安装
安装RPM包
```bash
$ sudo rpm -ivh --prefix=<installation_path> <package_name>
```
--prefix为可选项，用于指定安装路径。如不设置，系统会将 NebulaGraph 安装到默认路径/usr/local/nebula/。

例如，要在默认路径下安装3.8.0版本的 RPM 包，运行如下命令：
```bash
sudo rpm -ivh nebula-graph-3.8.0.el7.x86_64.rpm
```

## 启动NebulaGraph:
使用脚本管理服务
使用脚本 `nebula.service` 管理服务，包括启动、停止、重启、中止和查看。
> `nebula.service` 的默认路径是 `/usr/local/nebula/scripts`，如果修改过安装路径，请使用实际路径。

语法：
#### 启动nebulagraph服务：
```bash
$ sudo /usr/local/nebula/scripts/nebula.service start all
[INFO] Starting nebula-metad...
[INFO] Done
[INFO] Starting nebula-graphd...
[INFO] Done
[INFO] Starting nebula-storaged...
[INFO] Done
```

#### 停止nebaldgraph服务：
> [Danger]
> 请勿使用kill -9 命令强制终止进程，否则可能较小概率出现数据丢失。
```bash
$ sudo /usr/local/nebula/scripts/nebula.service stop all
[INFO] Stopping nebula-metad...
[INFO] Done
[INFO] Stopping nebula-graphd...
[INFO] Done
[INFO] Stopping nebula-storaged...
[INFO] Done
```

#### 查看 NebulaGraph 服务:
```bash
$ sudo /usr/local/nebula/scripts/nebula.service status all
```

如果返回如下结果，表示 NebulaGraph 服务正常运行。

```text
[INFO] nebula-metad(33fd35e): Running as 29020, Listening on 9559
[INFO] nebula-graphd(33fd35e): Running as 29095, Listening on 9669
[WARN] nebula-storaged after v3.0.0 will not start service until it is added to cluster.
[WARN] See Manage Storage hosts: ADD HOSTS in https://docs.nebula-graph.io/
[INFO] nebula-storaged(33fd35e): Running as 29147, Listening on 9779
```
> ### Note
> 从 3.0.0 版本开始，在配置文件中添加的 Storage 节点无法直接生效，配置文件的作用仅仅是将 Storage 节点注册到 Meta 服务中。必须使用 `ADD HOSTS` 命令后，才能正常读写 Storage 节点。
> 方式一：在连接池中得到会话之后执行ADD HOSTS命令
> ```python
> ip = "127.0.0.1"
> port = 9669
> config = Config()
> self.connection_pool = ConnectionPool()
> assert self.connection_pool.init([(ip, port)], config)
> self.session = self.connection_pool.get_session('root', 'nebula')
> # 执行ADD HOSTS命令
> resp = session.execute('ADD HOSTS 127.0.0.1:9559')
> resp = session.execute('ADD HOSTS 127.0.0.1:9779')
> ```

> 方式二：安装 [nebula-console](./docs/nebula-console.md)后，启动服务之后并执行ADD HOSTS命令。

















