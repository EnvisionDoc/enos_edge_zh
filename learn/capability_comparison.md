# EnOS Edge各型号能力列表

EnOS Edge有以下型号可供选择，各型号的能力不同，适用于不同的场景和要求。

## 规格比较

EnOS Edge各型号的规格如下表：

.. csv-table:: EnOS Edge产品系列
      :header: "产品系列", "定位", "场景", "特点"
      :widths: auto

      "Edge Gateway Logger", "轻量级网关和数据存储", "分布式光伏、楼宇储能、工业园区、商业楼宇、智慧城市及其他轻量级边缘计算场景", "基于ARM架构，性价比高"
      "Edge Gateway", "小规模至大规模网关", "集中式光伏、风能、大型工业园及其他场景，大规模数据传输和轻量级边缘计算", "基于X86架构，性能高"
      "Edge Gateway STD", "网关及运行本地应用的平台", "网关场景及本地应用，如SCADA、告警、数据上报、EnLight等等", "基于X86架构，具有多种服务和API，能够运行应用"
      "Edge Extensive", "区域小型数据中心、分布式部署", "具有监控、控制及Enlight数据分析功能的区域小型数据中心", "基于X86架构，能够运行应用，用于分布式部署场景"

## 功能对比

不同型号的Edge具有的能力如下表：

在下表中，**Yes** 代表该功能在该型号Edge的任何硬件配置下都可以运行。**No**代表该功能无法在该型号Edge上被支持。**Partial** 指的是该功能可以在该型号Edge的部分版本上运行，有一定的硬件规格要求。

.. csv-table:: Edge功能对比
   :header: "功能", "Edge Gateway Logger", "Edge Gateway", "Edge Gateway STD", "Edge Extensive"
   :widths: auto

   "远程配置", "Yes", "Yes", "Yes", "Yes"
   "协议转换", "Yes", "Yes", "Yes", "Yes"
   "数据断点续传", "Yes", "Yes", "Yes", "Yes"
   "边缘计算公式及脚本", "Yes", "Yes", "Yes", "Yes"
   "边缘计算StreamSets", "No", "Partial", "Yes", "Yes"
   "数据转发", "Yes", "Yes", "Yes", "Yes"
   "无线固件升级（OTA）", "Yes", "No", "No", "No"
   "高可用性主备部署（HA）", "No", "Yes", "Yes", "Yes"
   "数据服务", "No", "No", "Yes", "Yes"
   "模型和资产服务", "No", "No", "Yes", "Yes"
   "告警引擎", "No", "No", "Yes", "Yes"
   "API服务", "No", "No", "Yes", "Yes"
   "应用管理", "No", "No", "Yes", "Yes"
   "IAM及App Portal", "No", "No", "Yes", "Yes"
   "TSDB", "No", "Partial", "Yes", "Yes"

<!--The end-->