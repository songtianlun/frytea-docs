# 轻松玩转Kubernetes 实验

## 实验目录

- 1、Kubernetes 环境搭建和基础管理
- 2、Kubernetes 核心概念实践
- 3、CCE 搭建 flappybird
- 4、资源释放

注意事项
实验资源一旦购买就开始计费,请合理安排时间进行实验,并注意以下几点:
-  本实验预计 2 小时完成,实验结束后请根据实验指导进行释放资源;
 - 实验在在华北-北京四区域进行,选择实验规格时,若资源售罄,可选相近资源;
 - 若实验中途离开或中断,建议释放实验资源,否则将会按照购买的资源继续计费。

## 1 Kubernetes 环境搭建和基础管理

### 1.1 Kubernetes 环境搭建

步骤 1
打开华为公有云:https://www.huaweicloud.com/,输入账号和密码进行登录,若无公
有云账号则需先按要求进行注册。

步骤 2
选择产品>基础服务>网络>虚拟私有云 VPC,访问控制台。

步骤 3
单击创建虚拟私有云,参数配置如下:

- 区域:华为-北京四
- 名称:vpc-K8s,其他参数默认

步骤 4
点击“立即创建”,单击返回虚拟私有云列表,查看已经创建的 vpc。

步骤 5
保证区域在北京四区域,点击左上角控制台,进入华为云控制台,选择所有服务>计算,
单击云容器引擎 CCE。

步骤 6
页面跳转至 CCE 控制台。

步骤 7
单击资源管理,页面跳转至集群管理界面,单击混合集群的购买按钮。

步骤 8
按照以下参数配置 Kubernetes 集群信息。完成配置后,单击下一步。
- 计费模式:按需付费
- 区域:华为-北京四
- 集群名称:k8s-demo
- 版本:v1.13.10(选择最新版本)
- 集群管理规模:50 节点
- 高可用:是
- 虚拟私有云:vpc-K8s;所在子网:默认
- 网络模型:容器隧道网络
- 容器网段:自动选择
- 服务网段:不设置
- 鉴权方式:RBAC
- 其它:保持默认

步骤 9
按照以下信息配置节点参数,完成配置后单击下一步。
- 创建节点:现在添加
- 计费模式:按需付费
- 当前区域:华北-北京四
- 可用区:可用区一
- 节点类型:虚拟机节点
- 节点名称:k8s-demo-node
- 节点规格:通用型 s6.large.2 /2 核 4GB(若实验时,官网无此规格,可选相近规格节
点,实验不受影响)
- 操作系统:CentOS7.6
- 弹性 IP:现在购买;购买数量:1
- 规格:全动态 BGP(若界面无此类型,可选静态)
;
- 计费模式:按流量付费,独享
- 带宽大小:5Mbit/s
- 登录方式:密码
- 密码:Huawei@123!
- 节点购买数量:2
- 其它:保存默认即可

步骤 10
安装插件,全部保存默认,单击立即购买。

步骤 11
确认信息,单击提交按钮。

步骤 12
等待 6-10 分钟,完成 Kubernetes 集群的创建。单击返回集群管理。

步骤 13
等待集群状态由创建中变为正常,可用节点数为 2,此时 Kubernetes 环境搭建完成。

### 1.2 Kubernetes 环境管理

#### 1.2.1 进行 Kubectl 及配置文件下载

步骤 1
单击命令行工具,选择 kubectl。

步骤 2
进入 kubectl,往下找到步骤二: 使用 kubectl 操作集群。

步骤 3
分别单击此处下载 kubectl 和 kubectl 配置文件。

#### 1.2.2 Kubectl 客户端服务器购买

步骤 1
返回华为云控制台,选择服务列表>计算>弹性云服务器 ECS。

步骤 2
单击购买弹性云服务器。

步骤 3
按照以下参数进行弹性云服务器的购买。
- 计费模式:按需计费
- 区域:华北-北京四
- 可用区:随机分配
- CPU 架构:X86 架构
- 规格:通用计算型 s6.small.1 1vCPU/1GB(若页面无此规格,可选相近规格)
- 镜像:公共镜像 CentOS 7.5
- 系统盘:高 IO,40G

