# dev_MesosRH7
Mesos on RHEL 7.x

#### Mesos, Marathon Updated Versions
  - mesos-0:1.11.0-2.0.1.el7.x86_64  <br/>
# - mesos-0:1.4.1-2.0.1.x86_6  <br/>4
  - marathon-0:1.4.13-1.0.683.el7.x86_64 <br/>
# - marathon-0:1.4.9-1.0.668.el6.x86_64  <br/>

#### Create VMs
>-VirtualBox (2 Cores, 2Gb RAM, 100GB Storage, 2 NICs)<br/>
>-Base Install RHEL7.x (Infrastructure Server)<br/>
> https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_atomic_host/7/html/installation_and_configuration_guide/types_of_installation <br/>

#### Assign DNS
>10.100.10.190 mesos000<br/>
>10.100.10.191 mesos001<br/>
>10.100.10.192 mesos002<br/>
>10.100.10.193 mesos003<br/>
>10.100.10.194 mesos004<br/>
>10.100.10.195 mesos005<br/>

#### Install JAVA
`#yum install java-1.8.0-openjdk`<br/>

####

#### All Nodes
```
#$sudo rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-1.noarch.rpm
$sudo rpm -Uvh http://repos.mesosphere.com/el/7/noarch/RPMS/mesosphere-el-repo-7-2.noarch.rpm
$sudo yum -y install mesos marathon
```

#### Install Zookeeper on Master Nodes
`$sudo yum -y install mesosphere-zookeeper`<br/>

#### Configure Zookeeper and Mesos Masters on Master Nodes
`$sudo yum install zookeeper zkdump nmap-ncat`<br/>

### Configure Zookeeper
Edit /etc/zookeeper/oo.cfg to contain the following: <br/>
>server.1=mesos001:2888:3888
>server.2=mesos002:2888:3888
>server.3=mesos003:2888:3888
>server.4=mesos004:2888:3888
>server.5=mesos005:2888:3888
>autopurge.snapRetainCount=10
>autopurge.purgeInterval =12

