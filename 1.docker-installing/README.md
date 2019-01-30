# docker installing
even the kubernetes is a good tool to manage the docker, but it's running in the docker environment. so the first step is prepare a docker running environment.

## basic preparation
install some basic tools for further work require.
> yum install –y vim unzip wget

if there is an old version, please remove it firstly, or, skip to the next step:
> yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine

install some important tools:
> yum install -y yum-utils device-mapper-persistent-data lvm2

add the yum source of docker:
> yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

check the docker packages list:
> yum list docker-ce.x86_64 --showduplicates | sort -r

install the listed docker, I choose the docker-ce-18.06.0.ce-3.el7
> yum -y install docker-ce-18.06.0.ce-3.el7

you can also choose other version for installing.

<NOTES> ALL SERVER MUST DO THIS


 