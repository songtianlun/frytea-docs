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
打开华为公有云: https://www.huaweicloud.com/ ,输入账号和密码进行登录,若无公
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
- 规格:全动态 BGP(若界面无此类型,可选静态);
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

运行结果：

```
[root@ecs-sn3-medium-2-linux-20200204152547 ~]#  rm -rf $HOME/.kube/config/kubeconfig.json
[root@ecs-sn3-medium-2-linux-20200204152547 ~]# cd /home
[root@ecs-sn3-medium-2-linux-20200204152547 home]# mv -f kubeconfig.json $HOME/.kube/config
mv: cannot move ‘kubeconfig.json’ to ‘/root/.kube/config’: No such file or directory
[root@ecs-sn3-medium-2-linux-20200204152547 home]# cd /home
[root@ecs-sn3-medium-2-linux-20200204152547 home]# chmod +x kubectl
[root@ecs-sn3-medium-2-linux-20200204152547 home]# mv -f kubectl /usr/local/bin
[root@ecs-sn3-medium-2-linux-20200204152547 home]# mkdir -p $HOME/.kube
[root@ecs-sn3-medium-2-linux-20200204152547 home]# mv -f kubeconfig.json $HOME/.kube/config
[root@ecs-sn3-medium-2-linux-20200204152547 home]# kubectl config use-context internal
Switched to context "internal".
```

步骤 6
此次为 VPC 内接入访问,因此请执行以下命令。

```
kubectl config use-context internal
```

运行结果：

```
Kubernetes master is running at https://192.168.0.33:5443
CoreDNS is running at https://192.168.0.33:5443/api/v1/namespaces/kube-system/services/coredns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

步骤 7
通过以下命令查看 kubernetes 集群信息。

```
$ kubectl cluster-info
Kubernetes master is running at https://192.168.0.33:5443
CoreDNS is running at https://192.168.0.33:5443/api/v1/namespaces/kube-system/services/coredns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

步骤 8
通过以下命令获取集群节点信息。
```
$ kubectl get nodes
NAME            STATUS    ROLES     AGE       VERSION
192.168.0.163   Ready     <none>    17m       v1.13.10-r1-CCE2.0.28.B001
192.168.0.71    Ready     <none>    17m       v1.13.10-r1-CCE2.0.28.B001
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
$ kubectl apply -f POD-1Container.yml
pod/nginx created
```

步骤 4
通过以下命令查看 POD 的运行状态为 Running。


```
$ kubectl get pod
NAME      READY     STATUS    RESTARTS   AGE
nginx     1/1       Running   0          79s
```

步骤 5
通过以下命令查看 POD 运行在哪台弹性云服务器上。

```
$ kubectl get pod -o wide
NAME      READY     STATUS    RESTARTS   AGE       IP            NODE            NOMINATED NODE   READINESS GATES
nginx     1/1       Running   0          53s       172.16.0.51   192.168.0.163   <none>           <none>
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
$ docker container ls |grep nginx
956f93169644        nginx                                                      "nginx -g 'daemon of…"   6 minutes ago       Up 6 minutes                            k8s_nginx_nginx_default_39b76734-473b-11ea-b363-fa163e461ce6_0
1fed53b402e3        cce-pause:2.0                                              "/pause"                 6 minutes ago       Up 6 minutes                            k8s_POD_nginx_default_39b76734-473b-11ea-b363-fa163e461ce6_0
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
$ kubectl delete pod nginx
pod "nginx" deleted
$ kubectl get pod
No resources found.
```

#### 2.1.2 指定 POD 运行到指定的 NODE 上

步骤 1
通过以下命令查看 NODE 节点信息。

```
$ kubectl get node -a
NAME            STATUS    ROLES     AGE       VERSION
192.168.0.163   Ready     <none>    28m       v1.13.10-r1-CCE2.0.28.B001
192.168.0.71    Ready     <none>    28m       v1.13.10-r1-CCE2.0.28.B001
```

步骤 2
通过以下命令给第一个节点打上 node=test 的标签。

```
$ kubectl label nodes 192.168.0.163 node=test (换成自己 node 节点的 IP 地址)
node/192.168.0.163 labeled
```

步骤 3
通过以下命令查看上述节点标签信息。

