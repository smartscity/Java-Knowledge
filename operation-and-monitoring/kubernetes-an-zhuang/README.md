# Kubernetes 介绍

## 安装

* **Minikube** base on VM
* **microk8s** for Linux
* **Kubeadm** 
* a
* **Rancher**

## 网络

使Service暴露应用

* **ClusterIP（默认）**
  * 仅在集群内部IP上暴露服务，此类型使Service只能从群集中访问。
* **NodePort**
  * 通过每个 Node 上的 IP 和静态端口（NodePort）暴露服务（30000~）
* **LoadBalancer**
  * 使用云提供商的负载均衡器
* **Ingress**
* \*\*\*\*
* **ExternalName**

## **架构图**

![](../../.gitbook/assets/image-31.png)

## 下载

* `wget https://dl.k8s.io/v1.12.0-rc.2/kubernetes-server-linux-amd64.tar.gz` 下载地址

## Installing kubeadm, kubelet and kubectl

* 镜像配置

```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
       http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

* 关闭防火墙

```bash

```

* 安装

```bash
# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
sed -i 's/^SELINUX=disabled$/SELINUX=permissive/' /etc/selinux/config


yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
systemctl enable kubelet && systemctl start kubelet
```

* **Note**

```bash
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```

* 初始化 `kubeladm`

```bash
kubeadm init
```

