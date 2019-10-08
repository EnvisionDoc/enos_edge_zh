# Connecting Edge to EnOS and Simulated Sub-devices

The prerequisite to using Edge is connecting Edge to EnOS cloud and the sub-devices. This tutorial uses Edge installed on a Dell Edge Gateway 3000, a protocol simulator program, and EnOS to explain:

- How to connect Edge to EnOS cloud
- How to use your laptop to simulate sub-devices
- How to connect Edge to simualted sub-devices

## Step 1: Defining Models

A model is the abstraction of the features of an object connected to EnOS. It defines the features of a device, including attributes, measuring points, services, and events. For more information about models, see [Thing Model](/docs/device-connection/en/2.0.9/howto/model/model_overview).

The model of Edge has been provided by EnOS as a public model. Go to **Public** tab in **Model** and search for *EnOS_Edge_Standard_Model*. You can view its basic information and feature definitions in by clicking **View** .

In this step, we need to create models for the simulated sub-devices before they can be connected to EnOS. 

1. Go to **Model** . In **Private** tab, click **New Model** .

2. In the **New Model** pop-up, fill in the fields as follows:
  
   .. csv-table:: Model Information
      :header: "Field", "Value"
      :widths: auto

      "Identifier", "TrainingMeter"
      "Model Name", "TrainingMeter"
      "Catetory", "Training"
      "Created From", "No"
      "Source Model", "No"
      "Description", "Meter model for training"

   .. image:: ../media/tutorial_creating_model.png

3. Click **Edit** in the **Operations** column for **TrainingMeter**. Go to **Feature Definition** tab and create 4 measuring points by doing the following for every measuring point:

   1. Click **Add**.

   2. Fill in the fields as described below in the pop-up.

   3. Click **Confirm**.

   The information of the 4 measuring points are as follows:

   .. csv-table:: Measuring Point Information
      :header: "Measuring Point", "Feature Type", "Name", "Identifier", "Point Type", "Quality Indicator", "Data Type", "Default Value", "Unit", "Required", "Description"
      :widths: auto

      "1", "Measurement Points", "Ua", "Meter.Ua", "AI", "No", "double", "Electric Potential: volt | V", "Leave it blank"
      "2", "Measurement Points", "P", "Meter.ActivePower", "AI", "No", "double", "Power: kilowatt | kW", "Leave it blank"
      "3", "Measurement Points", "Q", "Meter.ReactivePower", "AI", "No", "double", "Power: KiloVolt Ampere Reactive | kVar", "Leave it blank"
      "4", "Measurement Points", "Ia", "Meter.Ia", "AI", "No", "double", "Electric Current: ampere | A", "Leave it blank"

   .. image:: ../media/tutorial_create_measuring_points.png

You have now created the model for simulated sub-devices we will use to connect to Edge. 

## Step 2: Creating Products

Now that we have defined the model, we need to create products based on the device and Edge models. 

A product is a collection of devices with identical features. While a model defines aspects of a concrete device from 4 aspects (atrributes, measuring points, services, and events), a product further defines the communication parameters for a concrete device.

1. Go to **Device Management > Product**. Click **New Product**.

2. In the **New Product** pop-up, fill in the fields as follows:
  
   .. csv-table:: Product Information for Simulated Devices
      :header: "Field", "Value"
      :widths: auto

      "Product Name", "training_meter"
      "Asset Type", "Device"
      "Model", "TrainingMeter"
      "Data Type", "JSON"
      "Certificate-Based Authentication", "Disabled"
      "Description", "Electric meter for training"

   .. image:: ../media/tutorial_creating_product_for_device.png

You have created the product for simulated meters. Now let's do the same thing for the Edge device.

.. image:: ../media/tutorial_creating_product_for_edge.png

Now we are ready to create the concrete devices on EnOS.

## Step 3: Creating Devices

A device instance on EnOS represents a concrete device in the physical world. A device is created from a product so that it inherits not only the basic features of the model, but also the communication parameters of the product (such as the product secret and product secret used for authentication).

In this step we need to create devices on EnOS for Edge and its sub-devices.

1. Go to **Device Management > Device**. Click **New Device**.

2. In the **New Device** pop-up, fill in the fields as follows:
  
   .. csv-table:: Device Information for Edge
      :header: "Field", "Value"
      :widths: auto

      "Product", "EnOS_Edge_Standard_Product"
      "Device Key", "Leave it blank"
      "Device Name", "Edge 01"
      "Time Zone/City", "UTC+08:00"
      "edge_type", "0: EdgeGateway"

   .. image:: ../media/tutorial_creating_device_for_edge.png

A device instance for Edge has been created. Now let's do the same thing for the sub-devices. In this scenario, we will use our laptop to simulate three electric meters that will be connected to Edge. We will name them "meter01", "meter02", and "meter03". Their other attributes are listed as follows:

.. csv-table:: Device Information for Sub-devices (Electric meter)
   :header: "Field", "Value"
   :widths: auto

   "Product", "training_meter"
   "Device Key", "Leave it blank"
   "Device Name", "meter01, meter02, and meter03"
   "Time Zone/City", "UTC+08:00"

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
