```
$ kubectl describe node 192.168.0.163
Name:               192.168.0.163
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    failure-domain.beta.kubernetes.io/is-baremetal=false
                    failure-domain.beta.kubernetes.io/region=cn-north-4
                    failure-domain.beta.kubernetes.io/zone=cn-north-4c
                    kubernetes.io/availablezone=cn-north-4c
                    kubernetes.io/hostname=192.168.0.163
                    node=test
                    node.kubernetes.io/subnetid=2553e744-157f-4472-9386-99c76910c93c
                    os.architecture=amd64
                    os.name=CentOS_Linux_7_Core
                    os.version=3.10.0-957.21.3.el7.x86_64
Annotations:        huawei.com/gpu-status=[]
                    node.alpha.kubernetes.io/ttl=0
CreationTimestamp:  Tue, 04 Feb 2020 18:22:49 +0800
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Tue, 04 Feb 2020 18:52:31 +0800   Tue, 04 Feb 2020 18:22:49 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Tue, 04 Feb 2020 18:52:31 +0800   Tue, 04 Feb 2020 18:22:49 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Tue, 04 Feb 2020 18:52:31 +0800   Tue, 04 Feb 2020 18:22:49 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Tue, 04 Feb 2020 18:52:31 +0800   Tue, 04 Feb 2020 18:22:49 +0800   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.0.163
  Hostname:    192.168.0.163
Capacity:
 attachable-volumes-hc-all-mode-disk:   58
 attachable-volumes-hc-scsi-mode-disk:  58
 attachable-volumes-hc-vbd-mode-disk:   22
 cce/eni:                               10
 cpu:                                   2
 ephemeral-storage:                     10186004Ki
 hugepages-1Gi:                         0
 hugepages-2Mi:                         0
 memory:                                3880028Ki
 pods:                                  110
Allocatable:
 attachable-volumes-hc-all-mode-disk:   58
 attachable-volumes-hc-scsi-mode-disk:  58
 attachable-volumes-hc-vbd-mode-disk:   22
 cce/eni:                               10
 cpu:                                   1930m
 ephemeral-storage:                     9387421271
 hugepages-1Gi:                         0
 hugepages-2Mi:                         0
 memory:                                2151516Ki
 pods:                                  110
System Info:
 Machine ID:                 4cdc38f4-8c73-4c14-bf62-9534c7800e51
 System UUID:                6A2ACB9C-44E9-497E-A2C7-D6C63BCCA543
 Boot ID:                    583ad1c4-a775-453a-ac7b-417abdc15cc9
 Kernel Version:             3.10.0-957.21.3.el7.x86_64
 OS Image:                   CentOS Linux 7 (Core)
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.9.0
 Kubelet Version:            v1.13.10-r1-CCE2.0.28.B001
 Kube-Proxy Version:         v1.13.10-r1-CCE2.0.28.B001
ProviderID:                  ac34c5a7-4737-11ea-920b-0255ac101d39
Non-terminated Pods:         (3 in total)
  Namespace                  Name                        CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                        ------------  ----------  ---------------  -------------
  kube-system                coredns-7847c6444b-p88vm    500m (25%)    500m (25%)  512Mi (24%)      512Mi (24%)
  kube-system                icagent-ggfqz               0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                storage-driver-trkxv        300m (15%)    300m (15%)  100Mi (4%)       100Mi (4%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource                              Requests     Limits
  --------                              --------     ------
  cpu                                   800m (41%)   800m (41%)
  memory                                612Mi (29%)  612Mi (29%)
  attachable-volumes-hc-all-mode-disk   0            0
  attachable-volumes-hc-scsi-mode-disk  0            0
  attachable-volumes-hc-vbd-mode-disk   0            0
  cce/eni                               0            0
Events:
  Type    Reason                   Age                From                       Message
  ----    ------                   ----               ----                       -------
  Normal  Starting                 31m                kube-proxy, 192.168.0.163  Starting kube-proxy.
  Normal  Starting                 29m                kubelet, 192.168.0.163     Starting kubelet.
  Normal  NodeHasSufficientMemory  29m (x2 over 29m)  kubelet, 192.168.0.163     Node 192.168.0.163 status is now: NodeHasSufficientMemory
  Normal  NodeHasNoDiskPressure    29m (x2 over 29m)  kubelet, 192.168.0.163     Node 192.168.0.163 status is now: NodeHasNoDiskPressure
  Normal  NodeHasSufficientPID     29m (x2 over 29m)  kubelet, 192.168.0.163     Node 192.168.0.163 status is now: NodeHasSufficientPID
  Normal  NodeAllocatableEnforced  29m                kubelet, 192.168.0.163     Updated Node Allocatable limit across pods
```

