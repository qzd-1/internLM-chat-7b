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































    +
