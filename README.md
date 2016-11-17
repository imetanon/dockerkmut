# docker@kmutnb
## Nov 17, 2016

## Docker Technology on Linux kernel
1. base Technology Stack on linux

![docker kernel](https://www.ociweb.com/files/1814/3050/6411/Dockertechstack.png)

2. Docker's Architecture
![architecture](https://docs.docker.com/engine/article-img/architecture.svg)
* The Docker daemon
  The Docker daemon runs on a host machine. The user uses the Docker client to interact with the daemon.

* The Docker client
  The Docker client, in the form of the docker binary, is the primary user interface to Docker. It accepts commands and configuration flags from the user and communicates with a Docker daemon. One client can even communicate with multiple unrelated daemons.

## Docker vs Virtual machine
![docker](http://blog.jayway.com/wp-content/uploads/2015/03/vm-vs-docker.png)
The image describes the difference between a VM and Docker. Instead of a hypervisor with Guest OSes on top, Docker uses a Docker engine and containers on top. Does this really tell us anything? What is the difference between a "hypervisor" and the "Docker engine"? A nice way of illustrating this difference is through listing the running processes on the Host.

## Underline Technology
![enjoy](https://docs.docker.com/opensource/images/docker-friends.png)
Docker is written in Go and takes advantage of several features of the Linux kernel to deliver its functionality.

1. Namespace
  * The ``pid`` namespace: Process isolation (PID: Process ID).
  * The ``net`` namespace: Managing network interfaces (NET: Networking).
  * The ``ipc`` namespace: Managing access to IPC resources (IPC: InterProcess Communication).
  * The ``mnt`` namespace: Managing filesystem mount points (MNT: Mount).
  * The ``uts`` namespace: Isolating kernel and version identifiers. (UTS: Unix Timesharing System).

2. Control groups
Docker Engine on Linux also relies on another technology called control groups (cgroups). A cgroup limits an application to a specific set of resources

3. Union File System
Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.

## Docker OS

![docker os](https://i2.wp.com/www.inovex.de/blog/wp-content/uploads/2015/05/docker-os-vergleich.jpg)
1. CoreOS
2. Project Atomic
3. Ubuntu Snappy
4. RancherOS
5. Photon
## Install Docker Engine
1. [on mac](https://docs.docker.com/docker-for-mac/)
2. [on window](https://docs.docker.com/docker-for-windows/)
3. [ubuntu](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
4. [fedora](https://docs.docker.com/engine/installation/linux/fedora/)
5. [Debian](https://docs.docker.com/engine/installation/linux/debian/) 

## Install Fedora 24
### install docker with script
```
curl -fsSL https://get.docker.com/ | sh
sudo systemctl enable docker.service
sudo systemctl start docker
```
### install docker machine
```
$ curl -L https://github.com/docker/machine/releases/download/v0.8.2/docker-machine-`uname -s`-`uname -m` >/usr/local/bin/docker-machine && \
chmod +x /usr/local/bin/docker-machine
```
### Install docker machine kvm
```
curl -L https://github.com/dhiltgen/docker-machine-kvm/releases/download/v0.7.0/docker-machine-driver-kvm > /usr/local/bin/docker-machine-driver-kvm && \
  chmod +x /usr/local/bin/docker-machine-driver-kvm
```
### Copy key from docker machine to docker client
```
cd /home/admin/.docker/machine/certs/
ls
ca-key.pem  ca.pem  cert.pem  key.pem
sudo cp * /etc/docker/
sudo chmod 644 /etc/docker/*
```
### Docker machine command
```
docker-machine create development -d kvm --kvm-cpu-count 2 --kvm-disk-size 40000
docker-machine env development
eval "$(docker-machine env development)"
docker-machine ssh
```
## Install CentOS Atomic

```
vagrant box add centos/atomic-host
mkdir atomic
cd atomic
vagrant init centos/atomic-host
vagrant up --provider libvirt
vagrant ssh

```
## Docker in Action
### Python
```
$ sudo docker run -ti python bash
Unable to find image 'python:latest' locally
Trying to pull repository docker.io/library/python ... 
latest: Pulling from docker.io/library/python
386a066cd84a: Already exists 
75ea84187083: Pull complete 
88b459c9f665: Pull complete 
1e3ee139a577: Pull complete 
729baab2cba2: Pull complete 
6792e43546b2: Pull complete 
61cd79806d9e: Pull complete 
Digest: sha256:845576d452e927e6d72502daf2f0e328b2464b56818786533dbcf18d42ccd827
Status: Downloaded newer image for docker.io/python:latest

root@79fb71e4d51e:/# pip install IPython
root@79fb71e4d51e:/# ipython
Python 3.5.2 (default, Nov 10 2016, 08:25:20) 
Type "copyright", "credits" or "license" for more information.

IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: 
```
### Ubuntu 
```
$ sudo docker run -i -t ubuntu /bin/bash

Unable to find image 'ubuntu:latest' locally
Trying to pull repository docker.io/library/ubuntu ... 
latest: Pulling from docker.io/library/ubuntu
aed15891ba52: Pull complete 
773ae8583d14: Pull complete 
d1d48771f782: Pull complete 
cd3d6cd6c0cf: Pull complete 
8ff6f8a9120c: Pull complete 
Digest: sha256:35bc48a1ca97c3971611dc4662d08d131869daa692acb281c7e9e052924e38b1
Status: Downloaded newer image for docker.io/ubuntu:latest

root@3fd3ea4bdfa8:/#
```
### Debian
```
$ sudo docker run -i -t debian:jessie bash
Unable to find image 'debian:jessie' locally
Trying to pull repository docker.io/library/debian ... 
jessie: Pulling from docker.io/library/debian
386a066cd84a: Already exists 
Digest: sha256:c1ce85a0f7126a3b5cbf7c57676b01b37c755b9ff9e2f39ca88181c02b985724
Status: Downloaded newer image for docker.io/debian:jessie
root@6766f2ea6fa2:/#
```
## Create Docker Volume
![volume](https://clusterhq.com/assets/images/simple-flocker-animation.gif)
```
$ sudo docker volume create --name pgdata
pgdata

$ sudo docker volume ls
DRIVER              VOLUME NAME
local               pgdata

$ sudo docker run -d -v pgdata:/var/lib/postgresql/data --name pgdb  postgres
Unable to find image 'postgres:latest' locally
Trying to pull repository docker.io/library/postgres ... 
latest: Pulling from docker.io/library/postgres
386a066cd84a: Pull complete 
e6dd80b38d38: Pull complete 
9cd706823821: Pull complete 
40c17ac202a9: Pull complete 
7380b383ba3d: Pull complete 
538e418b46ce: Pull complete 
c3b9d41b7758: Pull complete 
dd4f9522dd30: Pull complete 
920e548f9635: Pull complete 
628af7ef2ee5: Pull complete 
211678575a06: Pull complete 
Digest: sha256:3da198a1846d1fa6cf55978c8326d5c7e801155843c469ce9213cdbb25b5ae33
Status: Downloaded newer image for docker.io/postgres:latest
b440befdb5c420353756ae488f693c76104b5d06023800753a19ce833d216138

$ ls /var/lib/docker/volumes/pgdata/_data
base     pg_commit_ts  pg_ident.conf  pg_notify    pg_snapshots  pg_subtrans  PG_VERSION            postgresql.conf
global   pg_dynshmem   pg_logical     pg_replslot  pg_stat       pg_tblspc    pg_xlog               postmaster.opts
pg_clog  pg_hba.conf   pg_multixact   pg_serial    pg_stat_tmp   pg_twophase  postgresql.auto.conf  postmaster.pid

```
### Delete container and recreate

```
$ docker rm -f pgdb
$ sudo docker run -d -v pgdata:/var/lib/postgresql/data --name pgdb2  postgres
```

### Inspect docker

```
$ sudo docker inspect pgdb
...
        "Mounts": [
            {
                "Name": "pgdata",
                "Source": "/var/lib/docker/volumes/pgdata/_data",
                "Destination": "/var/lib/postgresql/data",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": "rslave"
            }
        ],
...
``` 
## Create Own Image

1. pull base image
2. Install package
3. commit docker to create image

### Add http to base centos
```
$ sudo docker pull centos
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos     latest              0584b3d2cf6d        2 weeks ago         196.5 MB

$ sudo docker run -it docker.io/centos /bin/bash
[root@d65459027741 /]#

[root@d65459027741 /]# yum install httpd
[root@d65459027741 /]# exit

$ sudo docker ps --all
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                      PORTS               NAMES
d65459027741        docker.io/centos    "/bin/bash"              About a minute ago   Exited (0) 11 seconds ago                       jolly_joliot

$ sudo docker commit -m "Add http to base Centos" -a "sawangpong M." d65459027741 itbakery/centos7_http:v0.1
sha256:734a14f07b6084c144273b1d93104a0eac182d092b872928743c5731c77b3f8f

$ sudo docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
itbakery/centos7_http   v0.1                734a14f07b60        32 seconds ago      338.3 MB

```

####  Create container from image
create container from image 'itbakery/centos7_http'
```
sudo docker run -ti itbakery/centos7_http:v0.1
[root@df35d54b6b78 /]# 
```

### Create Image Debian
```
sudo docker run -i -t debian:jessie bash
root@bffb8fb432de:/# apt-get update
root@bffb8fb432de:/# apt-get install postgresql

sudo docker commit bffb8fb432de debian_postgresql
sha256:59680951223ecbbf3002efd2875400827f40cf87b308c70dd90d7d3d72d9102e

```

## Create Image from Dockerfile
1. create newfolder name project
2. Create ``Dockerfile`` in project

```
FROM debian:jessie
# Dockerfile for postgres-iojs

RUN apt-get update
RUN apt-get install -y postgresql
```
3. build image from Dockerfile
```
sudo docker build --tag postgres-debian .
```