下一步:网络配置
- 网络:使用已创建的 vpc
- 弹性公网 IP:现在购买/全动态 BGP/独享带宽/按带宽计费/1Mbit/s

下一步:高级配置
- 云服务名称:ecs-k8s
- 密码:建议为 Huawei@123!(可自定义);其他参数默认

下一步:确认配置,确认配置后,立即购买。

步骤 4
返回云服务器列表,找到创建的 ecs-k8s。确认其公网 IP 地址。

步骤 5
分别单击服务列表>网络>虚拟私有云 VPC,进入 VPC 控制台。

步骤 6
单击安全组。

步骤 7
找到 Kubernetes 集群管理节点的安全组。

步骤
8 单击配置规则,选择添加 SSH(22)规则。

#### 1.2.3 安装和使用 kubectl

> 从以下链接获取实验所使用软件包和文件: <https://weirenzheng.obs.cn-north-
1.myhuaweicloud.com/kubernetes%20software%26yaml.rar>

步骤 1
打开 winscp 主机名输入 ecs-k8s 的公网 IP 地址,用户名为 root,密码为
Huawei@123!。

步骤 2
将 kubectl 和 kubectl 配置文件(在刚才下载两个文件存放的目录里)上传至 ecs-k8s 的
/home 目录。(可直接选中左边的文件直接拖到左边目录 home 下即可)。

步骤 3
等待文件上传成功后,打开 Putty 软件,HostName 中输入购买的弹性云服务器 ecs-
k8s 的公网 IP 地址。

步骤 4
输入创建云服务器 ecs-k8s 时的用户和密码。(密码在输入时不显示)

步骤 5
在 putty 中,分别执行以下命令:

```
cd /home
chmod +x kubectl
mv -f kubectl /usr/local/bin
mkdir -p $HOME/.kube
mv -f kubeconfig.json $HOME/.kube/config
```

步骤 6
此次为 VPC 内接入访问,因此请执行以下命令。

```
kubectl config use-context internal
```

步骤 7
通过以下命令查看 kubernetes 集群信息。

```
kubectl cluster-info
```

步骤 8
通过以下命令获取集群节点信息。
```
kubectl get nodes
```

## 2 Kubernetes 核心概念实践

### 2.1 POD 实践

> 从以下链接获取实验所使用软件包和文件: <https://weirenzheng.obs.cn-north-
1.myhuaweicloud.com/kubernetes%20software%26yaml.rar>

#### 2.1.1 创建 POD1(1POD:1 容器)

步骤 1
登录 winscp,通过 winscp 将下载的附件中的 yml 文件中 POD-1Container.yml 上传至
客户端服务器/home 目录。(左侧点击文件直接拖过去即可)

步骤 2
输入以下指令,服务器上终端查看文件是否上传成功。

```
ls
cat POD-1Container.yml
```

步骤 3
通过以下命令创建 POD。

```
kubectl apply –f POD-1Container.yml
```

步骤 4
通过以下命令查看 POD 的运行状态为 Running。


```
kubectl get pod
```

步骤 5
通过以下命令查看 POD 运行在哪台弹性云服务器上。

```
kubectl get pod –o wide
```

步骤 6
在 putty 中实行 ssh NodeIP 命令,登录到 node 节点。

```
ssh 192.168.0.227 (换成自己实践时的私网地址)
```

注意: 密码为前面购买集群节点时的密码,如示例是 Huawei@123!。

步骤 7
执行以下命令查看此节点上与刚刚创建 POD 相关容器实例的信息。

```
docker container ls |grep nginx
```

注意:通过观察可以发现容器名称带有 nginx 的有两个,一个使用的镜像名称为 nginx,另
外一个容器使用的镜像名称。

步骤 8
输入 exit 退出 Node 节点。

```
exit
```

