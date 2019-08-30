####安装
前提准备
*   关闭交换空间：`sudo swapoff -a`
*   避免开机启动交换空间：注释 `/etc/fstab` 中的 `swap`
*   关闭防火墙：`ufw disable`
查看虚拟内存
```
# free
              total        used        free      shared  buff/cache   available
Mem:        2017296      178020     1636676        1180      202600     1685716
Swap:             0           0           0
```
一键安装docker脚本(推荐)
```
# curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
# Executing docker install script, commit: 2f4ae48
+ sh -c 'apt-get update -qq >/dev/null'
+ sh -c 'apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null'
+ sh -c 'curl -fsSL "https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null'
Warning: apt-key output should not be parsed (stdout is not a terminal)
+ sh -c 'echo "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu bionic stable" > /etc/apt/sources.list.d/docker.list'
+ sh -c 'apt-get update -qq >/dev/null'
+ '[' -n '' ']'
+ sh -c 'apt-get install -y -qq --no-install-recommends docker-ce >/dev/null'
+ sh -c 'docker version'
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        2d0083d
 Built:             Thu Jun 27 17:56:23 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.7
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       2d0083d
  Built:            Thu Jun 27 17:23:02 2019
  OS/Arch:          linux/amd64
  Experimental:     false
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker your-user

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
```


安装docker
```
$ sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get -y update
$ sudo apt-get -y install docker-ce
$ docker version
Client:
 Version:           18.09.7
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        2d0083d
 Built:             Thu Jun 27 17:56:23 2019
 OS/Arch:           linux/amd64
 Experimental:      false
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.39/version: dial unix /var/run/docker.sock: connect: permission denied
```
docker加速
sudo vim /etc/docker/daemon.json
```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
```
```
sudo systemctl restart docker
$ sudo docker info
```
修改设备名称
```
$ hostnamectl
$ sudo hostnamectl set-hostname kubernetes-master
```
安装kebernetes的镜像
```
$ su root
# apt-get update && apt-get install -y apt-transport-https
# curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -
# cat << EOF >/etc/apt/sources.list.d/kubernetes.list
> deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
> EOF
# apt-get update
```
安装kebernetes三大件
```
# apt-get install -y kubelet kubeadm kubectl
Setting up kubeadm (1.15.0-00) ...
# systemctl enable kubelet && systemctl start kubelet
```
修改配置
```
kubeadm config print init-defaults --kubeconfig ClusterConfiguration > kubeadm.yml
```
配置文件内容
```
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 192.168.237.129
  bindPort: 6443
nodeRegistration:
  criSocket: /var/run/dockershim.sock
  name: kubernetes-master
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta2
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: registry.aliyuncs.com/google_containers
kind: ClusterConfiguration
kubernetesVersion: v1.15.0
networking:
  dnsDomain: cluster.local
  serviceSubnet: 10.96.0.0/12
scheduler: {}            
```
拉取镜像
```
# kubeadm config images pull --config kubeadm.yml
[config/images] Pulled registry.aliyuncs.com/google_containers/kube-apiserver:v1.15.0
[config/images] Pulled registry.aliyuncs.com/google_containers/kube-controller-manager:v1.15.0
[config/images] Pulled registry.aliyuncs.com/google_containers/kube-scheduler:v1.15.0
[config/images] Pulled registry.aliyuncs.com/google_containers/kube-proxy:v1.15.0
[config/images] Pulled registry.aliyuncs.com/google_containers/pause:3.1
[config/images] Pulled registry.aliyuncs.com/google_containers/etcd:3.3.10
[config/images] Pulled registry.aliyuncs.com/google_containers/coredns:1.3.1

```
查看镜像
```
:/home/baxiang# docker images
REPOSITORY                                                        TAG                 IMAGE ID            CREATED             SIZE
registry.aliyuncs.com/google_containers/kube-proxy                v1.15.0             d235b23c3570        3 weeks ago         82.4MB
registry.aliyuncs.com/google_containers/kube-apiserver            v1.15.0             201c7a840312        3 weeks ago         207MB
registry.aliyuncs.com/google_containers/kube-controller-manager   v1.15.0             8328bb49b652        3 weeks ago         159MB
registry.aliyuncs.com/google_containers/kube-scheduler            v1.15.0             2d3813851e87        3 weeks ago         81.1MB
registry.aliyuncs.com/google_containers/coredns                   1.3.1               eb516548c180        5 months ago        40.3MB
registry.aliyuncs.com/google_containers/etcd                      3.3.10              2c4adeb21b4f        7 months ago        258MB
registry.aliyuncs.com/google_containers/pause                     3.1                 da86e6ba6ca1        18 months ago       742kB

```