步骤 4
通过以下命令查看集群中带有 node=test 标签的节点信息。

```
$ kubectl get node -a -l "node=test"
Flag --show-all has been deprecated, will be removed in an upcoming release
NAME            STATUS    ROLES     AGE       VERSION
192.168.0.163   Ready     <none>    30m       v1.13.10-r1-CCE2.0.28.B001
```

步骤 5
通过 winscp 上传 POD-NodeSelector.yml 文件至客户端服务器/Home 目录。

步骤 6
返回 putty 客户端服务器确认并查看文件。

```
$ ls
$ cat POD-NodeSelector.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
```

步骤 7
运行以下命令创建 POD。

```
$ kubectl apply -f POD-NodeSelector.yml
# (当直接复制命令到 putty 出错时,可进行手敲进去尝试)
pod/nginx created
```

步骤 8
查看并确认 POD 运行节点是否为指定的节点。

```
$ kubectl get pod -o wide
NAME      READY     STATUS    RESTARTS   AGE       IP            NODE            NOMINATED NODE   READINESS GATES
nginx     1/1       Running   0          94s       172.16.0.52   192.168.0.163   <none>           <none>
```

#### 2.1.3 创建 POD2(1POD:2 容器)

步骤 1
通过 winscp 上传 POD-2Container.yml 容器定义文件。

步骤 2
通过 putty 确认文件上传。

```
$ ls
$ cat POD-2Container.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    node: test[root@ecs-sn3-medium-2-linux-20200204152547 home]# kubectl apply -f POD-NodeSelector.yml
pod/nginx created
[root@ecs-sn3-medium-2-linux-20200204152547 home]# cat POD-2Container.yml
apiVersion: v1
kind: Pod
metadata:
  name: two-containers
  labels:
    app: two-containers
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
```

步骤 3
通过以下命令创建 POD。

```
$ kubectl apply -f POD-2Container.yml
pod/two-containers created
```

步骤 4
通过以下命令查看 POD 运行状态。

```
$ kubectl get pods
NAME             READY     STATUS              RESTARTS   AGE
nginx            1/1       Running             0          2m15s
two-containers   0/2       ContainerCreating   0          14s
```

步骤 5
通过以下命令查看 POD 运行在哪个 Node 上。

```
$ kubectl get pod -o wide
NAME             READY     STATUS    RESTARTS   AGE       IP            NODE            NOMINATED NODE   READINESS GATES
nginx            1/1       Running   0          2m41s     172.16.0.52   192.168.0.163   <none>           <none>
two-containers   2/2       Running   0          40s       172.16.0.67   192.168.0.71    <none>           <none>
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
$ docker container ls | grep two-container
7574303afcb9        busybox                                                    "sh -c 'echo Hello K…"   About a minute ago   Up About a minute                       k8s_myapp-container_two-containers_default_fe53f1f8-473c-11ea-8ef9-fa163e2cbf0e_0
4db56c844e3d        nginx                                                      "nginx -g 'daemon of…"   2 minutes ago        Up 2 minutes                            k8s_nginx_two-containers_default_fe53f1f8-473c-11ea-8ef9-fa163e2cbf0e_0
86c48620d6ef        cce-pause:2.0                                              "/pause"                 2 minutes ago        Up 2 minutes                            k8s_POD_two-containers_default_fe53f1f8-473c-11ea-8ef9-fa163e2cbf0e_0
```

步骤 8
退出 node 节点。

```
exit
```

步骤 9
查看 kubernetes 集群 POD 信息并删除。

```
$ kubectl get pod
NAME             READY     STATUS    RESTARTS   AGE
nginx            1/1       Running   0          5m21s
two-containers   2/2       Running   0          3m20s
$ kubectl delete pod nginx
pod "nginx" deleted
$ kubectl delete pod two-containers
pod "two-containers" deleted
$ kubectl get pod
No resources found.
```

