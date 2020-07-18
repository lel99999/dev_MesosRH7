# dev_MesosRH7
Mesos on RHEL 7.x

#### Assign DNS
>10.100.10.190 mesos000<br/>
>10.100.10.191 mesos001<br/>
>10.100.10.192 mesos002<br/>
>10.100.10.193 mesos003<br/>
>10.100.10.194 mesos004<br/>
>10.100.10.195 mesos005<br/>

#### All Nodes
```
$sudo rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-1.noarch.rpm
$sudo yum -y install mesos marathon
```

#### Install Zookeeper on Master Nodes
`$sudo yum -y install mesosphere-zookeeper`<br/>

