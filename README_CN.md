# Pact Broker SaaS

> **Source**: https://github.com/tongzh/pact-broker-saas

Pact Broker SaaS是一个支持多租户的Pact契约文件中间件服务。

## 特性

### 多租户

通过容器隔离实现物理多租。通过docker-compose为每个租户拉起PactBroker和所依赖的postgresql数据库服务，每个租户的服务通过端口进行区分。

## 运维操作

### 新增租户

新增租户选择端口号时应避免与已有端口号冲突，可以通过`docker ps | grep pact_broker`命令来查看已占用的端口号。

新增租户运行命令

`./deploy <tenant_name> <tenant_port>`

例如，新租户名为tenant，为其在12999端口上提供服务，则运行以下命令：

`./deploy tenant 12999`

即可创建出对应的pact_broker容器与pact_db容器，通过12999端口访问服务。

### 停止租户容器运行

停止租户容器（包括pact_broker与pact_db两个容器），运行命令：

`./stop <tenant_name>`

例如，停止租户tenant的容器，则运行以下命令：

`./stop tenant`

### 删除租户容器和数据卷

删除已停止运行的租户容器（包括pact_broker与pact_db两个容器），并删除数据卷，运行命令：

`./remove <tenant_name>`

例如，删除租户tenant的容器及数据卷，则运行以下命令：

`./remove tenant`

### 备份租户数据

将租户数据备份到当前文件夹下的`<tenant_name>-data.tar`文件中，运行命令：

`./backup <tenant_name>`

例如，要备份租户tenant的数据，则运行以下命令：

`./backup tenant`

租户的数据库数据会被备份到当前文件夹下的`tenant-data.tar`文件中。

### 恢复租户数据

首先在当前目录下准备数据源文件，文件名必须是`<tenant_name>-data.tar`形式，运行命令：

`./restore <tenant_name>`

例如，要恢复租户tenant的数据，则运行以下命令：

`./restore tenant`

就会从当前文件夹下的`tenant-data.tar`文件中恢复租户数据，并且重启postgresql容器（由于postgresql本身的机制，必须重启容器才能读取到被恢复的数据）。
