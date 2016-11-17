# docker@kmutnb
## Nov 17, 2016

## Docker OS

![docker os](https://i2.wp.com/www.inovex.de/blog/wp-content/uploads/2015/05/docker-os-vergleich.jpg)

## Install CentOS Atomic

```
vagrant box add centos/atomic-host
mkdir atomic
cd atomic
vagrant init centos/atomic-host
vagrant up --provider libvirt
vagrant ssh

```

## Create Docker Volume

```
$ sudo docker volume create --name  pgdata
mydata






