**k8s 安装文档**

**一些简介**

* 本文档采用k8s v1.10.0二进制的集群部署方式，主要更改 使用kube-router 代理kube-proxy,使用ingress-nginx做边缘负载,使用haproxy+heartbeat实现高可用

	* 本文档持续更新，后续将继续深入了解prometheus，helm等组件，已经投产之后的一些故障和高可用方案。
