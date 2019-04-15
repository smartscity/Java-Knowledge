# Kubeadm

## 追踪日志

{% embed url="https://blog.csdn.net/shenhonglei1234/article/details/82421742" caption="" %}

```java
追踪日志 
要主动追踪当前正在编写的日志，大家可以使用-f标记。同样功能类似为tail -f，只要不终止，会一直监控 
journalctl -f 

也许最有用的过滤方式是你感兴趣的单位。我们可以使用这个-u选项来过滤我们可以使用这个-u选项来过滤 
journalctl -u 

所以我们最终使用的命令是： 
journalctl -f -u kubelet
```

## [https://www.cnblogs.com/Irving/p/9818440.html](https://www.cnblogs.com/Irving/p/9818440.html)

## [https://blog.csdn.net/tiger435/article/details/85002337](https://blog.csdn.net/tiger435/article/details/85002337)

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