步骤 9
然后执行以下命令删除 POD 并确认删除成功。

```
kubectl delete pod nginx
kubectl get pod
```

#### 2.1.2 指定 POD 运行到指定的 NODE 上

步骤 1
通过以下命令查看 NODE 节点信息。

```
kubectl get node -a
```

步骤 2
通过以下命令给第一个节点打上 node=test 的标签。

```
kubectl label nodes 192.168.0.227 node=test (换成自己 node 节点的 IP 地址)
```

步骤 3
通过以下命令查看上述节点标签信息。

```
kubectl describe node 192.168.0.227
```

步骤 4
通过以下命令查看集群中带有 node=test 标签的节点信息。

```
kubectl get node -a -l "node=test"
```

步骤 5
通过 winscp 上传 POD-NodeSelector.yml 文件至客户端服务器/Home 目录。

步骤 6
返回 putty 客户端服务器确认并查看文件。

```
ls
cat POD-NodeSelector.yml
```

步骤 7
运行以下命令创建 POD。

```
kubectl apply -f POD-NodeSelector.yml(当直接复制命令到 putty 出错时,可进行手敲进去尝
试)
```

步骤 8
查看并确认 POD 运行节点是否为指定的节点。

```
kubectl get pod –o wide
```

#### 2.1.3 创建 POD2(1POD:2 容器)

步骤 1
通过 winscp 上传 POD-2Container.yml 容器定义文件。

步骤 2
通过 putty 确认文件上传。

```
ls
cat POD-2Container.yml
```

步骤 3
通过以下命令创建 POD。

```
kubectl apply -f POD-2Container.yml
```

步骤 4
通过以下命令查看 POD 运行状态。

```
kubectl get pods
```

步骤 5
通过以下命令查看 POD 运行在哪个 Node 上。

```
kubectl get pod –o wide
```
(可网页返回 ECS 控制台查看发现 POD 运行在另外一台节点上)


步骤 6
SSH 登录到 POD 所在的节点。

```
ssh 192.168.0.7
```

步骤 7
通过以下命令查看和 my-app-pod 相关的容器。

```
docker container ls | grep two-container
```

步骤 8
退出 node 节点。

```
exit
```

步骤 9
查看 kubernetes 集群 POD 信息并删除。

```
kubectl get pod
kubectl delete pod nginx
kubectl delete pod two-containers
kubectl get pod
```

### 2.2 Deployment 实践

步骤 1
将 Deployment 定义文件 deployment.yml 上传至 ecs-k8s。

步骤 2
在 putty 中查看文件是否正确上传。

```
ls
cat deployment.yml
```

步骤 3
通过以下命令创建 depolyment。

```
kubectl apply –f deployment.yml
```

步骤 4
通过以下命令查看 deployment 状态。

```
kubectl get deployment
```

步骤 5
通过以下命令查看 POD。

```
kubectl get pod
```

步骤 6
通过以下命令手动删除上述查看到的 POD。

```
kubbectl delete pod nginx-deployment-67d4b848b4-sghfq
```

步骤 7
再次查看 POD。

```
kubectl get pod
```

步骤 8
通过以下命令扩展 Deployment 数量至 4。

```
kubectl scale deployment.v1.apps/nginx-deployment --replicas=4
kubectl get pod
```

步骤 9
再次查看 deployment 状态和数量。

步骤 10
通过以下命令删除 deployment 并查看。

```
kubbectl delete deployment nginx-deployment
kubectl get deployment
```

步骤 11
再次查看 POD。

```
kubectl get pod
```

### 2.3 StatefulSet 实践

步骤 1
在 Winscp 将 StatefulSet 定义文件 statefulset.yml 上传至 ecs-k8s。

步骤 2
在 putty 中查看文件是否正确上传。

```
ls
cat statefulset.yml
```

步骤 3
通过以下命令创建 StatefulSet。

```
kubetcl apply –f statefulset.yml
```

步骤 4
通过以下命令查看 StatefulSet 的运行状态。

