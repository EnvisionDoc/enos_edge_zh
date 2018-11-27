# 数据采集

## 采集功能说明

结合云端的设备模型和设备规约库，Edge能够快速地支持绝大多数能源设备的接入。Edge中提供的设备模板库，保存了所有接入过的设备型号的测点，规约及配置文件，原始点与标准领域点的映射，测点映射对应的处理算子等信息。当再次需要接入相同型号的设备时，只需要在设备模板库中选择对应的设备模板就可完快速完成设备数据接入。

当前EnOS™ Edge的设备模块库中，已积累了绝大多数能源设备型号的设备，能够光伏应用于风电，光伏，园区，楼宇等场景中。同时，借助于Edge中的前置模块对接入数据的灵活处理，EnOS™
Edge也可以快速支持第三方应用系统转发而来的数据接入。

**注意**：当前Edge支持采集的数据都是指实时数据。

*表：EnOS™ Edge支持数据采集的设备类型*

<table>
  <tr>
    <td>应用领域 </td>
    <td>设备类型 </td>
    <td>备注 </td>
  </tr>
  <tr>
    <td>光伏 </td>
    <td>逆变器 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>汇流箱 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>气象站 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>电能表 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>集中式电站监控系统 </td>
    <td>第三方系统 </td>
  </tr>
  <tr>
    <td>升压站综自系统 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>风能 </td>
    <td>风机 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>箱变 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>测风塔 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>升压站综自系统 </td>
    <td>第三方系统 </td>
  </tr>
  <tr>
    <td>工业园区/商业楼宇 </td>
    <td>电能表 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>充电桩 </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>配电管理系统 </td>
    <td>第三方系统 </td>
  </tr>
  <tr>
    <td>储能电池PCS </td>
    <td>设备 </td>
  </tr>
  <tr>
    <td>传统电力(火电、水电、燃气发电等) </td>
    <td>DCS/综自后台系统等 </td>
    <td>第三方系统 </td>
  </tr>
</table>

**注意**：包括但不限于表中所列出的类型。

## 数据时间戳处理

为支持云端上层应用复杂的数据处理逻辑，Edge不仅支持对接入数据打上接入时刻的最新时标，同时也支持保留设备数据上送的原始时间。具体处理规则如下：

- 数据原始时间将会保存在每一条数据的一个属性中上传的云端供领域应用灵活使用；

- Edge对接入时间打时标时，都使用UTC+0时间进行统一时间标注；

- Edge中的前置接入模块支持对不同规约上送的数据原始时标进行时区转换配置，如某一种设备型号上送的原始数据时标是UTC+8时区,则可以做相应配置，将接入的数据原始时标转换成UTC+0时区。

## 支持规约列表

Edge的数据采集能力，主要是基于Edge对各种通讯规约的支持能力。EnOS™ Edge规约库中不仅积累了大量的行业标准通讯规约，同时也有丰富的特定类型设备的私有通讯规约。

*表：EnOS™ Edge支持的行业标准规约*

<table>
  <tr>
    <td>规约</td>
    <td>数据类型</td>
    <td>通讯类型</td>
    <td>网络</td>
  </tr>
  <tr>
    <td>ModbusTCP</td>
    <td>Bit/Short/Ushort/Int/Uint/Float</td>
    <td>Client/Server</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>ModbusRTU</td>
    <td>Bit/Short/Ushort/Int/Uint/Float</td>
    <td>Master</td>
    <td>需要转换成以太网 </td>
  </tr>
  <tr>
    <td>IEC60870-5-104</td>
    <td>Int/Float</td>
    <td>Client/Server</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>DNP3.0</td>
    <td>Int/Float</td>
    <td>Client</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>OPC-DA</td>
    <td>Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool</td>
    <td>Client </td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>OPC-XML-DA</td>
    <td>Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool</td>
    <td>Client</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>OPC-UA</td>
    <td>Uint/Double/Float/Int/Long/Char/Ushort/Short/Bool</td>
    <td>Client</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>HTTP(s)</td>
    <td>Int/Float</td>
    <td>Web Service</td>
    <td>以太网 </td>
  </tr>
  <tr>
    <td>DL/T645-1997</td>
    <td>Int</td>
    <td>Master</td>
    <td>需要转换成以太网 </td>
  </tr>
  <tr>
    <td>DL/T645-2007</td>
    <td>Int</td>
    <td>Master</td>
    <td>需要转换成以太网 </td>
  </tr>
</table>

*表：EnOS™ Edge支持的行业标准规约*

<table>
  <tr>
    <td>设备厂家</td>
    <td>型号</td>
    <td>备注</td>
  </tr>
  <tr>
    <td>Growatt</td>
    <td>Growatt</td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">Goodwe</td>
    <td>GoodweTcp</td>
    <td></td>
  </tr>
  <tr>
    <td>GoodweWebService</td>
    <td></td>
  </tr>
  <tr>
    <td>Jinlang</td>
    <td>JinlangTCP</td>
    <td></td>
  </tr>
  <tr>
    <td>KSTAR(ksg)</td>
    <td>KSTAR(ksg)</td>
    <td></td>
  </tr>
  <tr>
    <td>Lekong</td>
    <td>Lekong</td>
    <td></td>
  </tr>
  <tr>
    <td>Omnik</td>
    <td>Omnik</td>
    <td></td>
  </tr>
  <tr>
    <td>TaiDa</td>
    <td>TaiDa</td>
    <td></td>
  </tr>
  <tr>
    <td>Taoke</td>
    <td>Taoke</td>
    <td></td>
  </tr>
  <tr>
    <td>SunGrow</td>
    <td>SunGrow</td>
    <td></td>
  </tr>
  <tr>
    <td>Apsystems</td>
    <td>Apsystems</td>
    <td></td>
  </tr>
  <tr>
    <td>YunKong</td>
    <td>YunKong</td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">Trannergy</td>
    <td>Trannergy</td>
    <td></td>
  </tr>
  <tr>
    <td>Trinasolar</td>
    <td></td>
  </tr>
  <tr>
    <td>Solarman</td>
    <td>Solarman</td>
    <td></td>
  </tr>
  <tr>
    <td>Aifu</td>
    <td>Aifu</td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">Dingyang</td>
    <td>dingyangTcp</td>
    <td></td>
  </tr>
  <tr>
    <td>dingyangTcp-jingfuyuan</td>
    <td></td>
  </tr>
  <tr>
    <td>SMA</td>
    <td>SMA</td>
    <td></td>
  </tr>
</table>
