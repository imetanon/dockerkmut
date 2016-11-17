# docker@kmutnb
## Nov 17, 2016


# Install CentOS Atomic

```
vagrant box add centos/atomic-host
mkdir atomic
cd atomic
vagrant init centos/atomic-host
vagrant up --provider libvirt
```
