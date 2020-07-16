### CentOS Notes



### Fix: "cannot find a valid baseurl for repo" on CentOS 7

出现这个问题是因为yum在安装包的过程中，虽然已经联网，但是没法解析远程包管理库对应的域名，所以我们只需要在网络配置中添加上DNS对应的ip地址即可。

```shell
# find your Internet name， general it is eth0. also like ens33.
$ ip addr 
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0 # append following lines bottom of the file.
DNS1=8.8.8.8
DNS2=4.2.2.2
# restart network
$ ifup eth0
```



### Install Docker on CentOS

Uninstall Old Version

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

Install Docker CE

Install Using the Repository

```shell
# Install required packages.
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
# set up the stable repository
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# Install Docker CE
$ sudo yum install docker-ce docker-ce-cli containerd.io
# Start Docker
$ sudo systemctl start docker
# Verify that Docker CE is installed correctly by running the hello-world image.
$ sudo docker run hello-world
```

Shell Script

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo docker run hello-world
```



Reference:

[Get Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)