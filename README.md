# 诸葛神码部署工具(for docker-compose)

## 简介

### 整体思路

诸葛神码采用微服务架构。

整个后端系统大概分为三层四块，三层分别是：网关层，服务层，存储层。这三层再加上贯穿三层架构的统一的运维中心，共四大块。

#### 网关层

网关层负责应用分发、负载均衡、流量控制、API授权控制

- 流量转发，ssl卸载： 推荐使用Sangfor AD，也可以使用其它具有ssl卸载能力的负载均衡设备完成
- 应用分发、流量控制等: apisix
- 登录认证、授权控制：keycloak,trampoline,kaptcha
  - 用户管理组件：keycloak
  - 登录跳板：trampoline
  - 登录过程使用的验证码服务：kaptcha

#### 服务层

服务层即几大核心服务，目前包括：

- 负责代码补全的代理后端：completion-server
- 负责对话服务的代理后端: chat-server

对话和补全，都使用代理后端的目的是，屏蔽不同模型API的细节，并提供额外的上下文处理等能力。

#### 存储层

存储层：

- 关系数据库: pgsql
- 键值数据库: etcd
- 缓存: redis

#### 运维中心

运维中心：

- grafana(可选)
- prometheus(可选)
- kibana(可选)
- elasticsearch(必选)

## 部署步骤

### 0. 前提条件

#### 使用自己部署的模型实例

1. 一台最低配置16C，32G，512G存储的X64硬件设备，具有支持模型推理服务运行的显卡(至少2张RTX4090,或1张A800)
2. 安装好centos7或wsl ubuntu,nvidia-docker,docker-compose等必要组件

#### 使用第三方API服务，或自行部署模型实例

1. 一台最低配置16C，32G，512G存储的X64硬件设备
2. 安装好centos7,docker,docker-compose等必要组件

### 1. 根据需求，修改配置

```sh
vim deploy.sh
vim configure.sh
```

### 2. 执行部署脚本
```shell
bash deploy.sh
```

### 3. 在one-api后台页面配置大模型api-key
     默认的地址 http://localhost:30000，默认账号root，密码123456.
     点击渠道，新增渠道，选择大模型供应商，填写名字、key即可，其余可不填写。

### 4. 在zhuge shenma插件配置apisix地址
    zhuge shenma baseurl: 默认 http://{本机ip}:8090/v1 （注：使用localhost可能有问题，建议ipconfig获取实际IP地址）。