### 2.2 Deployment 实践

步骤 1
将 Deployment 定义文件 deployment.yml 上传至 ecs-k8s。

步骤 2
在 putty 中查看文件是否正确上传。

```
$ ls
$ cat deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.12.2
        ports:
        - containerPort: 80
```

步骤 3
通过以下命令创建 depolyment。

```
$ kubectl apply -f deployment.yml
deployment.apps/nginx-deployment created
```

步骤 4
通过以下命令查看 deployment 状态。

```
$ kubectl get deployment
NAME               READY     UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1       1            1           17s
```

步骤 5
通过以下命令查看 POD。

```
$ kubectl get pod
NAME                                READY     STATUS    RESTARTS   AGE
nginx-deployment-67d4b848b4-spt86   1/1       Running   0          27s
```

步骤 6
通过以下命令手动删除上述查看到的 POD。

```
$ kubectl delete pod nginx-deployment-67d4b848b4-spt86
pod "nginx-deployment-67d4b848b4-spt86" deleted
```

步骤 7
再次查看 POD。

```
$ kubectl get pod
NAME                                READY     STATUS    RESTARTS   AGE
nginx-deployment-67d4b848b4-lcwm9   1/1       Running   0          19s
```

步骤 8
通过以下命令扩展 Deployment 数量至 4。

```
$ kubectl scale deployment.v1.apps/nginx-deployment --replicas=4
deployment.apps/nginx-deployment scaled
$ kubectl get pod
NAME                                READY     STATUS    RESTARTS   AGE
nginx-deployment-67d4b848b4-lcwm9   1/1       Running   0          59s
nginx-deployment-67d4b848b4-nc8gt   1/1       Running   0          9s
nginx-deployment-67d4b848b4-sc54x   1/1       Running   0          9s
nginx-deployment-67d4b848b4-xtptn   1/1       Running   0          9s
```

步骤 9
再次查看 deployment 状态和数量。

```
$ kubectl get deployment
NAME               READY     UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   4/4       4            4           2m37s
```

步骤 10
通过以下命令删除 deployment 并查看。

```
$ kubectl delete deployment nginx-deployment
deployment.extensions "nginx-deployment" deleted
$ kubectl get deployment
No resources found.
```

步骤 11
再次查看 POD。

```
$ kubectl get pod
No resources found.
```

### 2.3 StatefulSet 实践

步骤 1
在 Winscp 将 StatefulSet 定义文件 statefulset.yml 上传至 ecs-k8s。

步骤 2
在 putty 中查看文件是否正确上传。

```
$ ls
$ cat statefulset.yml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
          name: web
```

步骤 3
通过以下命令创建 StatefulSet。

```
$ kubectl apply -f statefulset.yml
statefulset.apps/web created
```

步骤 4
通过以下命令查看 StatefulSet 的运行状态。

```
$ kubectl get statefulset
NAME      READY     AGE
web       1/2       18s
(建议自己手动输入)
```

步骤 5
通过以下命令查看 POD 数量和名称。

```
$ kubectl get pod
NAME      READY     STATUS    RESTARTS   AGE
web-0     1/1       Running   0          45s
web-1     1/1       Running   0          34s
```

步骤 6
通过以下命令查看 web-0 的 POD 的名称、IP 地址和所在的节点信息。

```
$ kubectl describe pod web-0
Name:               web-0
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               192.168.0.71/192.168.0.71
Start Time:         Tue, 04 Feb 2020 19:08:03 +0800
Labels:             app=nginx
                    controller-revision-hash=web-b486b4959
                    statefulset.kubernetes.io/pod-name=web-0
Annotations:        <none>
Status:             Running
IP:                 172.16.0.70
Controlled By:      StatefulSet/web
Containers:
  nginx:
    Container ID:   docker://1204a44cc5058d7176ce3948381c210fdd523bccb3a8cee6248e09a72b8a6862
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:ad5552c786f128e389a0263104ae39f3d3c7895579d45ae716f528185b36bc6f
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 04 Feb 2020 19:08:13 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-f9p4f (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-f9p4f:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-f9p4f
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason                 Age              From                   Message
  ----    ------                 ----             ----                   -------
  Normal  Scheduled              1m               default-scheduler      Successfully assigned default/web-0 to 192.168.0.71
  Normal  Pulling                1m               kubelet, 192.168.0.71  pulling image "nginx"
  Normal  SuccessfulMountVolume  1m (x2 over 1m)  kubelet, 192.168.0.71  Successfully mounted volumes for pod "web-0_default(9d0c1bec-473e-11ea-8ea7-fa163e72dc33)"
  Normal  Pulled                 1m               kubelet, 192.168.0.71  Successfully pulled image "nginx"
  Normal  SuccessfulCreate       1m               kubelet, 192.168.0.71  Created container
  Normal  Started                1m               kubelet, 192.168.0.71  Started container
```

