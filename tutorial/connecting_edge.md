# 通过EnOS Edge将设备连接至EnOS

本教程会引导你逐步了解如何通过EnOS Edge将设备连接到EnOS Cloud。

作为软件，EnOS Edge可以部署在多种硬件上。在本教程中，EnOS Edge软件安装在Dell Edge Gateway 3000上。本教程中Edge在硬件上的部署（见步骤6），无需用户操作，有关Edge的部署，请联系EnOS Edge部署人员。

## 场景

本教程场景如下：

- 使用IEC104规约模拟器，用一台笔记本电脑模拟出三个电表设备。

- 配置Edge让其从模拟的电表设备处收集测点信息，然后发送至EnOS云端。

- 使用笔记本电脑登录EnOS，在EnOS控制台观察模拟电表设备上报的数据。

.. image:: ../media/tutorial_blueprint.png

## 步骤1：定义模型

模型是连接到EnOS的对象功能的抽象。它定义了设备的功能，包括属性，测点，服务和事件。有关模型的更多信息，参见[物模型](/docs/device-connection/zh_CN/2.0.9/howto/model/model_overview)。

EnOS Edge的模型已作为EnOS的公共模型提供。进入 **模型 > 公有模型**，搜索 *EnOS_Edge_Standard_Model*，然后点击 **查看** 查看其基本信息和功能定义。

在此步骤中，需要先为模拟的子设备创建模型，然后才能将其连接到EnOS。


1. 进入 **模型**，在 **私有模型** 选项卡点击 **创建模型**。

2. 在 **创建模型** 窗口，填写如下字段：

   .. csv-table:: 模型信息
      :header: "字段", "值"
      :widths: auto

      "模型标识符", "TrainingMeter"
      "模型名称", "TrainingMeter"
      "分类", "Training"
      "模型关系", "无"
      "模型模板", "无"
      "模型描述", "Meter model for training"

   .. image:: ../media/tutorial_creating_model.png


3. 点击 **TrainingMeter** 模型 **操作** 列中的 **编辑**。进入 **功能定义** 选项卡，按照以下步骤分别创建4个测点：

   a. 点击 **新增**。
   b. 填写弹出窗口中所需的字段。
   c. 点击 **确认**。

  4个测点的信息如下：

  .. csv-table:: 测点信息
     :header: "测点", "功能类型", "名称", "标识符", "测点类型", "是否有质量位", "数据类型", "默认值", "单位", "是否必填", "描述"
     :widths: auto

     "1", "测点", "Ua", "Meter.Ua", "AI", "无", "double", "电压：伏特 | V", "留空"
     "2", "测点", "P", "Meter.ActivePower", "AI", "无", "double", "功率：千瓦 | kW", "留空"
     "3", "测点", "Q", "Meter.ReactivePower", "AI", "无", "double", "功率：千乏 | kVar", "留空"
     "4", "测点", "Ia", "Meter.Ia", "AI", "无", "double", "电流：安培 | A", "留空"


  .. image:: ../media/tutorial_create_measuring_points.png

这四个测点分别代表电表的A相电压、交流电功率、无功功率和A相电流。

现在，你已为即将连接到Edge的模拟子设备创建了模型。

## 步骤2：创建产品

完成了模型定义后，需要基于设备和Edge模型创建产品。

产品是具有相同功能的设备的集合。当模型从4个方面（属性，测点，服务和事件）抽象设备功能时，产品会进一步定义设备的通信参数。

1. 进入 **设备管理 > 产品**，点击 **创建产品**。

2. 在 **创建产品** 窗口，填写如下字段：

   .. csv-table:: 模拟设备的产品信息
      :header: "字段", "值"
      :widths: auto

      "产品名称", "training_meter"
      "节点类型", "设备"
      "设备模型", "TrainingMeter"
      "数据格式", "Json"
      "证书双向认证", "禁用"
      "产品描述", "Electric meter for training"

   .. image:: ../media/tutorial_creating_product_for_device.png

