---
title: kubernetes
tags: [Java, Architecture, Kubernetes]
date: 2019-08-14 09:23:34
---

## 基本概念
### 集群
一个集群指容器运行所需要的云资源组合，关联了若干服务器节点、负载均衡、专有网络等云资源。
#### 节点
一台服务器（可以是虚拟机实例或者物理服务器）已经安装了 Docker Engine，可以用于部署和管理容器；容器服务的 Agent 程序会安装到节点上并注册到一个集群上。集群中的节点数量可以伸缩。
#### 容器
一个通过 Docker 镜像创建的运行时实例，一个节点可运行多个容器。
#### 镜像
Docker 镜像是容器应用打包的标准格式，在部署容器化应用时可以指定镜像，镜像可以来自于 Docker Hub，阿里云容器 Hub，或者用户的私有 Registry。镜像 ID 可以由镜像所在仓库 URI 和镜像 Tag（缺省为 latest）唯一确认。
#### 编排模板
编排模板包含了一组容器服务的定义和其相互关联，可以用于多容器应用的部署和管理。容器服务支持 Docker Compose 模板规范并有所扩展。
#### 应用
一个应用可通过单个镜像或一个编排模板创建，每个应用可包含1个或多个服务。
#### 服务
一组基于相同镜像和配置定义的容器，作为一个可伸缩的微服务。
#### 关联关系
![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/6859/15445132241060_zh-CN.png)

### Kubernetes 相关概念
Kubernetes是一个 Google 开源的，用于自动部署、扩展和管理容器化应用的大规模容器编排调度系统。具有可移植、可扩展和自动调度等特性。
#### 节点（Node）
Kubernetes 集群中的计算能力由 Node 提供，Kubernetes 集群中的 Node 是所有 Pod 运行所在的工作主机，可以是物理机也可以是虚拟机。工作主机的统一特征是上面要运行 kubelet 管理节点上运行的容器。
命名空间（Namespace）
命名空间为 Kubernetes 集群提供虚拟的隔离作用。Kubernetes 集群初始有 3 个命名空间，分别是默认命名空间 default、系统命名空间 kube-system 和 kube-public ，除此以外，管理员可以可以创建新的名字空间满足需要。
#### Pod
Pod是 Kubernetes 部署应用或服务的最小的基本单位。一个Pod 封装多个应用容器（也可以只有一个容器）、存储资源、一个独立的网络 IP 以及管理控制容器运行方式的策略选项。
副本控制器（Replication Controller，RC）
RC 确保任何时候 Kubernetes 集群中有指定数量的 pod 副本(replicas)在运行。通过监控运行中的 Pod 来保证集群中运行指定数目的 Pod 副本。指定的数目可以是多个也可以是1个；少于指定数目，RC 就会启动运行新的 Pod 副本；多于指定数目，RC 就会终止多余的 Pod 副本。
#### 副本集（Replica Set，RS）
ReplicaSet（RS）是 RC 的升级版本，唯一区别是对选择器的支持，RS 能支持更多种类的匹配模式。副本集对象一般不单独使用，而是作为 Deployment 的理想状态参数使用。
部署（Deployment）
部署表示用户对 Kubernetes 集群的一次更新操作。部署比 RS 应用更广，可以是创建一个新的服务，更新一个新的服务，也可以是滚动升级一个服务。滚动升级一个服务，实际是创建一个新的 RS，然后逐渐将新 RS 中副本数增加到理想状态，将旧 RS 中的副本数减小到 0 的复合操作；这样一个复合操作用一个RS是不太好描述的，所以用一个更通用的 Deployment 来描述。不建议您手动管理利用 Deployment 创建的 RS。
#### 服务（Service）
Service 也是 Kubernetes 的基本操作单元，是真实应用服务的抽象，每一个服务后面都有很多对应的容器来提供支持，通过 Kube-Proxy 的 port 和服务 selector 决定服务请求传递给后端的容器，对外表现为一个单一访问接口，外部不需要了解后端如何运行，这给扩展或维护后端带来很大的好处。
#### 标签（labels）
Labels 的实质是附着在资源对象上的一系列 Key/Value 键值对，用于指定对用户有意义的对象的属性，标签对内核系统是没有直接意义的。标签可以在创建一个对象的时候直接赋予，也可以在后期随时修改，每一个对象可以拥有多个标签，但 key 值必须唯一。
#### 存储卷（Volume）
Kubernetes 集群中的存储卷跟 Docker 的存储卷有些类似，只不过 Docker 的存储卷作用范围为一个容器，而 Kubernetes 的存储卷的生命周期和作用范围是一个 Pod。每个 Pod 中声明的存储卷由 Pod 中的所有容器共享。支持使用 Persistent Volume Claim 即 PVC 这种逻辑存储，使用者可以忽略后台的实际存储技术，具体关于 Persistent Volumn(pv)的配置由存储管理员来配置。
持久存储卷（Persistent Volume，PV）和持久存储卷声明（Persistent Volume Claim，PVC）
PV 和 PVC 使得 Kubernetes 集群具备了存储的逻辑抽象能力，使得在配置 Pod 的逻辑里可以忽略对实际后台存储技术的配置，而把这项配置的工作交给 PV 的配置者。存储的 PV 和 PVC 的这种关系，跟计算的 Node 和 Pod 的关系是非常类似的；PV 和 Node 是资源的提供者，根据集群的基础设施变化而变化，由 Kubernetes 集群管理员配置；而 PVC 和 Pod是资源的使用者，根据业务服务的需求变化而变化，由 Kubernetes 集群的使用者即服务的管理员来配置。
#### Ingress
Ingress 是授权入站连接到达集群服务的规则集合。你可以通过 Ingress 配置提供外部可访问的 URL、负载均衡、SSL、基于名称的虚拟主机等。用户通过 POST Ingress 资源到 API server 的方式来请求 ingress。 Ingress controller 负责实现 Ingress，通常使用负载均衡器，它还可以配置边界路由和其他前端，这有助于以 HA 方式处理流量。
参考：https://www.alibabacloud.com/help/zh/doc-detail/25975.htm?spm=a2c63.p38356.b99.9.10c927a1mjblZa


