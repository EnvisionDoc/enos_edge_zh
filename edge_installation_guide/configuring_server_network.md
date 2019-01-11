# 服务器网络配置

通过运行如下指令完成服务器的网络配置：

.. list-table:: 表 1：操作一：编辑网络配置文件
   :widths: auto

   * - 指令
     - 备注
   * - vim/etc/sysconfig/network-scripts/ifcfg-ens32
     - null
   * - DEVICE=ens32
     - null
   * - IPADDR=xxx.xxx.xxx.xxx
     - 使用已规划的IP地址
   * - NETMASK=255.255.255.0
     - null
   * - GATEWAY=yyy.yyy.yyy.yyy
     - 使用已规划的网关
   * - ONBOOT=yes
     - null
   * - NAME=ens32
     - null
   * - DNS1=zzz.zzz.zzz.zzz
     - 使用已规划的DNS


.. list-table:: 表 2：操作二：启动ssh服务
   :widths: auto

   * - 指令
     - 备注
   * - systemctl restart sshd
     - null
   * - systemctl enable sshd
     - null


完成以上配置后，既可以远程管理Edge及进行后续的安装操作。

<!--end-->
