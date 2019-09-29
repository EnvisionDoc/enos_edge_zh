# 通过EnOS Edge将设备连接至EnOS

本教程会引导你逐步了解如何通过EnOS Edge将设备连接到EnOS Cloud。

作为软件，EnOS Edge可以部署在多种硬件上。在本教程中，EnOS Edge软件安装在Dell Edge Gateway 3000上。故本教程以Edge软件已经正确安装为前提，因此不包括安装过程。有关如何安装，参见[安装指南](../howto/edge_installation_guide/index)。

## 应用场景

本教程适用的场景如下：

- 在笔记本电脑上安装了IEC104模拟器，用以模拟通过IEC104协议进行通信的三个电表。

- 作为子设备连接到EnOS Edge的模拟设备。

- EnOS Edge已连接到网络，并通过标准协议MQTT与EnOS Cloud通信。


## 步骤1：定义模型

模型是连接到EnOS的对象功能的抽象。它定义了设备的功能，包括属性，测点，服务和事件。有关模型的更多信息，参见[物模型](/docs/device-connection/zh_CN/2.0.9/howto/model/model_overview)。

EnOS Edge的模型已作为EnOS的公共模型提供。进入 **模型 > 公有模型**，搜索 *EnOS_Edge_Standard_Model*，然后点击 **查看** 查看其基本信息和功能定义。

在此步骤中，需要先为模拟的子设备创建模型，然后才能将其连接到EnOS。


1. 进入 **模型**，在 **私有模型** 标签下点击 **创建模型**。

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

<!--以上字段都可省略“模型”-->

3. 点击 **TrainingMeter** 模型 **操作** 列中的 **编辑**。进入 **功能定义** 标签，按照以下步骤分别创建4个测点：

   1. 点击 **新增**。

   2. 填写弹出窗口中所需的字段。

   3. 点击 **确认**。

   4个测点的信息如下：<!--测点的英文需统一为measurement point，英文文档里写的是measuring point-->

   .. csv-table:: 测点信息
      :header: "测点", "功能类型", "名称", "标识符", "测点类型", "是否有质量位", "数据类型", "默认值", "单位", "是否必填", "描述"
      :widths: auto

      "1", "测点", "Ua", "Meter.Ua", "AI", "无", "double", "电压：伏特 | V", "留空"
      "2", "测点", "P", "Meter.ActivePower", "AI", "无", "double", "功率：千瓦 | kW", "留空"
      "3", "测点", "Q", "Meter.ReactivePower", "AI", "无", "double", "功率：千乏 | kVar", "留空"
      "4", "测点", "Ia", "Meter.Ia", "AI", "无", "double", "电流：安培 | A", "留空"

   .. image:: ../media/tutorial_create_measuring_points.png


现在，你已为即将连接到Edge的模拟子设备创建了模型。

## 步骤2：创建产品

完成了模型定义后，需要基于设备和Edge模型创建产品。

产品是具有相同功能的设备的集合。当模型从4个方面（属性，测点，服务和事件）抽象设备功能时，产品会进一步定义设备的通信参数。

1. 进入 **设备管理 > 产品**，点击 **创建产品**。

2. 在 **创建产品** 窗口，填写如下字段：
  
   .. csv-table:: 虚拟设备的产品信息
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

The Edge and sub-device instances are now in the device list.

.. image:: ../media/tutorial_device_list.png

## Step 4: Creating Asset Tree and Asset for Edge

For Edge to function properly, we need to register the edge device in **Asset Tree**. To do that, we will first create an asset tree and bind the created Edge device to the tree as a node. You need an OU administrator account to create an asset tree.

1. Go to **Asset Tree**. Click |create| in the upper left corner.

   .. |create| image:: ../media/button_new_asset_tree.png

2. In the **Create Asset Tree** pop-up, fill in the fields as follows:

   .. csv-table:: Asset Tree Information for Edge
      :header: "Field", "Value"
      :widths: auto

      "Name", "Edge02"
      "Select Model", "EnOS_Edge_Standard_Model"
      "Time Zone/City", "UTC+08:00"
      "Description", "Leave it blank"
      "edge_type", "0: EdgeGateway"

   .. image:: ../media/tutorial_creating_asset_tree.png

3. Click the **+** to the right of the asset tree name "Edge02" in the left panel.