## Pod

## Service
Service 的虚拟 IP 是有 Kubernetes 虚拟出来的内部网络，外部网络是无法访问的，但是有一些 Service 有需要对外暴露，比如 Web 前段。这时候就需要增加一层路由转发，即外网到内网的转发，Kubernetes 提供了 NodePort Service方式发布Service
外部访问 - NodePort:
```
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-svc
  labels:
    app: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30010
  selector:
    app: nginx
```

## Deployment

## Ingress
Nginx
Traefik
Kubernetes Ingress 示例:
路径转换rewrite：
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: wms
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /wms
        backend:
          serviceName: wms
          servicePort: 8080
```
host域名映射：
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: config
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: config
    http:
      paths:
      - path: /config
        backend:
          serviceName: config
          servicePort: 8888
```

## kubectl
### 安装
官网提供的安装命令是:
> curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
Github安装方式：
- 1. 到这个页面选择当前的版本，点击进去
https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md#client-binaries-1
- 2. 找到client binaries（也就是kubectl），选择对应操作系统的客户端，然后复制地址
- 3. 下载kubectl包，解压后，将kubectl命令赋予权限和拷贝到用户命令目录下
```sh
wget https://dl.k8s.io/v1.9.3/kubernetes-client-linux-amd64.tar.gz
tar -zxvf kubernetes-client-linux-amd64.tar.gz
cd kubernetes/client/bin
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
- 4. 运行 kubectl version，返回版本信息，说明安装成功

### 常用命令
```
kubectl get namespaces
kubectl get nodes


