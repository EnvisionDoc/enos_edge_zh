# 配置服务器网络

按照以下步骤配置服务器网络：

1. 使用下列命令编辑网络配置文件：
   
  .. csv-table:: 
    :widths: auto  

    "命令", "注释"
    "vim/etc/sysconfig/network-scripts/ifcfg-ens32", "无"
    "EVICE=ens32", "无"
    "IPADDR=xxx.xxx.xxx.xxx", "使用已规划的IP地址"
    "NETMASK=255.255.255.0", "无"
    "GATEWAY=yyy.yyy.yyy.yyy", "使用已规划的IP网关"
    "ONBOOT=yes", "无"
    "NAME=ens32, "无"
    "DNS1=zzz.zzz.zzz.zzz", "使用已规划的DNS"


2. 使用下列命令启动SSH服务：

 ```sh
 $systemctl restart sshd
 $systemctl enable sshd
 ```

完成上述配置后，你就能够远程访问并管理Edge服务器。
