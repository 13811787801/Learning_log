# 安装 Kubernetes

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