步骤 7
手动删除名称为 web-0 的 POD。

```
$ kubectl delete pod web-0
pod "web-0" deleted
```

步骤 8
再次查看 POD。

```
$ kubectl get pod
NAME      READY     STATUS              RESTARTS   AGE
web-0     0/1       ContainerCreating   0          11s
web-1     1/1       Running             0          100s
```

步骤 9
再次查看 web-0 的 POD 的名称、IP 地址和所在的节点信息是够发生变化。

```
$ kubectl describe pod web-0
Name:               web-0
Namespace:          default
Priority:           0
PriorityClassName:  <none>
Node:               192.168.0.71/192.168.0.71
Start Time:         Tue, 04 Feb 2020 19:09:43 +0800
Labels:             app=nginx
                    controller-revision-hash=web-b486b4959
                    statefulset.kubernetes.io/pod-name=web-0
Annotations:        <none>
Status:             Running
IP:                 172.16.0.71
Controlled By:      StatefulSet/web
Containers:
  nginx:
    Container ID:   docker://f5e8d66c843d27c66347794a750b5f217ba044e078ddd1c29129af96a029f3f9
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:ad5552c786f128e389a0263104ae39f3d3c7895579d45ae716f528185b36bc6f
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Tue, 04 Feb 2020 19:09:55 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-f9p4f (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-f9p4f:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-f9p4f
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason                 Age                From                   Message
  ----    ------                 ----               ----                   -------
  Normal  Scheduled              48s                default-scheduler      Successfully assigned default/web-0 to 192.168.0.71
  Normal  Pulling                47s                kubelet, 192.168.0.71  pulling image "nginx"
  Normal  Pulled                 38s                kubelet, 192.168.0.71  Successfully pulled image "nginx"
  Normal  SuccessfulMountVolume  37s (x2 over 48s)  kubelet, 192.168.0.71  Successfully mounted volumes for pod "web-0_default(d8d89b81-473e-11ea-8ea7-fa163e72dc33)"
  Normal  SuccessfulCreate       37s                kubelet, 192.168.0.71  Created container
  Normal  Started                37s                kubelet, 192.168.0.71  Started container
```

重复删除 web-0 发现 StatefulSet 的 POD IP、名称和所在节点的规律。

> 稳定、标识唯一

### 2.4 DaemonSet 实践

步骤 1 在 winscp 中上传 daemonset.yml 文件至 ecs-k8s。

```
$ cat daemonset.yml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: gcr.io/fluentd-elasticsearch/fluentd:v2.5.1
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

步骤 2 通过以下命令查看 kube-system 命令空间中的 DaemonSet。

```
$ kubectl get ds -n kube-system
NAME             DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
icagent          2         2         2         2            2           <none>          43m
storage-driver   2         2         2         2            2           <none>          44m
```

步骤 3 通过以下命令创建 daemonset。

```
$ kubectl apply -f daemonset.yml
daemonset.apps/fluentd-elasticsearch created
```

步骤 4 再次查看 kube-system 中的 DaemonSet。

```

