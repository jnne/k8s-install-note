# 参考文档

[参考文档 etcd cluster 备份恢复](https://github.com/coreos/etcd/blob/master/Documentation/op-guide/recovery.md)
[etcd性能测试](https://github.com/coreos/etcd/blob/master/Documentation/op-guide/performance.md)


**1.etcd Cluster 备份**

>集群备份：在主机 10.255.72.189上执行：

``` bash
ETCDCTL_API=3 etcdctl \
--cacert=/etc/etcd/ssl/ca.pem \
--cert=/etc/etcd/ssl/etcd.pem \
--key=/etc/etcd/ssl/etcd-key.pem \
--endpoints=https://10.255.72.189:2379,https://10.255.72.190:2379,https://10.255.72.191:2379 snapshot save snapshot.db
```

**2.etcd Cluster 恢复**

> 集群恢复：

#恢复etcd01

``` bash
ETCDCTL_API=3 etcdctl snapshot restore snapshot.db \
  --name etcd01 \
  --initial-cluster etcd01=https://10.255.72.189:2380,etcd02=https://10.255.72.190:2380,etcd03=https://10.255.72.191:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-advertise-peer-urls https://10.255.72.189:2380
```