4. In the **New Sub-node** pop-up, select **Bind to Existing Asset** and click **Next**.

5. In the device list, search for the Edge device we just created "Edge01", check the box in front of it, and click **Confirm**.

   The Edge device has been bound to the asset tree as a sub-node.

   .. image:: ../media/tutorial_asset_tree_overview.png

## Step 5: Creating Template for Edge

The template determines how data collected from a device by using a protocol is mapped into the model measuring points defined on EnOS cloud.

1. Go to **EnOS Edge > Template**. Click **New Template**.

2. In the **Add Template** pop-up, fill in the fields as follows:

   .. csv-table:: Asset Tree Information for Edge
      :header: "Field", "Value"
      :widths: auto

      "Template Name", "meter"
      "Brand", "Envision (or any brand as you see fit)"
      "Device Type", "Envision sample (or any type as you see fit)"
      "Version", "v1.0 (or any random version number)"
      "Model", "TrainingMeter"

   .. image:: ../media/tutorial_add_template.png

3. Find the template you just created, click |edit| in the **Operations** column.

   .. |edit| image:: ../media/button_edit.png

4. In the **Edit Template** pop-up, click |download|. Click |download2| to download *point.csv* of "IEC104-Client-linux v3.0_debug" as this is the protocol we are going to use for this tutorial.

   .. |download| image:: ../media/button_download_template.png
   .. |download2| image:: ../media/button_download.png

5. Open *point.csv* you just downloaded with a text editor. We strongly recommend you use Notepad++ to avoid any garbled code.

   *point.csv* contains the attributes of collection and control points to be reported to Edge. Since we have defined 4 measuring points for the device models in **Step 1**, we need to create 4 collection points so that they can be mapped to the 4 measuring points later.

   Originally, *point.csv* looks like the following, the Chinese characters seperated by commas on the first line means measuring point, point number, value type, point type, coefficient, base value, and alias, from left to right:

   .. image:: ../media/tutorial_point_csv_original.png

   Let's edit *point.csv* so that it looks like this:

   .. image:: ../media/tutorial_point_csv_edited.png

   As you can see, we changed the measuring point, value type, and point type to match the "Identifier", "Data Type", and "Point Type" defined in the model in **Step 1**. 
   
   .. note:: As for point numbers, they must be continuous positive integers and follow the order in which the collection points are sent to EnOS as prescribed in the channel table.

6. Save and close *point.csv*.

7. Go back to **EnOS Edge > Template > Edit Template**. Click |upload| to upload *point.csv* you just edited.

   .. |upload| image:: ../media/button_upload.png

8. In **Edit Template** page, click |point_mapping| for each measuring point. 

   .. |point_mapping| image:: ../media/button_point_mapping.png

9. In the pop-up, select the collection point in **Collect** tab whose description matches the measuring point's identifier and click **Confirm**.

   By doing this, you map the measuring points defined in our model to the collection points defined in *point.csv*.

   .. image:: ../media/tutorial_point_mapping.png

10. Repeat step 8 and 9 until all the measuring points are properly mapped.

By now we have completed configuring our template. This means when we set up our Edge and connects it to EnOS cloud and sub-devices, the collection points from the simulated sub-devices can be properly mapped to the measuring points defined in the model.

## Step 6: Configuring Edge

In this step, we will add the Edge device we created in **Device Management** to **Edge Management**, configure its connection with sub-devices, and configure the offset value for the measuring points of each sub-device.

1. Go to **EnOS Edge > Edge Management**. Click **New Edge**.

2. In the **New Edge** pop-up, select the Edge we added and click **Confirm**.

3. Click **View** in **Operations** column of the Edge you added.

4. In **Edge Detail**, click |download_box_info| beside the **Publish** button and select **Download Edge Info(Main)** to download the configuration file for the Edge device.

   .. |download_box_info| image:: ../media/button_download_box_info.png
   .. image:: ../media/tutorial_download_box_info.png

5. Give the downloaded *box.conf* to an EnOS Edge support personnel and let the support personnel configure the Edge device with *box.conf*. You don't need to do this step yourself.

   After the support personnel has properly configured the Edge device, it can connect to EnOS once it is powered on.

6. In **Edge Detail**, go to **Access Management** tab. Click **Add Connection** in **Ethernet** tab.

