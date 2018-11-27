安装Docker
===================

运行如下指令：

*表 1： 操作一：安装docker*

.. list-table::
   :header-rows: 1

   * - 指令
     - 备注
   * - yum install docker
     - **注意**: 安装docker，\#请注意，这里不是docker-io，是docker。
   * - systemctl start docker
     - 启动docker，这里注意下，centos 7是采用systemctl来管理启动项
   * - systemctl enable docker
     - 让docker成为开机启动项

*表 2：操作二：新建映射目录*

.. list-table::
   :header-rows: 1

   * - 指令
     - 备注
   * - mkdir /root/dockerdata/box
     -
   * - mkdir /root/dockerdata/data
     -
   * - mkdir /root/dockerdata/config
     -
   * - vim /etc/hosts
       10.21.3.16 registry.envisioncn.com
     - 添加解析