$ kubectl get ds -n kube-system
NAME                    DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
fluentd-elasticsearch   2         2         0         2            0           <none>          20s
icagent                 2         2         2         2            2           <none>          44m
storage-driver          2         2         2         2            2           <none>          45m
```

步骤 5
返回华为云 CCE 控制台,选择资源管理>节点管理,单击购买节点。
购买节点信息如下,确认信息后,单击提交按钮。

- 计费模式:按需计费;区域:北京四;操作系统:CentOS7.6
- 密码登录;节点购买数:1 台
- 其他参数默认(可参考下图)

步骤 6 等待节点运行状态为可用。

步骤 7 返回 putty,通过命令行查看各个 DaemonSet 实例数是不是 3。

```
$ kubectl get ds -n kube-system
NAME                    DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
fluentd-elasticsearch   3         3         0         3            0           <none>          4m52s
icagent                 3         3         3         3            3           <none>          49m
storage-driver          3         3         2         3            2           <none>          49m
```

### 2.5 Job 实践

步骤 1 通过 winscp 将 Job.yml 上传至 ecs-k8s。

```
$ cat Job.yml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```

步骤 2 通过以下命令运行 Job。

```
$ kubectl apply -f Job.yml
job.batch/pi created
```

步骤 3
通过以下命令查看 job 运行状态。

```
$ kubectl get job
NAME      COMPLETIONS   DURATION   AGE
pi        0/1           14s        14s
```

步骤 4
通过以下命令查看是否有 pi 开头的 POD。

```
$ kubectl get pod
NAME       READY     STATUS              RESTARTS   AGE
pi-jfshg   0/1       ContainerCreating   0          31s
web-0      1/1       Running             0          9m27s
web-1      1/1       Running             0          12m
```

步骤 5
等待 10S 左右,再次查看 Job 的运行状态。

```
$ kubectl get pod
NAME       READY     STATUS    RESTARTS   AGE
pi-jfshg   1/1       Running   0          56s
web-0      1/1       Running   0          9m52s
web-1      1/1       Running   0          12m
```

步骤 6
通过以下命令查看此 Job 的输出。

```
$ kubectl get pod
NAME       READY     STATUS      RESTARTS   AGE
pi-jfshg   0/1       Completed   0          68s
web-0      1/1       Running     0          10m
web-1      1/1       Running     0          12m
$ kubectl logs pi-jfshg
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170679821480865132823066470938446095505822317253594081284811174502841027019385211055596446229489549303819644288109756659334461284756482337867831652712019091456485669234603486104543266482133936072602491412737245870066063155881748815209209628292540917153643678925903600113305305488204665213841469519415116094330572703657595919530921861173819326117931051185480744623799627495673518857527248912279381830119491298336733624406566430860213949463952247371907021798609437027705392171762931767523846748184676694051320005681271452635608277857713427577896091736371787214684409012249534301465495853710507922796892589235420199561121290219608640344181598136297747713099605187072113499999983729780499510597317328160963185950244594553469083026425223082533446850352619311881710100031378387528865875332083814206171776691473035982534904287554687311595628638823537875937519577818577805321712268066130019278766111959092164201989380952572010654858632788659361533818279682303019520353018529689957736225994138912497217752834791315155748572424541506959508295331168617278558890750983817546374649393192550604009277016711390098488240128583616035637076601047101819429555961989467678374494482553797747268471040475346462080466842590694912933136770289891521047521620569660240580381501935112533824300355876402474964732639141992726042699227967823547816360093417216412199245863150302861829745557067498385054945885869269956909272107975093029553211653449872027559602364806654991198818347977535663698074265425278625518184175746728909777727938000816470600161452491921732172147723501414419735685481613611573525521334757418494684385233239073941433345477624168625189835694855620992192221842725502542568876717904946016534668049886272327917860857843838279679766814541009538837863609506800642251252051173929848960841284886269456042419652850222106611863067442786220391949450471237137869609563643719172874677646575739624138908658326459958133904780275901
```

### 2.6 Service 实践

步骤 1
通过之前上传的 deployment 文件创建 Deployment。

```
$ kubectl apply -f deployment.yml
deployment.apps/nginx-deployment created
```

步骤 2
通过命令行查看 Deployment 状态和 POD 状态。

```
$ kubectl get deployment
NAME               READY     UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   0/1       1            0           7s
$ kubectl get pod
NAME                                READY     STATUS              RESTARTS   AGE
nginx-deployment-67d4b848b4-wg6dl   0/1       ContainerCreating   0          9s
pi-jfshg                            0/1       Completed           0          2m
web-0                               1/1       Running             0          10m
web-1                               1/1       Running             0          13m
```

步骤 3
用户通过命令行创建 NodePort 类型的 Service 并查看。

```
$ kubectl expose deployment nginx-deployment --type=NodePort
service/nginx-deployment exposed
$ kubectl get service
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes         ClusterIP   10.247.0.1      <none>        443/TCP        65m
nginx-deployment   NodePort    10.247.164.34   <none>        80:32341/TCP   19s
```

步骤 4
通过 curl 命令验证网站是否可以访问。(任意一个 Kubernetes NODE 节点地址都行。

```
$ kubectl get node
NAME            STATUS    ROLES     AGE       VERSION
192.168.0.144   Ready     <none>    9m42s     v1.13.10-r1-CCE2.0.28.B001
192.168.0.163   Ready     <none>    64m       v1.13.10-r1-CCE2.0.28.B001
192.168.0.71    Ready     <none>    64m       v1.13.10-r1-CCE2.0.28.B001
$ curl 192.168.0.144:32341
# (注意端口号为上一步查看到的 NodePort)
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

