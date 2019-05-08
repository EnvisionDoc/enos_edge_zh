# 数据采集

## 采集功能说明

结合云端的设备模型和设备规约库，Edge能够快速地支持绝大多数能源设备的接入。Edge中提供的设备模板库，保存了所有接入过的设备型号的测点，规约及配置文件，原始点与标准领域点的映射，测点映射对应的处理算子等信息。当再次需要接入相同型号的设备时，只需要在设备模板库中选择对应的设备模板就可完快速完成设备数据接入。

当前EnOS™ Edge的设备模块库中，已积累了绝大多数能源设备型号的设备，能够光伏应用于风电，光伏，园区，楼宇等场景中。同时，借助于Edge中的前置模块对接入数据的灵活处理，EnOS™
Edge也可以快速支持第三方应用系统转发而来的数据接入。

.. note:: 当前Edge支持采集的数据都是指实时数据。

.. list-table:: 表：EnOS™ Edge支持数据采集的设备类型

   * - 应用领域
     - 设备类型
     - 备注
   * - 光伏
     - 逆变器
     - 设备
   * - 汇流箱
     - 设备
     - --
   * - 气象站
     - 设备
     - --
   * - 电能表
     - 设备
     - --
   * - 集中式电站监控系统
     - 第三方系统
     - --
   * - 升压站综自系统
     - 设备
     - --
   * - 风能
     - 风机
     - 设备
   * - 箱变
     - 设备
     - --
   * - 测风塔
     - 设备
     - --
   * - 升压站综自系统
     - 第三方系统
     - --
   * - 工业园区/商业楼宇
     - 电能表
     - 设备
   * - 充电桩
     - 设备
     - --
   * - 配电管理系统
     - 第三方系统
     - --
   * - 储能电池PCS
     - 设备
     - --
   * - 传统电力(火电、水电、燃气发电等)
     - DCS/综自后台系统等
     - 第三方系统

.. note:: 包括但不限于表中所列出的类型。

## 数据时间戳处理

为支持云端上层应用复杂的数据处理逻辑，Edge不仅支持对接入数据打上接入时刻的最新时标，同时也支持保留设备数据上送的原始时间。具体处理规则如下：

- 数据原始时间将会保存在每一条数据的一个属性中上传的云端供领域应用灵活使用；

- Edge对接入时间打时标时，都使用UTC+0时间进行统一时间标注；

- Edge中的前置接入模块支持对不同规约上送的数据原始时标进行时区转换配置，如某一种设备型号上送的原始数据时标是UTC+8时区,则可以做相应配置，将接入的数据原始时标转换成UTC+0时区。

## 支持规约列表

Edge的数据采集能力，主要是基于Edge对各种通讯规约的支持能力。EnOS™ Edge规约库中不仅积累了大量的行业标准通讯规约，同时也有丰富的特定类型设备的私有通讯规约。

.. list-table:: 表：EnOS™ Edge支持的行业标准规约

   * - 规约
     - 数据类型
     - 通讯类型
     - 网络
   * - ModbusTCP
     - Bit/Short/Ushort/Int/Uint/Float
     - Client/Server
     - 以太网
   * - ModbusRTU
     - Bit/Short/Ushort/Int/Uint/Float
     - Master
     - 需要转换成以太网
   * - IEC60870-5-104
     - Int/Float
     - Client/Server
     - 以太网
   * - OPC-DA
     - Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool
     - Client
     - 以太网
   * - OPC-XML-DA
     - Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool
     - Client
     - 以太网
   * - OPC-UA
     - Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool
     - Client
     - 以太网
   * - HTTP(s)
     - Int/Float
     - Web Service
     - 以太网
   * - DL/T645-1997
     - Int
     - Master
     - 需要转换成以太网
   * - DL/T645-2007
     - Int
     - Master
     - 需要转换成以太网
   * - BACnet
     - Bool/Double/Uint/Int/Real/Enumerated
     - Client
     - 以太网
   * - ADS
     - Uint/Double/Float/Int/Long/Ushort/Short/
     - Client
     - 以太网

.. note:: 该表是支持规约的大类，Edge详细准备的规约支持列表请参看附件。


<!--end-->
