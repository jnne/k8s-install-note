``` bash
yum remove docker docker-common docker-selinux docker-engine
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum-config-manager --enable docker-ce-edge
yum-config-manager --enable docker-ce-testing
sudo yum-config-manager --disable docker-ce-testing
yum makecache fast
yum install docker-ce
yum list docker-ce.x86_64  --showduplicates | sort -r
yum install docker-ce-<VERSION>
```