步骤 5 通过华为云控制台查看其中一个 kubernetes Node 绑定的公网地址。

步骤 6 打开浏览器,输入 ECS 绑定的公网地址+Service 的 Nodeport。如本示例中为: 119.3.172.198:32341

### 2.7 NameSpace 实践

####  2.7.1 默认的 NameSpace 实践

步骤 1
通过以下命令查看系统默认的 NameSpace。

```
$ kubectl get namespace
NAME          STATUS    AGE
default       Active    69m
kube-public   Active    69m
kube-system   Active    69m
```

步骤 2
通过以下命令可以手动的创建一个 NameSpace 命名空间并查看。

```
$ kubectl create namespace test
namespace/test created
$ kubectl get namespace
NAME          STATUS    AGE
default       Active    69m
kube-public   Active    69m
kube-system   Active    69m
test          Active    1s
```

步骤 3
创建一个 POD 并指定此 POD 运行在 test 命名空间。

```
$ kubectl apply -f POD-1Container.yml --namespace=test
pod/nginx created
```

步骤 4
查看指定命名空间里的 POD。

```
$ kubectl get pod -n test
NAME      READY     STATUS    RESTARTS   AGE
nginx     1/1       Running   0          21s
```

#### 2.7.2 创建一个限制 CPU 和内存大小的 NameSpace。

步骤 1
通过以下命令创建一个 Namespace。

```
$ kubectl create namespace quota-mem-cpu-example
namespace/quota-mem-cpu-example created
$ kubectl get ns
NAME                    STATUS    AGE
default                 Active    72m
kube-public             Active    72m
kube-system             Active    72m
quota-mem-cpu-example   Active    5s
test                    Active    3m7s
```

步骤 2 在 winscpz 中上传 NameSpace 定义文件到 ecs-k8s 中。

步骤 3 通过以下命令查看文件信息。

```
$ cat ns-cpu-mem.yml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu-demo
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
```

步骤 4
创建 ResourceQuota 并和 NameSpace 进行关联。

```
$ ubectl create -f ns-cpu-mem.yml --namespace=quota-mem-cpu-example
resourcequota/mem-cpu-demo created
```

步骤 5
查看 ResourceQuota 详细信息。

```
$ kubectl get resourcequota mem-cpu-demo --namespace=quota-mem-cpu-example --output=yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  creationTimestamp: 2020-02-04T11:36:33Z
  name: mem-cpu-demo
  namespace: quota-mem-cpu-example
  resourceVersion: "14233"
  selfLink: /api/v1/namespaces/quota-mem-cpu-example/resourcequotas/mem-cpu-demo
  uid: 98668559-4742-11ea-8ef9-fa163e2cbf0e
spec:
  hard:
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.cpu: "1"
    requests.memory: 1Gi
status:
  hard:
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.cpu: "1"
    requests.memory: 1Gi
  used:
    limits.cpu: "0"
    limits.memory: "0"
    requests.cpu: "0"
    requests.memory: "0"
```

#### 2.7.3 添加限制
以上刚创建的 ResourceQuota 对象将在 quota-mem-cpu-example 名字空间中添加以下限
制:

每个容器必须设置内存请求(memory request),内存限额(memory limit),cpu 请求(cpu request)和 cpu 限额(cpu limit)。

