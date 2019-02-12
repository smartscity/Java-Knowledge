# Minikube

### 安装

```bash
curl -Lo minikube http://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/releases/v0.30.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

### 驱动器

```bash
--vm-driver= virtualbox /vmwarefusion/kvm2/kvm/hyperkit/xhyve/
```

### 启动

```bash
minikube start --registry-mirror=https://registry.docker-cn.com \
                                --kubernetes-version v1.12.1  \
                                --vm-driver="virtualbox"
```

### 

### aaaa

### helm init --service-account tiller --tiller-image registry.cn-hangzhou.aliyuncs.com/google\_containers/tiller:latest

### 参考资料

* [https://yq.aliyun.com/articles/221687](https://yq.aliyun.com/articles/221687)
* [https://istio.io/docs/setup/kubernetes/platform-setup/minikube/](https://istio.io/docs/setup/kubernetes/platform-setup/minikube/)



