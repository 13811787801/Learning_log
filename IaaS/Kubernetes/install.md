# 安装 Kubernetes

> homebrew 官网 [https://brew.sh/](https://brew.sh/)


`yum install -y kubelet kubeadm kubectl` # centos安装工具

> kubeadm：用于初始化 Kubernetes 集群
>
> kubectl：Kubernetes 的命令行工具，主要作用是部署和管理应用，查看各种资源，创建，删除和更新组件
>
> kubelet：主要负责启动 Pod 和容器

## 在 Mac 上安装

### install kubectl

1. 下载最新的版本 curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

2. 授予可执行文件 `chmod +x ./kubectl`

3. 移动文件到Path中 `sudo mv ./kubectl /usr/local/bin/kubectl`

4. 查看版本信息 `kubectl version`

### Install minikube

> minikube 是一个可以在本地轻松运行 Kubernetes 的工具。Minikube 会在您的笔记本电脑中的虚拟机上运行一个单节点的 Kubernetes 集群，以便用户对 Kubernetes 进行试用或者在之上进行 Kubernetes 的日常开发。

1. brew安装minikube `brew install minikube`

2. 虚拟管理系统 `brew install hyperkit`

3. 启动 minikube start --vm-driver=hyperkit