- 所有容器的内存请求总额不得超过 1 GiB。
- 所有容器的内存限额总额不得超过 2 GiB。
- 所有容器的 CPU 请求总额不得超过 1 CPU。
- 所有容器的 CPU 限额总额不得超过 2 CPU

步骤 1
在 winscp 中将 POD 定义文件上传至 ecs-k8s。

步骤 2
查看 POD 定义文件。

```
$ cat quota-mem-cpu-pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: quota-mem-cpu-demo
spec:
  containers:
  - name: quota-mem-cpu-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "800Mi"
        cpu: "800m"
      requests:
        memory: "600Mi"
        cpu: "400m"
```

步骤 3
创建 POD。

```
$  kubectl create -f quota-mem-cpu-pod.yml --namespace=quota-mem-cpu-example
pod/quota-mem-cpu-demo created
```

步骤 4
运行以下命令确认 POD 已运行。

```
$ kubectl get pod quota-mem-cpu-demo --namespace=quota-mem-cpu-example
NAME                 READY     STATUS    RESTARTS   AGE
quota-mem-cpu-demo   1/1       Running   0          26s
```

步骤 5
然后再次查看 ResourceQuota 对象的详细信息:

```
$ kubectl get resourcequota mem-cpu-demo --namespace=quota-mem-cpu-example --output=yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  creationTimestamp: 2020-02-04T11:36:33Z
  name: mem-cpu-demo
  namespace: quota-mem-cpu-example
  resourceVersion: "15004"
  selfLink: /api/v1/namespaces/quota-mem-cpu-example/resourcequotas/mem-cpu-demo
  uid: 98668559-4742-11ea-8ef9-fa163e2cbf0e
spec:
  hard:
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.cpu: "1"
    requests.memory: 1Gi
status:
  hard:
    limits.cpu: "2"
    limits.memory: 2Gi
    requests.cpu: "1"
    requests.memory: 1Gi
  used:
    limits.cpu: 800m
    limits.memory: 800Mi
    requests.cpu: 400m
    requests.memory: 600Mi
```

步骤 6 通过 winscp 上传第二个 POD 定义文件。

步骤 7 查看并确认。

```
$ cat quota-mem-cpu-pod-2.yml
apiVersion: v1
kind: Pod
metadata:
  name: quota-mem-cpu-demo-2
spec:
  containers:
  - name: quota-mem-cpu-demo-2-ctr
    image: redis
    resources:
      limits:
        memory: "1Gi"
        cpu: "800m"      
      requests:
        memory: "700Mi"
        cpu: "400m"
```

步骤 8
通过以下命令创建第二个 POD。

```
$ kubectl apply -f quota-mem-cpu-pod-2.yml --namespace=quota-mem-cpu-example
kubectl apply -f quota-mem-cpu-pod-2.yml --namespace=quota-mem-cpu-example
Error from server (Forbidden): error when creating "quota-mem-cpu-pod-2.yml": pods "quota-mem-cpu-demo-2" is forbidden: exceeded quota: mem-cpu-demo, requested: requests.memory=700Mi, used: requests.memory=600Mi, limited: requests.memory=1Gi
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
镜像名称:swr.cn-north-1.myhuaweicloud.com/hc_cce/flappybird:latest

步骤 4 容器设置选择默认,点击下一步。

步骤 5 添加服务,如下参数配置后点击确定。

- 访问类型:负载均衡,服务名称:flappybird;集群级别
- 端口配置,容器端口:80;访问端口:80
- 其他参数默认(可参照下图参数配置)

步骤 6
高级设置选择默认,点击创建。

步骤 7 返回工作负载,flappybird 的状态是运行中时,进行验证。

步骤 8 点击外部访问地址的链接。如果出现此界面,则游戏部署成功。

![2020-02-04-19-48-27-efc9d61563850bfa.png](https://imagehost-cdn.frytea.com/images/2020/02/04/2020-02-04-19-48-27-efc9d61563850bfa.png)

## 4 资源释放

步骤 1
进入云容器引擎 CCE 控制台。单击集群管理。选中实验中创建的集群,更多>删除集群
勾选集群所有选项,然后输入 DELETE,单击确定。课程名称+实验指导手册

步骤 2
进入云服务器控制台。勾选 ecs-k8s,单击更多>删除。
勾选所有选项,单击是。
单击资源>我的资源。

确认所有计费资源都已删除。
