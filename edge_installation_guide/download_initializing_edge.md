# 下载Edge配置文件，镜像初始化Edge

## 创建一个下载Edge配置文件box.env的脚本。

.. list-table:: 表：下创建载Edge配置文件box.env的脚本
   :widths: auto

   * - 指令
     - 备注
   * -
       ```
       vi download-box-env.sh url='http://10.21.18.226:8080/eostoolset/download?type=env&amp;boxid=' dir='default_box' box='' file='box.env' while getopts &quot;b:f:&quot; arg do case $arg in b) box=$OPTARG echo 'get box: '$box ;;
       f)
       file=$OPTARG
       echo 'save to: '$file
       ;;
       ?)
       echo &quot;unkonw argument&quot;
       exit 1
       ;;
       esac
       done
       download=&quot;'&quot;$url$box&quot;' -O $file&quot;
       cmd_wget='wget'
       echo 'command: '$cmd_wget $download
       $cmd_wget $url$box -O $file
       ```

     - --

.. note:: url需要根据不同环境使用不同环境的地址，各环境地址参见[Link](applying_edge)。

## 下载Edge配置文件box.env

.. list-table:: 表：下载Edge注册信息box.env
   :widths: auto

   * - 指令
     - 备注
   * -
       ```
       bash download-box-env.sh -b 4afc74ad-401b-41c1-ad8c-dfcfa5b5078b
       ```

     - SN部分使用之前保存的Edge的SN。

完成下载后，打开box.env文件，获取到下一步中初始化Edge的参数。

## 安装Edge镜像及初始化设置。

.. list-table:: 表： Edge镜像安装及初始化
   :widths: auto

   * - 指令
     - 备注
   * -
       ```
       docker run --name   eos-edge  --restart=always --env    INSTANCE_SPEC=dell3000 --env   BOX_ID=4afc74ad-401b-41c1-ad8c-dfcfa5b5078b --env    LOCAL_PUBLIC_KEY=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDKR8er9d1vFWrJIkVGEXqChfUYqWug4wK9RgV2A9Lc8P1mGqXBIfJcpevhCsijQmCfpwqx/p36ULCfNy/590d3guybfXfcELYG2MXGnjTgeSBj5bhqAObpW/78YomlnFq29KSCHqBw9TXmm6JvNebUUTUnKUe2GUWRv5XVEMnegwIDAQAB   --env   GLOBAL_PUBLIC_KEY=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCh+x8P5evInljwkALg9Qro20BJ9LOHndtvnI/yPrj5LqKeF7HkR/F1t+EDetF5/LQhOvML4xPSr9QQyuL51aCYJG8w/Ijpqp6pxNtTyEE61vj23KRxAYQ9rz01vMuxmITyltRmMhNSCKN95T2++DoxzaEntrV6mWW5cAY55n1IpQIDAQAB   -d --net=host  -v   /root/dockerdata/box:/home/envuser/box -v /root/dockerdata/data:/data -v   /root/dockerdata/config:/data/apps/config/lionconfig/config/   registry.envisioncn.com/eos-all/cloudedge:   tag-20180515-001
       ```

     - 参数需要使用之前下载的box.env中的参数替换；Edge应用Tag："tag-20180515-001" 需要向远景技术团队询问最新版本

## 安装完成后检查校验。

运行如下指令，检查服务是否正常启动，结果显示："docker服务正常"，则继续。

.. list-table:: 表 1：操作一：查看服务是否启动
   :widths: auto

   * - 指令
     - 备注
   * -
       ```
       container_id=`docker ps |   grep eos-edge | awk '{print $1}'` &amp;&amp; number=`docker exec   $container_id ps -ef | grep -E   'gateway|activemq|redis|conf_client|conn_clt|rtc|energy-os/fe' | wc -l`   &amp;&amp;  [ $number -eq 7 ]   &amp;&amp; echo &quot;docker service run normally&quot; || echo &quot;please wait&quot;
       ```

     - 改成UTF-8格式，否则可能显示不出中文。

运行如下指令，检查Edge与云端是否正常连通，结果显示："已连接到云端"，则安装成功。否则请联系远景技术团队支持。

.. list-table:: 表 2：操作二：查看是否有连通到云端
   :widths: auto

   * - 指令
     - 备注
   * -
       ```
       test=`netstat -ano | grep   -E '8099|8043' | grep -i est | awk '{print $5}' | wc -l` &amp;&amp; [ $test   -eq 0 ] &amp;&amp; echo &quot;Not connected to the cloud yet&quot; || echo   &quot; Connected to the cloud &quot;
       ```

     - --

<!--end-->