这样，模拟电表的产品就创建完成了。现在，可以对Edge设备进行相同的操作。

.. image:: ../media/tutorial_creating_product_for_edge.png

下面可以开始准备在EnOS上创建设备实例。

## 步骤3：创建设备

设备是由产品创建而来，因此它不仅继承了模型的基本功能，还继承了产品的通信参数（如，用于认证的产品密钥和产品密钥）。

此步骤会在EnOS上为Edge及其子设备创建设备。

1. 进入 **设备管理 > 设备管理**，点击 **添加设备**。

2. 在 **添加设备** 窗口，填写以下字段:

   .. csv-table:: Edge设备信息
      :header: "字段", "值"
      :widths: auto

      "产品", "EnOS_Edge_Standard_Product"
      "Device Key", "留空"
      "设备名称", "Edge01"
      "时区/城市", "UTC+08:00"
      "edge_type", "0: EdgeGateway"

   .. image:: ../media/tutorial_creating_device_for_edge.png

这样，Edge的设备实例就创建完成。现在，对子设备执行相同操作。在这种情况下，使用笔记本电脑模拟将要连接到Edge的三个电表，将它们命名为“meter01”，“meter02”和“meter03”。它们的其他属性如下所示：

.. csv-table:: 子设备（电表）信息
   :header: "字段", "值"
   :widths: auto

   "产品", "training_meter"
   "Device Key", "留空"
   "设备名称", "meter01, meter02, and meter03"
   "时区/城市", "UTC+08:00"

<!--The end-->

现在，Edge和它的子设备实例都能在设备列表中查看了。

.. image:: ../media/tutorial_device_list.png

## 步骤4：为Edge创建资产树与资产

为了使Edge能够正常运行，需要在 **资产树** 中注册Edge设备。为此，需要首先创建一个资产树，并将创建的Edge设备作为节点绑定到该树。你需要获得OU管理员权限才能创建资产树。

1. 进入 **资产树**，点击左上角的 |create|。

   .. |create| image:: ../media/button_new_asset_tree.png

2. 在 **创建资产树** 弹出窗口中填写以下字段：

   .. csv-table:: Edge资产树信息
      :header: "字段", "值"
      :widths: auto

      "资产树名称", "Edge02"
      "选择模型", "EnOS_Edge_Standard_Model"
      "时区/城市", "UTC+08:00"
      "时区/城市", "留空"
      "edge_type", "0: EdgeGateway"

   .. image:: ../media/tutorial_creating_asset_tree.png

3. 点击左侧面板上“Edge02”右侧的 **+** 按钮。

4. 在 **添加子节点** 弹出窗口中选择 **关联已有资产**，然后点击 **下一步**。

5. 在设备列表中，搜索刚刚创建的Edge设备“Edge01”，选中它后点击 **确认**。

   这样，Edge设备就作为子节点绑定到资产树了。

   .. image:: ../media/tutorial_asset_tree_overview.png

## 步骤5：创建设备模板以映射数据点

设备模板规定了通过协议从设备收集的数据与EnOS云上定义的模型测量点的映射关系。

1. 进入 **EnOS Edge > 模板配置**，点击 **添加**。

2. 在 **添加模板** 弹出窗口中填写以下字段：

   .. csv-table:: Edge模板信息
      :header: "字段", "值"
      :widths: auto

      "模板名称", "meter"
      "设备品牌", "Envision（或任何合适的品牌）"
      "设备型号", "Envision sample（或任何合适的型号）"
      "版本", "v1.0（或任何随机版本号）"
      "设备模型", "TrainingMeter"

   .. image:: ../media/tutorial_add_template.png

3. 找到创建的模板，点击其 **操作** 列中的 |edit|。

   .. |edit| image:: ../media/button_edit.png

4. 在 **编辑模板** 弹出窗口中，点击|download|。然后点击“IEC104-Client-linux v3.0_debug”后的|download2|来下载`point.xlsx`。这是将在本教程中使用的协议。<!--建议页面名称改为“编辑模板”-->

   .. |download| image:: ../media/button_download_template.png

   .. |download2| image:: ../media/button_download.png

