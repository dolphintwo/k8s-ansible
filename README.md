## kubernetes 离线安装

## 使用 ansible 命令进行集成安装

使用方式:

1.  在中间机生成 ssh 公私玥
2.  在需要备ansible控制的机器注入中间机密钥
3.  配置hosts文件,设置机器角色
4.  配置依赖文件，将需要的文件拷贝到files目录
5.  在此工程根目录下执行
        ansible-playbook -i hosts site.yml
6.  等待命令执行完成

## 安装顺序

1. install_etcd.yml
2. install_k8s_master.yml
3. install_k8s_node.yml
4. install_registry.yml
5. install_k8s_addon.yml

安装失败回滚(环境清除)

1. uninstall.yml

## 通过 kubelet 的 TLS 证书请求

kubelet 首次启动时向 kube-apiserver 发送证书签名请求，必须通过后 kubernetes 系统才会将该 Node 加入到集群。

在master节点查看未授权的 CSR 请求

    $ kubectl get csr
    NAME        AGE       REQUESTOR           CONDITION
    csr-2b308   4m        kubelet-bootstrap   Pending

通过 CSR 请求：

    $ kubectl certificate approve csr-2b308
    certificatesigningrequest "csr-2b308" approved
    $ kubectl get nodes
    NAME        STATUS    AGE       VERSION
    10.64.3.7   Ready     49m       v1.6.2


## kubernetes 版本说明
1. kubernetes 版本　1.6.2
2. docker 1.12.6
3. etcd 3.1.6
4. Flanneld 0.7.1 vxlan 网络

## ansible 角色文件说明
1. __cfssl__ 证书工具安装，对应所有机器
2. __ca_grant__ 证书文件生成
3. __kubectl__ kubectl 命令行安装
4. __etcd__ etcd 服务安装
5. __flannel__ flannel网络安装,所有kubernetes 运行主机需要安装
6. __docker__ docker安装
7. __k8smaster__ kubernetes master 节点安装
8. __k8snode__ kubernetes node节点安装

## 参考文档

[和我一步步部署 kubernetes 集群](https://github.com/opsnull/follow-me-install-kubernetes-cluster) 主要参考kubernetes集群安装方式
    
[Kubernetes 1.6 高可用集群部署](http://blog.frognew.com/2017/04/install-ha-kubernetes-1.6-cluster.html) 主要参考master的HA方案

