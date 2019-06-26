# 下载Edge配置文件并初始化Edge镜像

## 创建脚本

```sh
vi download-box-env.sh
url='http://10.21.18.226:8080/eostoolset/download?type=env&boxid='
dir='default_box'
box=''
file='box.env'
while getopts "b:f:" arg
do
case $arg in
b)
box=$OPTARG
echo 'get box: '$box
;;
f)
file=$OPTARG
echo 'save to: '$file
;;
?)
echo "unkown argument"
exit 1
;;
esac
done
download="'"$url$box"' -O $file"
cmd_wget='wget'
echo 'command: '$cmd_wget $download
$cmd_wget $url$box -O $file
```

在上述脚本中，URL是Edge注册环境地址，需要根据你使用的环境来配置。不同环境的地址，见下表：


.. list-table:: 
   :widths: auto

   * - 环境
     - URL
   * - 欧洲
     - https://eoseu.envisioncn.com/configuration/
   * - 美国
     - https://eosus.envisioncn.com/configuration/
   * - 中国AWS
     - https://eos.envisioncn.com/configuration/
   * - 中国Azure
     - https://eosaz.envisioncn.com/configuration/


## 运行脚本以下载Edge配置文件

```bash
bash download-box-env.sh -b 4afc74ad-401b-41c1-ad8c-dfcfa5b5078b
```

在上述命令行中，`4afc74ad-401b-41c1-ad8c-dfcfa5b5078b`是Edge的序列号，请用你在上一步获取的Edge序列号替换掉命令行中的该项参数。

## 安装Edge镜像并初始化参数

```dockerfile
docker run --name eos-edge --restart=always --env INSTANCE_SPEC=dell3000 --env BOX_ID=4afc74ad-401b-41c1-ad8c-dfcfa5b5078b --env  LOCAL_PUBLIC_KEY=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDKR8er9d1vFWrJIkVGEXqChfUYqWug4wK9RgV2A9Lc8P1mGqXBIfJcpevhCsijQmCfpwqx/p36ULCfNy/590d3guybfXfcELYG2MXGnjTgeSBj5bhqAObpW/78YomlnFq29KSCHqBw9TXmm6JvNebUUTUnKUe2GUWRv5XVEMnegwIDAQAB   --env   GLOBAL_PUBLIC_KEY=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCh+x8P5evInljwkALg9Qro20BJ9LOHndtvnI/yPrj5LqKeF7HkR/F1t+EDetF5/LQhOvML4xPSr9QQyuL51aCYJG8w/Ijpqp6pxNtTyEE61vj23KRxAYQ9rz -v /root/dockerdata/box:/home/envuser/box -v /root/dockerdata/data:/data -v /root/dockerdata/config:/data/apps/config/lionconfig/config/   registry.envisioncn.com/eos-all/cloudedge: tag-20180515-001
```

以下参数需要用 _box.env_ 文件中的参数替换：
- INSTANCE_SPEC
- BOX_ID
- LOCAL_PUBLIC_KEY
- GLOBAL_PUBLIC_KEY

以下参数需要向Envision产品经理或者客户支持代表询问以获得：
-  tag-20180515-001

## 验证安装

运行以下命令，查看服务是否可以正常启动。如返回"docer service run normally"消息，则表示安装成功。
1. 运行下列命令，检车Docker服务可用性：
   
  ```sh
  container_id=`docker ps |   grep eos-edge | awk '{print $1}'` && number=`docker exec   $container_id ps -ef | grep -E 'gateway|activemq|redis|conf_client|conn_clt|rtc|energy-os/fe' | wc -l`   &&  [ $number -eq 7 ]   && echo "docker service run normally" || echo "please   wait"
  ```

  如果中文字符无法显示，请将编码方案设置为UTF-8。

2. 运行下列命令，检查Edge与云端的连接是否正常，返回"Connected to the cloud"消息表示连接正常，安装成功；否则请咨询EnOS技术支持人员。

  ```sh
  test=`netstat -ano | grep   -E '8099|8043' | grep -i est | awk '{print $5}' | wc -l` && [ $test   -eq 0 ] && echo "Not connected to the cloud yet" || echo   "Connected to the cloud"
  ```

<!--end-->