5. 打开刚刚下载的 `point.xlsx`。

   `point.xlsx` 中包含要报告给Edge的采集点和控制点的属性。由于在 **步骤1** 中已为设备模型定义了4个测点，因此需要创建4个采集点，以便将它们映射到4个测点。

   `point.xlsx` 初始文件如下所示，第一行从左至右的字段表示测点、点号、值类型、点类型、系数、基值和别名：

   .. image:: ../media/tutorial_point_csv_original.png

   编辑 `point.xlsx` 使之如下所示：

   .. image:: ../media/tutorial_point_csv_edited.png

   如上图所示，测点、值类型和点类型都被更改，以匹配 **步骤1** 中的模型所定义的“标识符”、“数据类型”和“测点类型”。

   .. note:: 关于点号，它们必须是连续的正整数，并遵循通道表中规定的将收集点发送到EnOS的顺序。<!--这个channel table在哪里？-->


6. 保存并关闭 `point.xlsx`。

7. 返回到 **EnOS Edge > 模板配置 > 模板编辑**。点击 |upload| 来上传 *point.xlsx*。

   .. |upload| image:: ../media/button_upload.png

8. 在 **模板编辑** 页面，点击每个测点后的 |point_mapping|。

   .. |point_mapping| image:: ../media/button_point_mapping.png

9. 在弹出窗口的 **采集** 选项卡下，选择其描述与测点标识符匹配的采集点，然后点击 **确定**。

   这样，你就将模型中定义的测点和 *point.xlsx* 中定义的采集点建立了映射关系。

   .. image:: ../media/tutorial_point_mapping.png

10. 重复第8及第9步，直到完成所有测点的映射。点击 **保存**。

至此，模板已配置完成。这表示当我们设置Edge并将其连接到EnOS云和子设备时，来自模拟子设备的采集点能够被正确地映射到模型定义的测点上。

## 步骤6：配置Edge

在本步骤中，我们把在 **设备管理** 中创建的Edge设备添加到 **Edge管理** 中，配置其与子设备的连接，并为每个子设备的测点配置偏移值。

1. 进入 **EnOS Edge > Edge管理**，点击 **添加**。

2. 在 **添加 Edge** 弹出窗口中选择已经添加的Edge并点击 **确定**。

3. 在添加的Edge的 **操作** 栏中点击 **查看**。

4. 在 **Edge详情** 中点击 **发布**旁的 |download_box_info| 并选择 **下载配置（主）** 来下载Edge的配置文件。

   .. |download_box_info| image:: ../media/button_download_box_info.png

   .. image:: ../media/tutorial_download_box_info.png

5. 将下载的 *box.conf* 交给EnOS Edge支持人员，让支持人员使用*box.conf* 配置Edge设备。你无需自己执行此步骤。

   在支持人员正确配置Edge设备后，一旦打开Edge的电源，它就可以连接到EnOS。

6. 在 **Edge详情** 页面，进入 **接入管理** 选项卡。点击 **以太网** 选项卡下的 **添加连接**。

7. 在 **添加连接** 弹出窗口中填写以下字段：

   .. csv-table:: Edge资产树信息
      :header: "字段", "值"
      :widths: auto

      "连接名称", "Meter_Training"
      "模式", "TCP/IP Client"
      "是否为短连接", "关闭"
      "地址（主）", "即笔记本电脑的IPv4地址。对于端口，你可以输入任意大于或等于1000整数。"
      "地址（备）", "留空"
      "规约类型", "IEC104"
      "规约", "IEC104-Client-linux v3.0_release"
      "配置文件", "在此种情况下，无需任何操作。但若使用其它协议，则可能需要修改配置。（或按需修改）"

   .. image:: ../media/tutorial_add_connection.png

8. 点击 **确定**。

   现在，你已为Edge设备创建了连接。

