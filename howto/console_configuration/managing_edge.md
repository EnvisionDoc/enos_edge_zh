# 添加Edge

为使用Edge作为网关收集数据以及其他各项功能，你需要在 **Edge网关 > Edge管理** 中添加该Edge，并配置相应的通信参数。

## 开始前准备

- 有Edge管理操作权限，如果没有需联系组织管理员添加。有关EnOS内的用户权限，参见[策略，角色，与权限](/docs/iam/zh_CN/2.0.9/access_policy)。
- 完成Edge模型、产品和设备的创建。参见[设备管理](/docs/device-connection/zh_CN/2.0.9/device_management_overview)。Edge模型的模型名称必须为 **EnOS_Edge_Standard_Model**， Edge产品的产品名称必须为 **EnOS_Edge_Standard_Product**。

## 添加Edge实例

1. 在 **设备管理 > 产品管理** 中，创建一个产品，其具体属性如下表：

  .. csv-table:: EnOS Edge产品
     :header: "属性", "值"
     :widths: auto

     "产品名称", "EnOS_Edge_Standard_Product"
     "节点类型", "网关"
     "设备模型", "EnOS_Edge_Standard_Model"
     "数据格式", "JSON"
     "证书双向认证", "可根据实际需要决定是否启用"
     "产品描述", "根据实际情况填写"

 有关创建产品的更多信息，参见[创建产品](/docs/device-connection/zh_CN/2.0.9/howto/device/manage/creating_product)。

2. 在 **设备管理 > 设备资产** 中，创建一个设备。

  该设备的 **产品** 必须选择 **EnOS_Edge_Standard_Product**。

  有关创建设备的更多信息，参见[创建设备](/docs/device-connection/zh_CN/2.0.9/howto/device/manage/creating_device)。

3. 选择 **EnOS Edge > Edge管理** ，点击 **添加** ，在“添加Edge”窗口的列表中，从已创建好的Edge设备中，选择需要添加的Edge设备，点击**确定**。

## 将Edge相关配置部署到Edge端

在 **Edge管理** 页面，点击 **发布** 旁边的 |download_box_info| 按钮，选择 **下载配置（主）** 。 将下载的配置文件交给Edge的实施人员，让他们将配置文件部署到Edge设备上。让Edge设备能够与EnOS Cloud连接通信。

.. |download_box_info| image:: ../../media/button_download_box_info.png

## 添加连接

添加Edge设备后，下一步为Edge添加连接。在添加好的Edge设备的操作栏，点击 **查看** ，选择 **接入管理** ，可以在“以太网”和“串口”两个标签页根据需要，点击 **添加连接** 。

### 添加以太网连接

点击 **添加连接** 后，按界面提示填入字段。其中：

.. csv-table::
   :widths: auto

   "字段", "说明"
   "是否为短连接", "默认为长连接"
   "地址（主）", "不同模式下Edge与子设备通信所需要的地址。填写方法见下文说明。"
   "地址（备）", "当主地址异常时用于通信的地址。**HTTP(s)** 和 **其他** 模式暂不支持备用地址。其他填写方法与主地址相同。"


地址（主）在不同模式下的含义如下：

- TCP/IP客户端：此模式下Edge作为客户端，子设备作为服务器。因此需要填写作为服务器的子设备的IP和端口号。
- TCP/IP服务端：此模式下Edge作为服务器，子设备作为客户端。因此需要填写作为客户端的子设备的IP和端口号。
- HTTP(s)客户端：此模式下Edge作为客户端，子设备作为服务器。因此需填写作为服务器的子设备的主地址。
- HTTP(s)服务端：此模式下Edge作为服务器，子设备作为客户端。因此需填写作为客户端的子设备的主地址。
- UDP：需要分别填写Edge和子设备双方的主地址；
- 其他：面向特殊模式的连接，当存在不属于上述各模式的连接时，请咨询Edge团队该参数的具体填写方式。

### 添加串口连接

按界面提示，根据串口连接的实际情况填入相关字段即可。

## 添加设备

添加完连接之后，你应当在连接下添加设备。

单击以展开创建好的连接，选择 **添加设备** 。按照页面提示完成添加操作。

### 配置逻辑地址或偏移量

由于一条连接下接入了多个设备，因此需为每个设备配置其逻辑编号及各类点的偏移量。其配置方法取决于所采用的通信规约及其设置。可以逐个设备进行配置，也可导出设备连接信息表线下配置完成后再导入系统，实现批量配置。

.. note:: 导出的表中支持AI，DI，PI，AO，DO，PO偏移量的配置。基本的配置方式为用短划线连接，如0-50，当同时存在多个偏移量时，可用\#隔离，如0-50\#1000-1050。


## 转发数据

你也可以根据需求，将Edge收集到的数据转发给第三方系统。设置数据转发的步骤与连接Edge和EnOS的步骤类似，包括添加连接、添加设备、配置连接相关参数。EnOS支持以下使用以下规约进行数据转发：

- IEC104

### 使用IEC104转发数据

1. 点击 **添加设备** ，按界面提示填入需要的字段即可。其中：

   - 地址：配置方式与 **接入管理** 中，添加连接时的方式相同。


2. 点击连接，选择 **添加设备** ， 使用设备模型和产品筛选处需要添加的设备，点击 **保存** ；

3. 为设备选择转发模板，点击 **保存**

在完成添加设备后，系统会根据添加设备的顺序为设备自动分配序号，当然，用户也可修改设备序号。

.. note:: 设备的序号为从1开始的自然数，且在一个连接下必须连续且不可重复。

## 发布配置

点击 **Edge管理** 页面的 **发布** ，即可配置发布到对应的Edge设备或服务器上使配置生效。

## 通信测试

在用户完成添加连接、添加设备并完成相关配置发布后，可通过通信测试功能测试该连接或端口。点击连接右侧的 |test| 按钮进行测试。

.. |test| image:: ../../media/button_test.png
   :alt: 图：测试按钮

你可以用测试功能查看本连接下各以太网、串口连接的情况，包括以下信息：

- 数据：本连接下的各设备测点的实时数据和处理后的测点数据，用户也可以在数据标签页下使用置数功能
- 原始报文：本连接下的原始通信报文
- 日志：本连接下的通信日志，只显示Warning和Error类型的日志

### 置数

点击 **数据** 标签页的 **置数** ，并设置一个值，点击 **发送** 即可让设备发送该数值作为模拟测点数据至EnOS，置数功能不会打断实时数据的上送，只是在上送数据流中插入一个你设置的数据。

.. |view| image:: ../../media/button_view.png

### 控制台

固化了常用的通信调试命令，包括基本ping测试，本机IP查看，Telnet命令和TCP连接查看命令。

其中，ping测试需在输入框中填写需要ping的IP地址；Telnet测试需填写IP和端口号。

### 单台设备的通信测试

在 **接入管理** 标签页，点击单设备的 |view| 按钮，可对单台设备进行置数测试。此功能与批量通常测试中的置数功能一致，只是这里仅对单台设备进行测试。

.. |view| image:: ../../media/button_view.png

## 资产树

你可以在 **Edge详情 > 资产树** 页面，管理Edge及其子设备。在Edge上进行资产树管理的相关概念和操作与EnOS相同，参见[资产树管理](/docs/device-connection/zh_CN/2.0.9/howto/asset_tree/index)。

需要指出的是，Edge详情页的资产树显示的不是整个OU所有资产树及其所有节点，而只显示当前Edge下子设备挂载的资产树及当前Edge的子设备所在节点。


<!--end-->
