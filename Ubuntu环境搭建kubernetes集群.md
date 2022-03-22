- 以下步骤均在root用下操作，借鉴了文章如下：  
[K8S的安装(Ubuntu 20.04)](https://www.jianshu.com/p/520d6414a4ab)
## 1. 关闭swap
```cassandraql
swapoff -a 
```

## 2. 安装docker
参考[官网安装教程](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)

## 3. 更换源
- 切换目录至/etc/apt，然后替换sources.list文件中的内容：
```cassandraql
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse                          
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu bionic stable
# deb-src [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu bionic stable
```

## 4. 更新源
```cassandraql
# 使得 apt 支持 ssl 传输
apt-get update && apt-get install -y apt-transport-https
# 下载 gpg 密钥   这个需要root用户否则会报错
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
# 添加 k8s 镜像源 这个需要root用户否则会报错
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
# 更新源列表
apt-get update
# 下载 kubectl，kubeadm以及 kubelet
apt-get install -y kubelet kubeadm kubectl
```

## 5. 安装master节点
```
# 先拉取coredns的镜像否则下一步会失败
docker pull coredns/coredns:1.8.4
docker tag coredns/coredns:1.8.4 registry.aliyuncs.com/google_containers/coredns:v1.8.4

# 初始化
kubeadm init \
--apiserver-advertise-address=192.168.196.141 \
--image-repository registry.aliyuncs.com/google_containers \
--pod-network-cidr=10.244.0.0/16
```

## 6. 如果报错，表示kubernetes和docker使用的cgroup不一致
```cassandraql
The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.
```

## 7. 更改docker的cgroup
```cassandraql
# Create daemon json config file
sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

# Start and enable Services
sudo systemctl daemon-reload 
sudo systemctl restart docker
sudo systemctl enable docker
```

## 8. kubeadm init初始化报错可以执行kubeadm reset复原，后重新执行，以下为执行成功的现象：
```cassandraql
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.196.141:6443 --token i9rpmm.8jqs342cmyj1hfwg \
    --discovery-token-ca-cert-hash sha256:95f83d494d4d484945f8017a70bf2f7c6b238c8ca844d431facc4b19dc4105f2
```

## 9. 配置kubectl工具
```cassandraql
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## 10. 测试kubectl是否可用
```cassandraql
# 查看已加入的节点
kubectl get nodes
# 查看集群状态
kubectl get cs
```

## 11. 部署 flannel 网络
```cassandraql
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml
```

## 12. 其他子节点安装前三步安装好kubernetes tools后，执行
```cassandraql
kubeadm join 192.168.196.141:6443 --token i9rpmm.8jqs342cmyj1hfwg \
    --discovery-token-ca-cert-hash sha256:95f83d494d4d484945f8017a70bf2f7c6b238c8ca844d431facc4b19dc4105f2
```

## 13. 如执行kubectl get nodes,发现节点为notready状态，查看日志：
```cassandraql
"Container runtime network not ready" networkReady="NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized"
```

## 14. 解决方案：
- 登录如下网址，查看raw.githubusercontent.com对应的ip地址：
[查看IP](http://ip.tool.chinaz.com/raw.githubusercontent.com)

- 将ip地址和域名写入/etc/hosts文件
```cassandraql
185.199.110.133 raw.githubusercontent.com
```

## 15. 重新执行
```cassandraql
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

## 16. 重启kubelet
```cassandraql
systemctl restart kubelet
```
