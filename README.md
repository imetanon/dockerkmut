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

## Underline Technology
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

## Create Docker Volume

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








