## 相关

- 课程链接: [轻松玩转Kubernetes](https://edu.huaweicloud.com/certifications/32f661c5c9a142c8ad3ef050cce337a2)

# 轻松玩转Kubernetes

## 目录

1. Kubernetes 概述
2.  Kubernetes 云上环境搭建
3.  Kubernetes 核心概念
4. 华为云 CCE flappybird实践

## 1、Kubernetes 概述

### 什么是容器？

容器为 App 提供独立的、受控的运行环境，是一种轻量级的操作系统虚拟化。

Concept for create environment for software, without disturbing the rest of the core operating system.

简单的容器 sandbox，沙盒

### 容器基本概念

- 容器关键概念
  - 容器
  - 镜像
- 容器关键技术
  - Cgroup: 容器资源管理
  - NameSpace: 容器隔离

容器时代的“双城记”

- docker - 共用系统内核
- kubernetes/k8s - 多容器管理

### kubernetes 是什么 / 组成

- master - 管理节点：管理和控制
- node - 计算节点：工作负载，默认 docker 作为容器引擎

### Master 节点

master 节点提供的集群控制，对集群作出全局性决策（例如调度）通常在master不运行用户容器

包括:
- API Server：整个系统的对外接口
- Scheduler：集群内部的资源调度
- Controller manger：负责管理控制器
- etcd：kubernetes 的后端存储：备份计划

### Node节点

节组建云顶在每一个 node 节点上，维护运行的pod并提供 kubernetes 运行时环境

- Pod：Kubernetes 最基本的操作单元
- Docker：创建容器
- Kubelet：负责监视指派到它所在的 Node 上的 Pod，包括创建、修改、监控、删除等；(master节点通过该结点实现调度)
- Kube-proxy
- Fluented

master 节点和 node 节点交互

![2020-02-01-22-44-25-b57647facec3231c.png](https://imagehost-cdn.frytea.com/images/2020/02/01/2020-02-01-22-44-25-b57647facec3231c.png)

## 2、Kubernetes 云上环境搭建

### CCE - 基于开源 k8s、Docker技术的企业级容器服务

云容器引擎（Cloud COntainer Engine, CCE） 是基于业界主流的 Docker 和 KUbernetes 开源技术构建的容器服务，提供众多契合企业大规模容器集群场景的功能，在系统可靠性、高性能、开源社区兼容性等多个方面具有独特的优势，满足企业在构建容器云方面的各种需求。

CCE优势
- 简单易用
- 高性能
- 安全可靠
- 开放兼容

### 怎样管理 K8s 集群

- Kubernetes集群的管理
  - Web-Ui
    - CCE 控制台
    - Dashboard
  - Kubelet
    - WebTerminal
    - NOde + EIP

###  华为云 Kubernetes 环境快速搭建架构

- CCE 快速构建 Kubernetes
- Kubernetes 的访问

![2020-02-02-18-57-38-56a6c27c3969d362.png](https://imagehost-cdn.frytea.com/images/2020/02/02/2020-02-02-18-57-38-56a6c27c3969d362.png)

### Kubernetes 环境管理

1. 进行 Kubectl 及配置文件下载
2. 下载 KUbectl 和 Kubectl 配置文件
3. Kubectl 客户端服务器购买
4. 集群中管理节点安全组设置
5. 安装和使用 Kubectl

使用 Kubernetes 只需一个部署文件，使用一条命令就可以部署多层容器（前端，后台等）的完整集群：

```
$ kubectl create -f single-config-file.yaml
# kubectl 是和 Kubernetes API 交互的命令行程序
```
## 3、Kubernetes 核心概念

### 3.1 Kubernetes 基本管理单元 Pod

- Pod 是 Kubernetes 管理的最小基础甘愿
- 一个 Pod 中封装了：
  - 一个或多个紧耦合的应用容器
  - 存储资源
  - 独立的 IP
  - 容器运行的选项

相同 pod 中的任何容器都将共享相同的名称空间和本地网络。容器可以很容易地与其他容器在相同的容器中进行通信。

> 实践1：Pod 的创建和管理
> 1. Pod 定义文件的上传
> 2. 创建 Pod
> 3. Pod 的管理
> 4. 删除 Pod

### 3.2 Kubernetes 控制器

K8s 中认为 pod 是不可靠的。通过控制器来保证 pod 的高可靠。

#### 有状态应用和无状态应用

有无个性化数据

- 有状态应用
  - 从部署开始，这些容器就开始和上游镜像不同，时间越长差异越大。
- 无状态应用
  - 易于部署和扩展，随时可被替代。

#### 无状态应用控制器 - Depolyment

始终维持 pod 运行数量维持在一个数值。

Replication Controller - RelicaSets - Deployment

> 实践2：Deployment 的创建和管理
> 1. 创建 deployment
> kubectl apply -f deployment.yml
> 2. 查看 Pod
> kubectl get Pod
> 3. 手动删除 Pod
> kubbectl delete pod nginx-
> deployment-67d4b848b4-sghfq
> 4. 扩展 Deployment 数量
> kubectl scale deployment.v1.apps/nginx-deployment --reloicas=4
> kubectl get Pod
> 5. 查看 deployment 状态和数量

#### 有状态应用控制器 - StatefulSet

在具有以下特点时使用 StatefulSets
- 稳定性，唯一的网络标识符
- 稳定性，持久化存储
- 有序的部署和扩展
- 有序的删除和终止
- 有序的自动滚动更新

> 实践3：StatefulSet 的创建和管理
> 1. 在 Winscp 将 StatefulSet 定义文件
> 2. 创建 StatefulSet
> kubectl apply -f statefulset.yml
> 3. 查看 Pod 数量和名称
> kubbectl get pod
> 4. 手动删除名称为 web-0 的 pod
> kubectl delete pod web-0
> kubectl get Pod
> 5. 再次查看 pod
> kubectl get pod

#### 系统应用控制器 - DaemonSet

DaemonSet 能够让所有或者特定的Node 节点运行一个 pod。当家电加入到 kubernetes 集群中， pod 会被 （DaemonSet）调度到该节点上运行。当节点从 kubernetes 集群中被清除。调度的 pod 会被移除。

![2020-02-03-16-06-22-70c7b099534347c0.png](https://imagehost-cdn.frytea.com/images/2020/02/03/2020-02-03-16-06-22-70c7b099534347c0.png)

> 实践4：DaemonSet 的创建和管理
> 1. 在 Winscp 中上传 daemonset.yml 文件至 ecs-k8s
> 2.查看 kube=system 命令空间中的 Daemon Set
> kubectl get ds -n kube-system
> 3. 创建 daemonset
> kubbectl apply
 -f daemonset.yml
> 4. 再次查看 kube-system 中的个 DaemonSet
> kubectl get ds -n kube-system
> 5. 在 cce 中购买节点
> 6. 查看各个 DaemonSet 实例数
> kubectl get ns -n kube-system

#### 临时任务控制器 - Job

- job - 用完即焚
- CronJob - 定期执行

> 实践5：Job 的创建和管理
> 1. 在 Winscp 中上传 job.yml
> 2.运行 job
> kubectl apply -f job.yml
> 3. 查看 job 运行状态
> kubbectl get job
> 4. 查看 job 的输出
> kubectl get pod
> kubectl logs pi-c5pgr

### 3.3 Pod 集群网络

#### 应用访问 - service

![2020-02-03-16-15-43-b04b95299a3c04a6.png](https://imagehost-cdn.frytea.com/images/2020/02/03/2020-02-03-16-15-43-b04b95299a3c04a6.png)

#### Kubernetes 应用间互访 - Cluster IP

![2020-02-03-16-16-36-02e9ab04528d6d3e.png](https://imagehost-cdn.frytea.com/images/2020/02/03/2020-02-03-16-16-36-02e9ab04528d6d3e.png)

#### Kubernetes 集群外互访 - NodePort

![2020-02-03-16-18-27-85ebaf6310bf8aa4.png](https://imagehost-cdn.frytea.com/images/2020/02/03/2020-02-03-16-18-27-85ebaf6310bf8aa4.png)

#### 公网访问 - LoadBalancer

![2020-02-03-16-19-35-58a35f1ee18e2c9e.png](https://imagehost-cdn.frytea.com/images/2020/02/03/2020-02-03-16-19-35-58a35f1ee18e2c9e.png)

> 实践6：service 的创建和管理

### 3.4 资源的隔离

#### 命名空间 - NameSpace

- default
- kube-system
- kube-public

> 实践7：命名空间的创建和管理

## 4、华为云 CCE flappybird实践

1. 云容器引擎控制台，点击“创建无状态工作负载”
2. 添加容器
3. 添加服务
4. 公网访问