kubectl get pods -n kube-system
kubectl get rc -n kube-system
kubectl get rs -n kube-system
kubectl get deployment -n kube-system
kubectl get services -n kube-system

kubectl get secrets -n kube-system
kubectl get roles.rbac.authorization.k8s.io -n kube-system
kubectl get serviceaccounts -n kube-system
kubectl get rolebindings.rbac.authorization.k8s.io -n kube-system

kubectl get namespace
kubectl get svc,deploy,pod,ingress -n ingress-nginx
kubectl get cm -n ingress-nginx
kubectl get svc -n ingress-nginx
```

## 发布

![基于Jenkins的持续集成与发布](https://jimmysong.io/kubernetes-handbook/images/kubernetes-jenkins-ci-cd.png)

应用构建和发布流程说明。

1. 用户向Gitlab提交代码，代码中必须包含Dockerfile
2. 将代码提交到远程仓库
3. 用户在发布应用时需要填写git仓库地址和分支、服务类型、服务名称、资源数量、实例个数，确定后触发Jenkins自动构建
4. Jenkins的CI流水线自动编译代码并打包成docker镜像推送到Harbor镜像仓库
5. Jenkins的CI流水线中包括了自定义脚本，根据我们已准备好的kubernetes的YAML模板，将其中的变量替换成用户输入的选项
6. 生成应用的kubernetes YAML配置文件
7. 更新Ingress的配置，根据新部署的应用的名称，在ingress的配置文件中增加一条路由信息
8. 更新PowerDNS，向其中插入一条DNS记录，IP地址是边缘节点的IP地址。
8. Jenkins调用kubernetes的API，部署应用


### 滚动更新
  如果通过滚动更新来更新部署服务，Kubernetes 会慢慢终止旧的 pod，同时加速生成新的 pod。为了最大程度减小对最终用户的影响，并尽可能缩短恢复时间，应用程序能够正常终止十分重要。
  Kubernetes 决定终止 pod，将发生一系列事件。Kubernetes 终止生命周期的各个步骤如下：
1. 将 pod 设置为“正在终止”状态，并将其从所有服务的端点列表中移除
此时，pod 停止获取新流量，Pod 中运行的容器不受影响。
2. 执行 preStop 钩子
preStop 钩子是向 pod 中的容器发送的特殊命令或 http 请求。
如果您的应用程序在收到 SIGTERM 后未正常关闭，可使用此钩子触发正常关闭。大多数程序在收到 SIGTERM 后都会正常关闭，但如果您使用的是第三方代码或管理的系统不受您控制，preStop 钩子将是一个不错的方案，可帮您在不修改应用程序的情况下触发正常关闭。
3. 向 pod 发送 SIGTERM 信号
此时，Kubernetes 将向 pod 中的容器发送 SIGTERM 信号。此信号通知容器它们即将被关闭。
您的代码应侦听此事件，并在此时开始“干净地”关闭。这可能包括停止所有长时间连接（如数据库连接或 WebSocket 流）、保存当前状态或类似任务。
即使您现在已经在使用 preStop 钩子，也有必要测试一下应用程序在您向它发送 SIGTERM 信号后的反应，以免在实际使用时对实际情况感到惊讶！
4. Kubernetes 等待片刻（宽限期）
此时，Kubernetes 将等待片刻，此时间称为终止宽限期，具体值可指定。默认值为 30 秒。需要注意的是，这与 preStop 钩子和 SIGTERM 信号并行发生。Kubernetes 不会等待 preStop 钩子完成。
如果您的应用在 terminationGracePeriod 完成之前完成关闭并退出，Kubernetes 将立即转到下一步。
如果您的 pod 通常需要 30 秒以上的时间才能关闭，请务必延长宽限期。您可以通过在 Pod YAML 中设置 terminationGracePeriodSeconds 选项来实现此目的。例如，可将该值改为 60 秒：
5. 向 pod 发送 SIGKILL 信号，pod 随即被移除
如果宽限期结束后容器仍在运行，将向容器发送 SIGKILL 信号并强制将它们移除。此时，所有 Kubernetes 对象将被一同清理。

结论
Kubernetes 可以出于各种原因终止 pod，确保您的应用程序正常执行这些终止操作是创建稳定系统和提供良好用户体验的核心。


蓝绿部署
在进行蓝/绿部署时，应用程序的一个新副本（绿）将与现有版本（蓝）一起部署。然后更新应用程序的入口/路由器以切换到新版本（绿）。然后，您需要等待旧（蓝）版本来完成所有发送给它的请求，但是大多数情况下，应用程序的流量将一次更改为新版本。

无缝发布 滚动发布 金丝雀发布 灰度发布
Canary

金丝雀发布:
- 准备和生产环境隔离的“金丝雀”服务器。
- 将新版本的服务部署到“金丝雀”服务器上。
- 对“金丝雀”服务器上的服务进行自动化和人工测试。
- 测试通过后，将“金丝雀”服务器连接到生产环境，将少量生产流量导入到“金丝雀”服务器中。
- 如果在线测试出现问题，则通过把生产流量从“金丝雀”服务器中重新路由到老版本的服务的方式进行回退，修复问题后重新进行发布。
- 如果在线测试顺利，则逐渐把生产流量按一定策略逐渐导入到新版本服务器中。
- 待新版本服务稳定运行后，删除老版本服务。

参考：https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489100&amp;idx=1&amp;sn=eab291eb345c074114d946b732e037eb&source=41#wechat_redirect

## Liveness和Readiness探针
kubelet 使用 liveness probe（存活探针）来确定何时重启容器。例如，当应用程序处于运行状态但无法做进一步操作，liveness 探针将捕获到 deadlock，重启处于该状态下的容器，使应用程序在存在 bug 的情况下依然能够继续运行下去。
Kubelet 使用 readiness probe（就绪探针）来确定容器是否已经就绪可以接受流量。只有当 Pod 中的容器都处于就绪状态时 kubelet 才会认定该 Pod处于就绪状态。该信号的作用是控制哪些 Pod应该作为service的后端。如果 Pod 处于非就绪状态，那么它们将会被从 service 的 load balancer中移除。

### 配置 Probe
Probe 中有很多精确和详细的配置，通过它们您能准确的控制 liveness 和 readiness 检查：
- initialDelaySeconds：容器启动后第一次执行探测是需要等待多少秒。
- periodSeconds：执行探测的频率。默认是10秒，最小1秒。
- timeoutSeconds：探测超时时间。默认1秒，最小1秒。
- successThreshold：探测失败后，最少连续探测成功多少次才被认定为成功。默认是 1。对于 liveness 必须是 1。最小值是 1。
- failureThreshold：探测成功后，最少连续探测失败多少次才被认定为失败。默认是 3。最小值是 1。
### HTTP probe 中可以给 httpGet设置其他配置项：
- host：连接的主机名，默认连接到 pod 的 IP。您可能想在 http header 中设置 “Host” 而不是使用 IP。
- scheme：连接使用的 schema，默认HTTP。
- path: 访问的HTTP server 的 path。
- httpHeaders：自定义请求的 header。HTTP运行重复的 header。
- port：访问的容器的端口名字或者端口号。端口号必须介于 1 和 65525 之间。
对于 HTTP 探测器，kubelet 向指定的路径和端口发送 HTTP 请求以执行检查。 Kubelet 将 probe 发送到容器的 IP 地址，除非地址被httpGet中的可选host字段覆盖。 在大多数情况下，您不想设置主机字段。 有一种情况下您可以设置它。 假设容器在127.0.0.1上侦听，并且 Pod 的hostNetwork字段为 true。 然后，在httpGet下的host应该设置为127.0.0.1。 如果您的 pod 依赖于虚拟主机，这可能是更常见的情况，您不应该是用host，而是应该在httpHeaders中设置Host头。

What’s next