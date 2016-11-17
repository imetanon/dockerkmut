# docker@kmutnb
## Nov 17, 2016

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
### 

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