```
kubetcl get statefulset(建议自己手动输入)
```

步骤 5
通过以下命令查看 POD 数量和名称。

```
kubectl get pod
```

步骤 6
通过以下命令查看 web-0 的 POD 的名称、IP 地址和所在的节点信息。

```
kubectl describe pod web-0
```

步骤 7
手动删除名称为 web-0 的 POD。

```
kubectl delete pod web-0
```

步骤 8
再次查看 POD。

```
kubectl get pod
```

步骤 9
再次查看 web-0 的 POD 的名称、IP 地址和所在的节点信息是够发生变化。

```
kubectl describe pod web-0
```

重复删除 web-0 发现 StatefulSet 的 POD IP、名称和所在节点的规律。


### 2.4 DaemonSet 实践

步骤 1 在 winscp 中上传 daemonset.yml 文件至 ecs-k8s。

步骤 2 通过以下命令查看 kube-system 命令空间中的 DaemonSet。

```
kubectl get ds –n kube-system
```

步骤 3 通过以下命令创建 daemonset。

```
kubectl apply –f daemonset.yml
```

步骤 4 再次查看 kube-system 中的 DaemonSet。

```
kubectl get ds –n kube-system
```

步骤 5
返回华为云 CCE 控制台,选择资源管理>节点管理,单击购买节点。
购买节点信息如下,确认信息后,单击提交按钮。
- 计费模式:按需计费;区域:北京四;操作系统:CentOS7.6
- 密码登录;节点购买数:1 台
- 其他参数默认(可参考下图)

步骤 6 等待节点运行状态为可用。

步骤 7 返回 putty,通过命令行查看各个 DaemonSet 实例数是不是 3。

### 2.5 Job 实践

步骤 1 通过 winscp 将 Job.yml 上传至 ecs-k8s。

步骤 2 通过以下命令运行 Job。

```
kubectl apply –f Job.yml
```

步骤 3
通过以下命令查看 job 运行状态。

```
kubectl get job
```

步骤 4
通过以下命令查看是否有 pi 开头的 POD。

```
kubectl get pod
```

步骤 5
等待 10S 左右,再次查看 Job 的运行状态。

```
kubectl get job
```

步骤 6
通过以下命令查看此 Job 的输出。

```
kubectl get pod
kubectl logs pi-c5pgr
```

### 2.6 Service 实践

步骤 1
通过之前上传的 deployment 文件创建 Deployment。

```
kubectl apply -f deployment.yml
```

步骤 2
通过命令行查看 Deployment 状态和 POD 状态。

```
kubectl get deployment
kubectl get pod
```

步骤 3
用户通过命令行创建 NodePort 类型的 Service 并查看。

```
kubectl expose deployment nginx-deployment –type=NodePort
kubectl get service
```