初始化主节点
```
kubeadm init --pod-network-cidr 10.244.0.0/16 --kubernetes-version stable
```
或者
```
kubeadm init --config=kubeadm.yml | tee kubeadm-init.log
```
```

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.237.129:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:6487845dbd51ddd8874dda2257ecf6157a0a6d7487317355ddc8a081c8525cc1
```
配置 kubectl
```
 mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

```
查看节点信息
```
# kubectl get node
NAME                STATUS     ROLES    AGE     VERSION
kubernetes-master   NotReady   master   9m17s   v1.15.0
```

查看节点所有namespaces
```
# kubectl get pods --all-namespaces
NAMESPACE     NAME                                        READY   STATUS    RESTARTS   AGE
kube-system   coredns-bccdc95cf-njhpw                     0/1     Pending   0          12m
kube-system   coredns-bccdc95cf-z4br9                     0/1     Pending   0          12m
kube-system   etcd-kubernetes-master                      1/1     Running   0          11m
kube-system   kube-apiserver-kubernetes-master            1/1     Running   0          12m
kube-system   kube-controller-manager-kubernetes-master   1/1     Running   0          12m
kube-system   kube-proxy-qw6bn                            1/1     Running   0          12m
kube-system   kube-scheduler-kubernetes-master            1/1     Running   0          12m
```
节点加入命令
```
kubeadm join 192.168.237.129:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:6487845dbd51ddd8874dda2257ecf6157a0a6d7487317355ddc8a081c8525cc1
```
####安装网络插件 Calico
[https://www.projectcalico.org/#getstarted](https://www.projectcalico.org/#getstarted)
[https://docs.projectcalico.org/v3.8/getting-started/kubernetes/](https://docs.projectcalico.org/v3.8/getting-started/kubernetes/)


在线实践平台
[https://labs.play-with-k8s.com/](https://labs.play-with-k8s.com/)
####集群设计
![image.png](https://upload-images.jianshu.io/upload_images/143845-df3cb11f6d6e1d0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Kubernetes 可以管理大规模的集群，使集群中的每一个节点彼此连接，能够像控制一台单一的计算机一样控制整个集群。
集群有两种角色，一种是 master ，一种是 Node（也叫worker）。
- master 是集群的"大脑"，负责管理整个集群：像应用的调度、更新、扩缩容等。
- Node 就是具体"干活"的，一个Node一般是一个虚拟机或物理机，它上面事先运行着 docker 服务和 kubelet 服务（ Kubernetes 的一个组件），当接收到 master 下发的"任务"后，Node 就要去完成任务（用 docker 运行一个指定的应用）
![image.png](https://upload-images.jianshu.io/upload_images/143845-e315396fcfa4533f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/143845-57fee5594a9732c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####Deployment - 应用管理者

当我们拥有一个 Kubernetes 集群后，就可以在上面跑我们的应用了，前提是我们的应用必须支持在 docker 中运行，也就是我们要事先准备好docker镜像。

有了镜像之后，一般我们会通过Kubernetes的 Deployment 的配置文件去描述应用，比如应用叫什么名字、使用的镜像名字、要运行几个实例、需要多少的内存资源、cpu 资源等等。

有了配置文件就可以通过Kubernetes提供的命令行客户端 - kubectl 去管理这个应用了。kubectl 会跟 Kubernetes 的 master 通过RestAPI通信，最终完成应用的管理。
比如我们刚才配置好的 Deployment 配置文件叫 app.yaml，我们就可以通过
"kubectl create -f app.yaml" 来创建这个应用啦，之后就由 Kubernetes 来保证我们的应用处于运行状态，当某个实例运行失败了或者运行着应用的 Node 突然宕机了，Kubernetes 会自动发现并在新的 Node 上调度一个新的实例，保证我们的应用始终达到我们预期的结果。



####Pod —— Kubernetes最小调度单位
其实在上一步创建完 Deployment 之后，Kubernetes 的 Node 做的事情并不是简单的docker run 一个容器。出于像易用性、灵活性、稳定性等的考虑，Kubernetes 提出了一个叫做 Pod 的东西，作为 Kubernetes 的最小调度单位。所以我们的应用在每个 Node 上运行的其实是一个 Pod。Pod 也只能运行在 Node 上。如下图：

![image.png](https://upload-images.jianshu.io/upload_images/143845-146dcb7336b0247e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么什么是 Pod 呢？Pod 是一组容器（当然也可以只有一个）。容器本身就是一个小盒子了，Pod 相当于在容器上又包了一层小盒子。这个盒子里面的容器有什么特点呢？

- 可以直接通过 volume 共享存储。
- 有相同的网络空间，通俗点说就是有一样的ip地址，有一样的网卡和网络设置。
- 多个容器之间可以“了解”对方，比如知道其他人的镜像，知道别人定义的端口等。

至于这样设计的好处呢，还是要大家深入学习后慢慢体会啦~

![image.png](https://upload-images.jianshu.io/upload_images/143845-1f76ae0ed5b701c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


####Service - 服务发现 - 找到每个Pod

上面的 Deployment 创建了，Pod 也运行起来了。如何才能访问到我们的应用呢？

最直接想到的方法就是直接通过 Pod-ip+port 去访问，但如果实例数很多呢？好，拿到所有的 Pod-ip 列表，配置到负载均衡器中，轮询访问。但上面我们说过，Pod 可能会死掉，甚至 Pod 所在的 Node 也可能宕机，Kubernetes 会自动帮我们重新创建新的Pod。再者每次更新服务的时候也会重建 Pod。而每个 Pod 都有自己的 ip。所以 Pod 的ip 是不稳定的，会经常变化的。

面对这种变化我们就要借助另一个概念：Service。它就是来专门解决这个问题的。不管Deployment的Pod有多少个，不管它是更新、销毁还是重建，Service总是能发现并维护好它的ip列表。Service对外也提供了多种入口：

1. ClusterIP：Service 在集群内的唯一 ip 地址，我们可以通过这个 ip，均衡的访问到后端的 Pod，而无须关心具体的 Pod。
2. NodePort：Service 会在集群的每个 Node 上都启动一个端口，我们可以通过任意Node 的这个端口来访问到 Pod。
3. LoadBalancer：在 NodePort 的基础上，借助公有云环境创建一个外部的负载均衡器，并将请求转发到 NodeIP:NodePort。
4. ExternalName：将服务通过 DNS CNAME 记录方式转发到指定的域名（通过 spec.externlName 设定）。
![image.png](https://upload-images.jianshu.io/upload_images/143845-f52f916cbb7dba83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


好，看似服务访问的问题解决了。但大家有没有想过，Service是如何知道它负责哪些 Pod 呢？是如何跟踪这些 Pod 变化的？


最容易想到的方法是使用 Deployment 的名字。一个 Service 对应一个 Deployment 。当然这样确实可以实现。但k ubernetes 使用了一个更加灵活、通用的设计 - Label 标签，通过给 Pod 打标签，Service 可以只负责一个 Deployment 的 Pod 也可以负责多个 Deployment 的 Pod 了。Deployment 和 Service 就可以通过 Label 解耦了。
![image.png](https://upload-images.jianshu.io/upload_images/143845-9e51a4d9a8825126.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####RollingUpdate - 滚动升级
滚动升级是Kubernetes中最典型的服务升级方案，主要思路是一边增加新版本应用的实例数，一边减少旧版本应用的实例数，直到新版本的实例数达到预期，旧版本的实例数减少为0，滚动升级结束。在整个升级过程中，服务一直处于可用状态。并且可以在任意时刻回滚到旧版本。
![image.png](https://upload-images.jianshu.io/upload_images/143845-c8dbbbfa5ad89bb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