9. 点击以展开创建完成的连接。

10. 点击 **添加设备**。 在 **添加设备** 窗口中，选择刚刚创建的产品 **training_meter**，然后勾选所有三个设备：电表01、电表02和电表03。

11. 在 **设备模板设置** 下，选择为以上设备创建的模板 **meter**。

12. 点击 **保存**。

13. 点击以展开连接。在设备列表中点击任一设备 **操作** 列中的 |edit|。

  |edit| image:: ../media/button_edit.png

14. 在 **修改设备** 弹出窗口中，将 **AI偏移量** 设置为“a-b#c-d”的格式，a、b、c、d表示整数且a < b，c < d。 “-”表示值范围，“#”则表示该范围可以不连续。

   偏移值范围用于识别模拟方案文件中的哪些值属于该Edge设备。

   此处需要做一些运算。由于对每台设备都要映射4个测点，因此必须确保单个设备的值范围内有4个整数，包括最大与最小值。

   在此示例中，为了使单个设备具有四个整数，可将 **AI偏移量** 设置为 “2-5”或“2-3#6-7”。方便起见，我们使用“2-5”。

15. 点击 **确定**。

16. 对其余设备重复14和15步。所有四个AI偏移范围都是唯一的，不会重叠。

## 步骤7：安装即配置IEC104规约模拟器

在本步骤中，我们将在笔记本电脑上安装协议模拟器，用以模拟将IEC104规约中的测点数据发送到Edge的设备。然后，我们将配置此模拟器将数据发送至EnOS，最后进行模拟。

1. 下载 [IECServer.exe](../_static/IECServer.exe) 和 [iec_ai_12.properties](../_static/iec_ai_12.properties)。（鼠标右键点击下载文件，选择“链接另存为”）

2. 使用文本编辑器打开 *iec_ai_12.properties* （推荐使用Notepad++）。

   该文件中的每个项都代表一个测点。由于有3个电表，每个电表有4个测点，所以如该文件所示，总测点为12。

   修改“IOB”值，以便将其正确映射到每个设备的测点。

   将所有项目分为3组：ITEM1至ITEM4； ITEM5至ITEM8，ITEM9至ITEM12，每组代表一个电表的4个测点。

   假设ITEM1至ITEM4是电表01的测点，其AI偏移量为步骤6中配置的“2-5”。那么ITEM1至ITEM4的IOB值应为16385+2、16385+3、16385+4、16385+5， 即16387、16388、16389、16390。“16385”是IEC104规约 *protocol.sys* 中规定的AI数据的起始地址。

   对此配置文件中的所有项目重复以上步骤。然后保存更改。

3. 打开 **IECServer.exe**，点击 **Load**，找到并选择刚刚编辑的配置文件，然后单击 **打开**。

   这样就将配置文件加载到模拟器中。

4. 在按钮 **StartServer** 旁的文本框内输入与子设备连接地址相同的端口。在我们的例子中是“2404”。

   可以通过以下步骤获取端口信息：

   1. 进入 **EnOS Edge > Edge管理**。

   2. 点击创建的Edge设备的 **查看** 进入 **Edge详情** 页面。

   3. 进入 **接入管理** 选项卡，在连接的 **地址** 项中即可查看到端口信息。

5. 点击 **StartServer** 来运行模拟器。

   .. image:: ../media/tutorial_iec104_simulator.png

## 步骤8：在EnOS控制台查看数据

现在，进入 **EnOS Edge > Edge管理**，点击Edge01设备的 **查看**，再进入 **接入管理** 选项卡。点击展开连接可以看到设备前的绿灯，这表明连接已成功。

.. image:: ../media/tutorial_connection_success.png

点击任一设备的 |view| 都可以查看到来自模拟器的数据。

.. |view| image:: ../media/button_view.png

.. image:: ../media/tutorial_device_data.png

恭喜！你已了解如何将Edge设备连接到EnOS，并使用模拟器来模拟通过Edge将设备数据传输到EnOS云了。