步骤 4
通过 curl 命令验证网站是否可以访问。(任意一个 Kubernetes NODE 节点地址都行。

```
kubectl get node
curl 192.168.0.227:32465(注意端口号为上一步查看到的 NodePort)课程名称+实验指导手册
```

步骤 5 通过华为云控制台查看其中一个 kubernetes Node 绑定的公网地址。

步骤 6 打开浏览器,输入 ECS 绑定的公网地址+Service 的 Nodeport。如本示例中为: 117.78.7.134:32465

### 2.7 NameSpace 实践

####  2.7.1 默认的 NameSpace 实践

步骤 1
通过以下命令查看系统默认的 NameSpace。

```
kubectl get namespace
```

步骤 2
通过以下命令可以手动的创建一个 NameSpace 命名空间并查看。

```
kubectl create namespace test
kubectl get namespace
```

步骤 3
创建一个 POD 并指定此 POD 运行在 test 命名空间。

```
kubectl apply –f POD-1Container.yml –namespace=test
```

步骤 4
查看指定命名空间里的 POD。

```
kubectl get pod –n test
```

#### 2.7.2 创建一个限制 CPU 和内存大小的 NameSpace。

步骤 1
通过以下命令创建一个 Namespace。

```
kubectl create namespace quota-mem-cpu-example
kubectl get ns
```

步骤 2 在 winscpz 中上传 NameSpace 定义文件到 ecs-k8s 中。

步骤 3 通过以下命令查看文件信息。

```
ls
cat ns-cpu-mem.yml
```

步骤 4
创建 ResourceQuota 并和 NameSpace 进行关联。

```
kubectl create –f ns-cpu-men.yml –namespace=quota-mem-cpu-example
```

步骤 5
查看 ResourceQuota 详细信息。

```
kubectl get resourcequota mem-cpu-demo –namespace=quota-mem-cpu-example --
output=yaml
```

#### 2.7.3 添加限制
以上刚创建的 ResourceQuota 对象将在 quota-mem-cpu-example 名字空间中添加以下限
制:
每个容器必须设置内存请求(memory request),内存限额(memory limit)
,cpu 请求
(cpu request)和 cpu 限额(cpu limit)
。
- 所有容器的内存请求总额不得超过 1 GiB。
- 所有容器的内存限额总额不得超过 2 GiB。
- 所有容器的 CPU 请求总额不得超过 1 CPU。
- 所有容器的 CPU 限额总额不得超过 2 CPU

步骤 1
在 winscp 中将 POD 定义文件上传至 ecs-k8s。课程名称+实验指导手册

步骤 2
查看 POD 定义文件。

```
ls
cat quota-mem-cpu-pod.yml
```

步骤 3
创建 POD。

```
kubectl create –f quota-mem-cpu-pod.yml –-namespace=quota-mem-cpu-example
```

步骤 4
运行以下命令确认 POD 已运行。

```
kubectl get pod quota-mem-cpu-demo --namespace=quota-mem-cpu-example
```

步骤 5
然后再次查看 ResourceQuota 对象的详细信息:

```
kubectl get resourcequota mem-cpu-demo --namespace=quota-mem-cpu-example --
output=yaml
```

步骤 6 通过 winscp 上传第二个 POD 定义文件。

步骤 7 查看并确认。

```
ls
cat quota-mem-cpu-pod-2.yml课程名称+实验指导手册
```

步骤 8
通过以下命令创建第二个 POD。

```
kubectl apply –f quota-mem-cpu-pod-2.yml --namespace=quota-mem-cpu-example
```

以下命令输出显示第二个 Pod 并没有创建成功。错误信息说明了如果创建第二个 Pod,内存
请求总额将超出名字空间的内存请求配额。

## 3 CCE 搭建 flappybird

步骤 1 返回云容器引擎控制台,点击“创建无状态工作负载”

步骤 2 工作负载基本信息配置如下
- 工作负载名称:flappybird
- 集群名称:选择前面创建的 CCE 集群,如 k8s-demo
- 实例数量:1 个

步骤 3
点击下一步,添加容器,点击“第三方镜像”,选择镜像相关参数
镜像名称:swr.cn-north-1.myhuaweicloud.com/hc_cce/flappybird:latest课程名称+实验指导手册

步骤 4 容器设置选择默认,点击下一步。

步骤 5 添加服务,如下参数配置后点击确定。
- 访问类型:负载均衡,服务名称:flappybird;集群级别
- 端口配置,容器端口:80;访问端口:80
- 其他参数默认(可参照下图参数配置)

步骤 6
高级设置选择默认,点击创建。

步骤 7 返回工作负载,flappybird 的状态是运行中时,进行验证。

步骤 8 点击外部访问地址的链接。如果出现此界面,则游戏部署成功。

## 4 资源释放

步骤 1
进入云容器引擎 CCE 控制台。单击集群管理。选中实验中创建的集群,更多>删除集群
勾选集群所有选项,然后输入 DELETE,单击确定。课程名称+实验指导手册

步骤 2
进入云服务器控制台。勾选 ecs-k8s,单击更多>删除。
勾选所有选项,单击是。
单击资源>我的资源。

确认所有计费资源都已删除。