7. In the **Add Connection** pop-up, fill in the fields as follows:

   .. csv-table:: Asset Tree Information for Edge
      :header: "Field", "Value"
      :widths: auto

      "Name", "Meter_Training"
      "Mode", "TCP/IP Client"
      "Short Connection", "Disabled"
      "Primary Interface", "The IPv4 Address of your laptop. For port you can give it a random integer larger than 1000, this number included."
      "Standby Interface", "Leave it blank"
      "Protocol Type", "IEC104"
      "Protocol Version", "IEC104-Client-linux v3.0_debug"
      "Configuration File", "No operation is needed in this case. But you might have to modify configuration if you use other protocols or as required."

   .. image:: ../media/tutorial_add_connection.png

8. Click **Confirm**. 

   You have now created a connection for the Edge device.

9. Click to expand the connection you just created. 

10. Click **Add Device**. In **Add Device** window, select the product **training_meter** we just created, and check all three devices meter01, meter02, and meter03.

11. Under **Template Setting**, select the template we created for these device, **meter**.

12. Click **Save**.

13. Click to expand the connection. In the device list, click |edit| in the **Operation** column of any device.

14. In the **Edit Device** pop-up, set the **AI Offset** in format "a-b#c-d", a, b, c, d representing integers and a < b, c < d. "-" indicates that this is a value range, while # indicates that the range can be discontinuous.

   The offset value range is used to identify which values in the simulation scheme file belong to this device. 

   We need to do a little bit math here. Since there are 4 measuring points we need to map to every device, we must make sure that there are 4 integers in the value ranges of one device, both end of a range included.

   In this example, in order to have four integers for one device. We can set the **AI Offset** as "2-5" or "2-3#6-7". We choose to use "2-5" for convenience's sake.

15. Click **Confirm**.

16. Repeat step 14 and 15 for all remaining devices, all the four AI offset ranges being unique, not overlapping any other.

## Step 7: Installing and Configuring IEC104 Protocol Simulator

We are almost there! In this step, we will install a protocol simulator, which simulates devices sending measuring point data in IEC104 protocol to Edge, on our laptop. And then we will configure how this simulator sends data to EnOS and finally, kick off the simulation.

1. Download the [IECServer.exe](../_static/IECServer.exe) and [iec_ai_12.properties](../_static/iec_ai_12.properties).

2. Open *iec_ai_12.properties* with a text editor (again we recommend Notepad++).

   Each item in this file represents a measuring point. As we have 4 measuring points each for 3 electric meters, the total measuring points is 12, as there are in this file.

   We need to change their "IOB" values so that they can be properly mapped to measuring points of each devices.

   We divide the items into 3 groups: ITEM1 to ITEM4; ITEM5 to ITEM8, ITEM9 to ITEM12, each group representing the 4 measuring points of an electric meter.

   Let's say ITEM1 to ITEM4 are measuring points of meter01, whose AI offset is 2-5 as configured in Step 6. Then the IOB values of ITEM1 to ITEM4 should be 16385+2, 16385+3, 16385+4, 16385+5, i.e., 16387, 16388, 16389, 16390.

   Repeat this step for all items in this configuration file. Save your changes.

3. Open **IECServer.exe**, click **Load**, locate and select the configuraion file you just edited, click **Open**.

   You loaded the configuraion file into the simulator.

4. Set the port number as the same port you set to the address of your Edge connection. In our case, it's 2404.

   You can find the port number as follows:
   
   1. Go to **EnOS Edge > Edge Management**.
   
   2. Click **View** to go to the **Edge Detail** page of the Edge you created.

   3. Go to **Access Management** tab, click to expand the connection. You can see the port number in the **Address** field.

5. Click **StartServer** to start simulation.

   .. image:: ../media/tutorial_iec104_simulator.png

## Step 8: Viewing the Data on EnOS Console

Now let's go to **EnOS Edge > Edge Management**, click **View** and go to **Access Management** tab. Click to expand our connection. You can see that the devices have a green light in front of them, indicating a successful connection.

.. image:: ../media/tutorial_connection_success.png

Click |view| for any device to view the data that is coming from our simulator.

.. |view| image:: ../media/button_view.png

.. image:: ../media/tutorial_device_data.png

Congratulations! You have learnt how to connect an Edge device to EnOS, and use a simulator to simulate devices transferring data through Edge to EnOS cloud.
